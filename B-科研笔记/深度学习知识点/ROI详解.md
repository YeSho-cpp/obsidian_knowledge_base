# ROI详解
Region of interest(ROI),中文译为感兴趣区域。在计算机视觉领域，从输入的图像中框选处理待处理的区域就是ROI。
ROI / Region proposals大致过程：

- 输入一张图片
- 在图片中找到物体/目标（objects）的所有位置
- 输出/获得这些一系列的objects的bounding box.

![链接](https://img-blog.csdnimg.cn/de8d5ba0e3c141f2a9e1cecda8905d54.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5L2g5aW95ZWK77ya77yJ,size_14,color_FFFFFF,t_70,g_se,x_16#pic_center)
![链接](https://img-blog.csdnimg.cn/98597edd938041008a34bd853557104c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5L2g5aW95ZWK77ya77yJ,size_16,color_FFFFFF,t_70,g_se,x_16#pic_center)

# RoI Pooling详解

## Pooling操作

在了解ROI pooling前，先了解一下pooling操作  
以2x2的maxpooling为例，能够在2x2大小中选取一个最大值。  
如下图所示4x4的矩阵，变为了2x2的矩阵。
![链接](https://img-blog.csdnimg.cn/img_convert/92fed6855f62883540ea7239f77c77b2.png#pic_center)

ROI pooling
Roi pooling的操作流程：

输入图像后经过特征提取，得到特征图（Feature Map）。
RoI区域映射到特征图上（映射：与ROI在原图上的位置相对应）。
将映射后的区域划分成多个部分，部分的数目的输出的维度有关。
对每个部分进行pooling(max pooling)操作。
下面是一个图像的特征图，使用0.88、0.44等构成的8x8矩阵进行表示，需要输出2x2大小的矩阵。
![链接](https://img-blog.csdnimg.cn/082fd36c0c524ceb92c55e14d40f2ffb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5L2g5aW95ZWK77ya77yJ,size_20,color_FFFFFF,t_70,g_se,x_16)
图中红框表示ROI在Feature map 上的映射区域，（1，2）和（7，7）分别表示映射区域的左上角及右下角坐标。  
(怎么映射？特征图和原图存在一定的大小比例，按照比例对原图上的ROI区域进行调整，就能够得到红色区域)
![链接](https://img-blog.csdnimg.cn/f81986f0e50747669b14764d2a9b989a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5L2g5aW95ZWK77ya77yJ,size_20,color_FFFFFF,t_70,g_se,x_16)

现在要输出2x2的矩阵，所以要将ROI映射区域划分为四个部分（现在ROI区域是5 x 6 大小）。  
划分过程如下：
-   5/2 =2.5，即将5行划分为 2 + 3 行两部分
-   6/2 = 3,即将6列平均划分为 3+3列两部分
![链接](https://img-blog.csdnimg.cn/4824253418c34350bbcfead60459afea.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5L2g5aW95ZWK77ya77yJ,size_20,color_FFFFFF,t_70,g_se,x_16)

-   当然，现在我们不用 2 X 2 大小的maxpooling进行池化，大小不够。
-   而使用 2 X 3 和3 X 3的maxpooling对四个区域进行池化操作。
-   最终得到2 X 2d的所需结果。
![链接](https://img-blog.csdnimg.cn/4f7ab7afff6c4fcca97cf2bff76e2ed2.png)
# ROI Align详解
ROI Align是对ROI pooling中取整操作，造成的偏差的改进。

ROI pooling的取整操作:

- ROI映射到feature map上，比例进行变化。但是这个比例变换不一定整数倍的变换，存在小数是就会取整操作
- ROI 映射到特征层后，按照输出维度划分ROI映射区域时，划分的区域不一定是刚好划分平均（5/2 = 2.5, 所以分为2 + 3）。
主要是使用线性插值的方法，我自己只能理解，所以推荐下下面的链接。当然也可以多在网上查一查相关资料。