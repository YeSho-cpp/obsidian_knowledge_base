## 模型讲解
<img src="https://article.biliimg.com/bfs/article/a532a8cf0979aeb7d2da1e4eed2a733adbebe646.png" alt="image.png" style="zoom:100%;" />


**VIT的缺点**:
- VIT对每一层的`Transformer Blcok`的输入token尺寸都是一样的,是通过全局的自注意力达到建模,但是对**多尺度**的把握就会弱一些,但是视觉领域的[[科研名词解释#下游任务：|下游任务]]是很关键的,在CNN中类似的任务如[[FPN|FPN]]和[[Unet笔记|Unet]],能抓住**物体不同尺寸**的特征.
- VIT是直接下采样16倍(这里意思是整个$16×16$区域看成一个像素了,相当于将分辨率降低到1/16了)
- VIT的自注意力始终实在最大的窗口上进行,做的是做全局的自主意力,也就是在整张图上进行,复杂度跟图像尺寸平方成正比,而很多下游任务的图片都是很大的

**Swin Transformer创新思路**:
- 卷积之所以有多尺度的特征,是因为有池化这个操作,增大了每一个卷积核的[[感受野|感受野]]大小,池化后的特征可以抓住不同尺寸的特征
- 将特征图划分成了多个不相交的区域（Window），并且Multi-Head Self-Attention只在每个窗口（Window）内进行,能够减少计算量的.
- Swin Transformer提出了一个类似于池化的操作`patch merging`,就是把**相邻的小patch==合并==大patch**,大patch就能看到之前4个小patch看到的内容，感受也就增大了，如上图(a),开始的patch大小是$4×4$,到后面成$16×16$,这样就能抓住多尺度特征了,



<img src="https://article.biliimg.com/bfs/article/55c47c5a6af837372a5d4267dae4b0eef8dd186c.png" alt="image.png" style="zoom:100%;" />
虽然减少了计算量但也会**隔绝不同窗口之间的信息传递**,所以提出了
 移动窗口机制:
- 这里的右边的白色小方格就是一个最小的单元$4×4$,红色的方格是一个中型的计算单元,就是一个**窗口**,默认有49个白色小方格
- 这个shift操作就是向右下角方向移动两个patch,像右边一样,在这个新的特征图中我们再分成4个窗口,shift完之后窗口就变多了
	- 这样的好处是**窗口与窗口之间可以进行互动了**,原来的方式窗口都是不重叠的,每次自主意力操作都是一个小的窗口进行了,那当前窗口的patch就注意不到其他窗口的patch信息了,这样是达不到transformer理解**上下文**的初衷了
	- 这样的shift移动就可以达到窗口与窗口之间交互了,再配合`patch merging`合并到最后几层时,每一个pacth的感受野就会很大了,能看到大部分图片了,shift移动相当于把**局部自主意力**变成一个**全局自主意力了**


## 模型架构


整体流程:


<img src="https://article.biliimg.com/bfs/article/4dee527525d751cab1307f962e89eee6445807dd.png" alt="image.png" style="zoom:50%;" />

1. 假如我们输入一张图片,swin的patch大小时$4×4$,经过一个`Patch Partition`就变成$(224/4)×(224/4)×(4×4×3)=56×56×48$
2. 接下来进入一个`Linear Embedding`,用来转换维度,转换成C维度,这是个超参数,这里是96,输出后的就是$56×56×96=3136×96$
3. 这里的3136的序列长度太大了,swin引入了基于窗口的自主意力计算,而每个窗口的patch只有$7×7=49$个序列长度
4. 这里`Swin Transformer Block`讲就是做基于窗口的自主意力计算,经过两次`Swin Transformer Block`后还是$56×56×96$
5. 然后进入`patch merging`的过程
	- 类似卷积的池下采样两倍,选点时,每隔一个点选一个,原来的一个张量就变成了4个张量了,张量大小变成了$H/2$和$W/2$了,尺寸缩小了一倍,这4个张量在维度C上拼接,就变成$(H/2)×(W/2)×4C$,为了类似卷积的池化,再经过一个卷积把维度变为$2C$
	- <img src="https://article.biliimg.com/bfs/article/7fb8acaaa5ef85048819b35d34ed85d46a7a5867.png" alt="image.png" style="zoom:40%;" />
	- 经过`patch merging`后输入就变成了$28×28×192$了
6. 后面就是重复的阶段步骤,张量最终变成$7×7×768$,这样的过程很像一个卷积的前向传播的过程

## 滑动窗口机制


<img src="https://article.biliimg.com/bfs/article/5c411c2f74077f4dd51520a56857b5067ef43576.png" alt="image.png" style="zoom:50%;" />

我们拿第一层的窗口举例,窗口大小是$56×56×96$,切成这种橘黄色的小方格(这里就是前面提到的**红色中型窗口**)，里面有49个小patch,所有的自主意力都是在这个小窗口进行的,那我们就有$(56/7)×(56/7)=64$,在这64个窗口分别去计算自主意力

对比与VIT计算量


$$
\Omega(MSA)=4hwC^2+2(hw)^2C   
$$
$$
\Omega(W-MSA)=4hwC^2+2M^2hwC
$$


h和w分别代表高宽有多少个patch,这里都是56,C就是特征维度,这里的M就是7,一个窗口的**一条边上有多少个patch**

公式推导过程:
1. `self-attention`变成q,k,v三个向量,相当于原来的向量乘上三个系数矩阵
2. q和k会去做一个相乘,最后得到一个attention,attention再和v做一次乘法,相当于做了一次加权
3. 后面还有一个`projection layer
4. 给这些向量加上该有的维度,q,k,v计算复杂度为$3hwc^2$,算attention以attention和v做乘法复杂度是$(hw)^2C$,投射层计算复杂度为计算复杂度为$hwc^2$,整个复杂度变成上面式子
5. 而W-MSA的计算`self-attention`宽高是M了,代入上面公式就是$4M^2C^2+2M^4C$,在乘上$(h/M)*(w/M)$就会得到上述公式

<img src="https://article.biliimg.com/bfs/article/f6290deb3c702399ef8c31dd4762df22f204ecc3.png" alt="image.png" style="zoom:50%;" />


是先做一次基于窗口的`self-attention`,在做一次移动窗口的`self-attention`,这样来达到窗口之间的通信

<img src="https://article.biliimg.com/bfs/article/eef513fc0f70ff32f7a0e078e7d494dd580feae3.png" alt="image.png" style="zoom:70%;" />
这两个Blocks才是swin一个基本的计算单元


## 移动窗口改进

<img src="https://article.biliimg.com/bfs/article/55c47c5a6af837372a5d4267dae4b0eef8dd186c.png" alt="image.png" style="zoom:100%;" />

上面的移动窗口移动后窗口数量变多了,而且大小不一,怎样还能**保证移动窗口还是4个呢**?每个窗口的patch数量不变呢?

<img src="https://article.biliimg.com/bfs/article/30785532ad2fcee0292c1175b6de5eae216cafd8.png" alt="image.png" style="zoom:40%;" />

这里可以看到我们采用A,B,C移动过去,窗口就是4个了,但是这样只有一个可以做自主意力了,因为另外3个都是不完整不相邻的,不应该做`self-attention`,那么怎没办到呢?
这里采用掩码方式解决

<img src="https://article.biliimg.com/bfs/article/b0ed13f80e032700c520d07c23b72ef622683178.png" alt="image.png" style="zoom:50%;" />
- 上面相同序号的是一个临近区域,不同序号的块它们是不相邻的
- 以左下角的3和6为例,做`self-attention`时,将向量拉直,这里的3的边长是4,6的边长是3,它们的宽度都是7,数量分别是28和21,进行转置乘法时,得到右边图所示
- 这里的36和63都不要,我们加上一个绿色的矩阵,那么36和63就会时一个很小的数,经过softmax就是0,就不起作用了,达到这些区域`masked`效果

算完`self-attention`后,要再还原回去,保持原来图片的相对位置

<img src="https://article.biliimg.com/bfs/article/91d261a411e1007fae8275b92bee46dcf0be964f.png" alt="image.png" style="zoom:50%;" />

这是不同区域的要相加的矩阵的可视化 

## 相对位置偏置

$$Attention(Q,K,V)=SoftMax(QK^T/ \sqrt{d}+B)V$$
这里的B就是`Relative position bias` 


<img src="https://img-blog.csdnimg.cn/d438f7d2265743e585e4a6f716631fe9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5aSq6Ziz6Iqx55qE5bCP57u_6LGG,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center" style="zoom:70%;" />

