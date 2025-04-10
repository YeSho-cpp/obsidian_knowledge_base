# WSL任务
数字病理学图像级任务任务：给定大约150K×150K的图像（例如：WSI），预测：
> [!example]- 任务
> - 癌症阶段/[[科研名词解释#亚型|亚型]]
> - 生存结果
> - 对治疗的反应

![image.png|500](https://article.biliimg.com/bfs/article/f8d2139c48a6ce2e53c871910264179ba6d4054b.png)

---

现在的示例
将输入的大图像（例如WSI）分割成==一组小图像块==（也称为图像片段或图像块），每个小图像块可以被视为一个“**集合**”。这种分割可以方便地对每个小图像块进行处理和分析，从而实现数字病理学图像级任务。

![image.png|500](https://article.biliimg.com/bfs/article/91424dc2d19b4baac956c5df68b04eab0ee68875.png)

---


# 现有的难点

数字病理学图像级任务的难点主要包括以下几个方面：

> [!summary]- 困难点
> 1. <font color=#ff0000>上下文信息的获取</font>：数字病理学图像通常非常大，需要将其分割成小的图像块进行处理。然而，小的图像块可能无法捕捉到足够的上下文信息，从而导致任务的难度增加。
> 2. <font color=#ff0000>层次信息的获取</font>：数字病理学图像中存在着复杂的层次结构，例如细胞、组织、器官等。对于一些任务，例如癌症阶段预测，需要捕捉到这些层次结构中的重要信息，这也是一个难点。
> 3. <font color=#ff0000>大规模序列输入</font>：一些任务，例如生存结果预测，需要将大量的小图像块组合成一个序列输入。这样的输入规模非常大，会导致计算量和存储量的增加，同时也会限制模型的设计和训练。
> 4. <font color=#ff0000>MIL架构的限制</font>：多示例学习（MIL）是数字病理学图像级任务中常用的方法之一，但是现有的MIL架构通常只能使用[[科研名词解释#置换不变池化|置换不变池化]]，无法处理序列输入和层次结构信息，从而限制了任务的表现。
> 5. <font color=#ff0000>高维低样本问题</font>：数字病理学图像级任务通常是高维低样本的问题，即输入数据的维度非常高，但是样本数量非常少。这也会导致模型的训练和泛化能力受到限制，容易出现过拟合等问题。


![image.png|950](https://article.biliimg.com/bfs/article/b47f7f504d85da453af2d92c5dc02d0cc9c3e115.png)

---


在20倍放大下，WSls的图像比例尺大约为每像素0.5微米。

# 创新点

> [!important]- 创新点
> 1. 首先，数字病理学图像中的视觉概念是客观的，因为在给定的放大倍数下，图像比例尺始终固定。
> 2. 其次，整个数字病理学图像呈现出一种层次结构的视觉标记，跨越不同的分辨率：从定义细胞活动的16 x 16标记，到定义细胞间相互作用的256大小的图像块，到定义组织中细胞空间组织的4K x 4K图像区域，整个数字病理学图像描绘了整个组织微环境。

![image.png|550](https://article.biliimg.com/bfs/article/4824cd2d8ad0e3a9c3fd3974e10da02b3b0672d9.png)


---

# HIPI网络
根据这个提出了HIPT(Hierarchical lmage Pyramid Transformer Architecture)
> [!note]- 结构
> - WSl被建模为一组不相交的嵌套patch嵌入序列（类似于长文档）
> - 使用3个ViT模型在本地窗口中计算Transformer注意力。

1. HIPT将WSI==递归==地划分为非重叠的tokens，首先将其分成4K大小的图像区域，然后在每个4K大小的区域内将其划分为256大小的tokens，最后再将其划分为16 x 16的tokens
	![image.png|550](https://article.biliimg.com/bfs/article/c7ce67eca59fbd9d3df8c9a10ccd1ddda5656e14.png)

2. 在每个窗口内进行嵌套的自下而上==聚合==，使用Patch-level ViT对每个token进行建模，建模token之间的依赖关系并输出token级别的表示。
	![image.png|312](https://article.biliimg.com/bfs/article/a2c2c66809f4129af1eff8e699f94d6cf3246e03.png)
3. 这些计算出的token特征用作区域级别ViT的输入序列，然后将区域特征用作wsl级别ViT的输入序列。
	![image.png|525](https://article.biliimg.com/bfs/article/a0f1bcbcaa0163f1f120e4197a8c02cf463fe51f.png)
4. 注意对于256和4K图像，序列长度始终为256，使Transformer注意力可处理，并且在建模标记之间的依赖关系方面具有等效的子问题。此外，我们进一步假设递归性质可以使VIT预训练技术在高分辨率图像的各个阶段上进行泛化。
![image.png|475](https://article.biliimg.com/bfs/article/c2dbf8b65bb1ca73e23aed43adbc62f50770ed85.png)

**我们认为使用递归的方式对数字病理学图像进行建模可以使ViT预训练技术在不同分辨率的图像上进行泛化，从而提高模型的泛化能力。**

![HIPT Architecture.gif](https://article.biliimg.com/bfs/article/824e2d30f56ae1b90d8aab88c6cfe8b656357428.gif)

---


# HIPT的预训练

HIPT的预训练过程分为两个阶段。
首先，使用DINO（一种基于学生-教师[[科研名词解释#知识蒸馏|知识蒸馏]]的自监督学习方法）对第一阶段的Patch-Level ViT进行预训练，注意到注意力头可以隔离出独特的视觉概念，例如基质、细胞和白色空间。

![image.png](https://article.biliimg.com/bfs/article/7dcc41c04633f77e2d615d5141d91104fc7ed8dc.png)

---

然后，将Patch-Level ViT冻结，从每个区域中预提取256大小的特征，并再次使用DINO对区域级别ViT进行预训练。这个方法被称为分层预训练。虽然只有HIPT的前两个阶段进行了预训练，但我们注意到这种方法可以推广到具有足够数据点的任何图像分辨率。

![image.png](https://article.biliimg.com/bfs/article/1138963600609cdf064643f959d2dfef602379fd.png)


---

# 效果对比

![image.png](https://article.biliimg.com/bfs/article/5aac209abb7f0fc0f6dff4294743779e30e5cd9c.png)



---

# HIPT热图

HIPT的热图可以将==注意力分布==因子化，以可视化粗到细的特征。例如，使用Patch-Level ViT和Region-Level ViT中的注意力头可以分别隔离出细胞和基质区域，并将它们的分布因子化在一起，可以检测出与肿瘤相关的基质。使用不同的注意力头，还可以检测高细胞密度的肿瘤区域。

![链接](https://article.biliimg.com/bfs/article/5de31519dcecdbaeff397739d01fb7d2a6e4d409.png)





1.n的值
数据库
细粒度分类论文



![](https://img-blog.csdnimg.cn/61272145ba8244f78b0f3d0034a19928.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATWVuZ1lhX0RyZWFt,size_20,color_FFFFFF,t_70,g_se,x_16)

