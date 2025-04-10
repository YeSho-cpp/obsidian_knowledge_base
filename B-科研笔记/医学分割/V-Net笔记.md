Vnet是Unet的一个变型，与Unet的不同点：  
1. Vnet针对3D图像用`volumetric convolutions`，不需要对输入集下切片处理  
2. 引入残差结构，加快收敛速度  
3. 卷积层代替池化层  
4. 提出了基于Dice系数最大化的新的目标函数
5. 用`random non-linear transformations` 和 `histogram matching` 扩充数据

# 一、网络结构

![链接](https://img-blog.csdnimg.cn/45d93f035fc2475dbe7bc0d72fd9bac0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARGVtb27mnpw=,size_20,color_FFFFFF,t_70,g_se,x_16)

网络的左边部分由一个压缩路径组成，右边部分对左边的信号进行解码，直到达到原始大小为止。网络中的卷积全部使用了适当的padding，进行尺寸控制，确保输出与输入图像尺寸一致。
网络的左侧分为不同阶段，以不同的分辨率运行。每个阶段由一到三个卷积层（卷积核尺寸为5×5×5）组成，提取图像特征。每个阶段都使用了<font color=#ff0000>残差学习</font>，输入的`feature map`经几次卷积和非线性激活后得到的与输入的feature map逐点相加，在每个阶段结束后进行下采样（卷积核尺寸为2×2×2，stride=2），通过使用适当的步幅降低图像的分辨率，下采样后`feature map`大小减半，特征通道数量加倍。
网络的右侧也分为不同阶段与左侧对称，每个阶段由一到三个卷积层（卷积核尺寸为5×5×5，卷积核数量为上一阶段的一半）组成，在解码过程的每个阶段也使用了参数学习，在每个阶段之后执行转置卷积，将`feature map`大小加倍。
最后使用1×1×1的卷积，产生与输入体积大小相同的两个`feature map`输出，然后通过softmax，输出每个体素属于前景和背景的概率。
Vnet使用了与Unet类似的`skip-connection`结构，收集在压缩路径中会丢失的细粒度细节信息，提高网络的分割精度。

# 二、损失函数
我们感兴趣的解剖一般仅占据很小的扫描区域，这通常会使学习过程陷入损失函数的局部最小值，从而产生一个预测强烈偏向于背景的网络，导致前景区域经常丢失或仅被部分检测。为避免上述情况的发生，本文提出了一个基于dice系数的新的目标函数，它是一个介于0-1之间的量，我们的目标是使其最大化。
骰子系数：
	![链接](https://img-blog.csdnimg.cn/70e9780fb8b444aa9ef536999736fdcf.png)


```python
class InputTransition(nn.Module):
    def __init__(self, outChans, elu):
        super(InputTransition, self).__init__()
        self.conv1 = nn.Conv3d(1, 16, kernel_size=5, padding=2)
        self.bn1 = ContBatchNorm3d(16)
        self.relu1 = ELUCons(elu, 16)

    def forward(self, x):
        # do we want a PRELU here as well?
        out = self.bn1(self.conv1(x))
        # split input in to 16 channels
        x16 = torch.cat((x, x, x, x, x, x, x, x,
                         x, x, x, x, x, x, x, x), 0)
        out = self.relu1(torch.add(out, x16))
        return out
class VNet(nn.Module):
    # the number of convolutions in each layer corresponds
    # to what is in the actual prototxt, not the intent
    def __init__(self, elu=True, nll=False):
        super(VNet, self).__init__()
        self.in_tr = InputTransition(16, elu)
```


> [!note]+
> `InputTransition`的操作将原始的x进行一个卷积通道变为16，进行一个BN,一个ELU，一个残差连接
> `DownTransition`的操作进行一个下采样卷积将通道变成2倍，大小缩小2倍，进行一个下采样卷积、一个BN，一个ELU作为一个原始值；再根据是否dropout，进行dropout处理，那根据ncovs的数量，进行ncovs个序列操作(一个`[通道不变大小不变]`卷积，一个BN，一个ELU),得到结果，结果值与原始值进行残差后ELU
> `UpTransition` 进行条件进行1或者2次dropout,上卷积操作会使图片大小变2，通道减半，进行一个序列(一个上卷积，一个BN，一个ELU)，经结果与输入的中间层拼接，通道数就加倍了，再根据ncovs的数量，进行ncovs个序列操作(一个`[通道不变大小不变]`卷积，一个BN，一个ELU),得到结果，结果值与原始值进行残差后ELU
> 