本文提出的方式是使用带有`soft attention`的Unet，通过下一级的`feature`来<font color=#ff0000>监督</font>上一级的feature来实现attention机制（抑制输入图像中的不相关的区域，突出特定局部区域的显著特征。
**网络结构**
相对于原始版本的Unet，作者提出了一种Attention Gate结构，AG接在每个跳跃连接的末端，对提取的feature实现attention机制。

![链接](https://img-blog.csdnimg.cn/37c9daefaa184ece9f79f36fd62db870.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARGVtb27mnpw=,size_20,color_FFFFFF,t_70,g_se,x_16)

Attention Gate的具体结构如下：

![链接](https://img-blog.csdnimg.cn/9e15a871cdf3405ebcf1ba405ed1a7e5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBARGVtb27mnpw=,size_20,color_FFFFFF,t_70,g_se,x_16)

其中g为<font color=#ff0000>门控信号</font>， $x^l \quad w$为上一层的`feature map`，g来自于下一层，所以尺寸大小是上一层的1/2，所以要对上一层进行下采样。

Attention Gate模块接收两部分的输入$x^l$和$g$。因为g提取<font color=#ff0000>粗粒度信息</font>，包含上下文信息，g里面的信息就是注意力该学习的方向，因此将g作为门控信号，将g里面的信息叠加到$x^l$，学习`attention coefficients`，将`attention coefficients`与$x^l$相乘，最后将得到的输出与上采样后的特征图拼接。
Attention Gate的公式如下，$b_g$和$b \psi$是偏置项：
$$
q^l_{att}=\psi^T(\sigma_1(W^T_xx^l_i+W^T_gg_i+b_g))+b_{\psi}  
$$
$$\alpha^l_i=\sigma_2(q^l_{att}(x^l_i,g_i;\theta_att)),$$

其中$\sigma_1$表示Relu函数，$\sigma_2$是sigmoid激活函数,因sigmoid函数可以使训练更好的收敛。

$$\sigma_1(x^i_{i,c}=max_(0,x^i_{i,c}))$$
$$\sigma_2(x_i,c)=\frac{1}{1+exp(-x_i,c)}$$

Attention Gate的最终输出是输入特征图和attention系数相乘：再将得到的最终输出与解码器上采样之后的feature map串联。

$$\hat{x}^l_{i,c}=x^l_{i,c}\cdot\alpha^l_i$$


AGs在正向传递和反向传递中filter the neuron activations，来自背景区域的梯度在向后传递时向下加权。l-1层中卷积参数的更新规则：
![链接](https://img-blog.csdnimg.cn/8f3d09c113224269bf92c6d1e8d57e01.png)


```
3Dunet网络门控模块
首先是一个普通的卷积操作
```