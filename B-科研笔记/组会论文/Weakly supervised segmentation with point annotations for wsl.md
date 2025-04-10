# 传统工作的缺点和难点

> [!summary]+
> 1. 大多数的wsl任务方法都是基于[[科研名词解释#有监督学习|有监督的学习]]，严重依赖大量的详细的标注样本，这种样本还很贵。
> 2. wsl相比于其他图像有两个特性 这都是处理的难点  见[[Weakly supervised segmentation with point annotations for wsl#^5834ad|图1]]
> 	- 目标组织是不规则形状
> 	- 没有**相对一致**的纹理
> 3. 传统方法解决样本标注问题，大多都是采用[[科研名词解释#弱监督学习|弱监督]]方法，弱监督采用[[科研名词解释#多示例学习（Multiple Instance Learning）|多实例学习]]，分为下面三个标注，部分标注有最好的**目标定位**能力，而且需要较少的标注成本。第三种现在被称为**部分监督学习**，但是这些监督方法都适于处理wsl图像的两个难点。
> 	- **图像级标注**
> 	- **图像内标注**(点或涂鸦标注)
> 	- **局部图像的完全像素标注**([[科研名词解释#半监督学习|半监督学习]])
> 4. wsl图像的一个重大特点是肿瘤通常聚集在大区域内，其中**形态特征或纹理与外部区域相似**，但在视觉上不同，并且存在**相对清晰的边界**。如[[Weakly supervised segmentation with point annotations for wsl#^5834ad|图1]]所示，现有的监督方法没有利用这一特性。
> 5. 从wsl图像包含一些非目标区域，这些区域与其他图像不同，如果不标记，对训练模型来说就是==新==的，如果它们的预测类别与目标组织相似，就会判断，这也就是传统的基于[[对比学习|对比学习]]的自监督方法的问题。


# 作者提出的工作

1. 基于上述传统工作的缺点和难点，作者采用一种**变体的分割模型**来提供一种**互补的监督信息**，以**辅助**深度分割模型的训练。这种变分分割模型本身将由**目标内点**和**目标外点**的标注的来指导对图像中的目标区域进行分割。如[[Weakly supervised segmentation with point annotations for wsl#^5834ad|图1]]的红点和蓝点所示
2. 因为现有的**变体分割模型**不能直接应用于包含更高层次语义信息的**高维深度特征**。引入了**对比图(Contrast Maps)** 的概念，


# 近期的工作

## Selective segmentation
主要介绍了`Selective segmentation`是一种利用`ROI区域`进行分割的变分分割方法。它通常通过将`distance constraints`合并到[[科研名词解释#Energy functions|energy function]]中进行优化来实现。距离约束基于输入点的位置,然而，传统的选择性分割方法主要依赖于图像的**几何关系**和**像素值**，而没有考虑图像中的**高级区域相关性**。因此，它们在处理复杂纹理和不规则形状的区域，尤其是组织病理学图像中的区域方面存在不足。


## Partially supervised segmentation
大量的这类方法通过==合成==从`partial annotations`生成的标签来解决部分监督学习问题，通过这些标签，学习过程实际上变得**完全监督**。然而，许多现有的合成方法基于同一图像的不同**增强版本**之间的一致性，这在处理未标记的新区域方面是有缺陷的。特别是，组织病理学图像通常在每个图像中具有一些特殊区域，这些区域对于其他图像中的图像是独特的。除了还有**正则化**，然而正则化不太适用于组织病理学图像，因为组织区域通常包含复杂的纹理或不规则的形状。



# 方法

## frame work
提出**双组件架构**，第一个组件是**特征提取器**，第二个组件是**常规深度分割网络**

![image.png|1075](https://article.biliimg.com/bfs/article/0dfabdcced58da24ab14ad9ca5c51fb700a2425d.png)


> [!summary]+ 流程概述
> 1. 具有高分辨率的图像被馈送到第一个组件特征提取器（图中的黄色框）。当输入高分辨率图像时，特征提取器将生成降维特征图，提取输入图像的高级特征。
> 2. 生成的特征图被转发到第二个组件，即用于分割的常规深度分割网络（图中的橙色框）。然后，分割网络输出分割结果。
> 3. 同时，基于对比度的变分模型将来自卷积分割网络的特征映射和标注的点输入，并生成变分分割。深度分割模型由变分分割和标注的点同时分别由加权KL散度和部分交叉熵监督。
> 4. 红点代表标注的的目标内点，而蓝点代表标注的的目标外点。


## Contrast-based variational model for segmentation

![image.png|750](https://article.biliimg.com/bfs/article/9b29c396bd381b3b3ec710c65df0f75fb2f0355f.png)


> [!example]+ 过程
> 1. 根据这些标注的点生成一组相关图
> 2. 通过一种`subtraction operation` 由相关图获得`Contrast Maps`
> 3. 用一个变体的公式来聚集获得的对比图以获得最后的分割结果
> 4. 变体分割通过(KL)散度为深度分割模型提供了**补充监督**。此外，所提出的模型可以缓解前面提到的未标记的新组织区域的问题，该问题是由于获得`Contrast Maps`时的`subtraction operation` 而导致的。



## Loss function for training
由两个损失函数组成，**kl散度**和**部分交叉熵**

![image.png](https://article.biliimg.com/bfs/article/4dab583bffe698f538e7b07a0177cec5594bafa7.png)

**部分交叉熵**
![image.png](https://article.biliimg.com/bfs/article/15cc5973f6d3639cd404d00d083f4e20ec2bc6bd.png)


**kl散度**

![image.png|375](https://article.biliimg.com/bfs/article/4e8c39b77d67196bb596e713c763f5c4678c15d2.png)


# 实验

公共Camelyon16乳腺癌数据集结果

<img src="https://article.biliimg.com/bfs/article/069a695ef04d623b30d454432e3efbcb89d07da7.png" alt="image.png" style="zoom:65%;" />


## 消融实验

![image.png](https://article.biliimg.com/bfs/article/446d9f682e651a77b933c80b7db1b7462e418a58.png)



## 展示不同方法的热图对比

<img src="https://article.biliimg.com/bfs/article/70df06a153334d03942e733c0cf5011af270e633.png" alt="image.png" style="zoom:75%;" />]()


^p1
<img src="https://article.biliimg.com/bfs/article/5afa967eb86e1ed06376967b08558916287ddf70.png" alt="image.png" style="zoom:75%;" /> ^5834ad

