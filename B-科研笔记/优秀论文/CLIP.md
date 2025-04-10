文章提出了一种基于**视觉-语言**的**多模态预训练**方法，该方法能够进行高效的**零样本学习**。

### 亮点：

- 提出了一种基于视觉-语言的多模态预训练方法 CLIP。
- CLIP 能够进行高效的零样本学习 zero-shot learning。

## 2 方法

在自然语言领域，自学习的预训练模型已经取得了巨大的成功，比如 GPT-3。但是，计算机视觉领域仍然主要在使用 ImageNet 的预训练模型。OpenAI 从互联网上收集了 4 亿的图像-文本数据对，（1）使用图像和文本的**对比预训练**。然后，（2）使用[[科研名词解释#prompt|prompt]]构建了一个适合零样本的分类头，（3）通过和输入图像进行相关性对比，以得到最终的分类结果。


<img src="https://pic3.zhimg.com/80/v2-e9f5fa3146589f72e3c0cc0a72152fb6_1440w.webp" alt="image.png" style="zoom:70%;" />

## CLIP 预训练

<img src="https://pic2.zhimg.com/80/v2-81cc0e519108bb09c66a1137b85a9221_1440w.webp" alt="image.png" style="zoom:80%;" />

上图不是对角线的元素就是负样本
CLIP 模型对图像和文本构建了两个不同的编码器：
- 文本编码器（Text Encoder）：作者采用 Transformer 模型，使用 [[科研名词解释#BPE|byte pair encoding (BPE)]] 表征的，并且使用了 masked self-attention。对于输入文本$T$，它的输出是一个一维的特征表征$T_f$
- 图像编码器（Image Encoder）：作者仍然采用了 Transformer 模型，更具体得他们采用了 ViT 模型。对于输入图像$I$，它的输出也是一个一维的特征表征$I_f$。
然后，对这两个输出，再通过线性层$W_t,W_i$,进行一个多模态编码分别得到：
- 文本的多模态编码$T_c$
- 图像的多模态编码$I_e$
之后，计算这两个模态之间的余弦相似度得到$N×N$个相似度，通过对比学习 (InfoNCE) 损失拉近对应图像和文本对的相似度，拉远不对应文本对的相似度。并且，使用了一个可学习的温度系数。以上的步骤，可以见下图的伪代码所示：

<img src="https://pic2.zhimg.com/80/v2-04c5c25e11cb2f268e96b5218044f419_1440w.webp" alt="image.png" style="zoom:60%;" />
在这里，作者的一个比较重要的提升是使用 `bag-of-words` 的整体表征进行图像文本对的预训练，而不要求每个文字和图像进行精准的匹配。这种方式，大大降低了对标注准确度的需求，也成为了 CLIP 成为的一个较为重要的因素。
在 CLIP 之前，和它的预训练较为相似的模型还有 VirTex, ICMLM, ConVIRT 等。CLIP 的工作也吸取了他们的精华，并进行了改进。

### CLIP 进行零样本预测

<img src="https://pic2.zhimg.com/80/v2-4292a4773ff448f03cb1b4e24328330d_1440w.webp" alt="image.png" style="zoom:70%;" />

上面的预训练方式并没有显式的分类头，为了能够进行分类。作者通过文本-图像编码器，构建了一个检索头用于分类。具体地，作者将**所有可能的图像类别标签**全部**使用文本编码器进行编码**，然后**一一和图像编码器的输出进行相似度的计算**，那么**图像和文本相似度最高**的对所**对应的文本就是图像的预测**。为了提升性能，作者这里不是简单的将标签 plane, car, dog, ..., bird 送入，而是进行了 prompt，使用 A photo of a {object} 作为输入，这种方式大大提升了零样本预测的结果。

## 实验

### 使用 Bag-of-Words (BOW) 是一种有效的训练方法

<img src="https://pic4.zhimg.com/80/v2-30b7839bb4aaab1f7cb4377de5fc9f1b_1440w.webp" alt="image.png" style="zoom:70%;" />

相比于逐个单词的准确预测（Transformer Language Model），使用了 BoW 的方法在悬链效率上有着显著的提升。在 BoW 方法中， 使用了文本和图像对监督的 CLIP 又有着更加显著的提升。


### Prompt 工程很有用

对于使用了 Prompt 和 Ensemble 的 CLIP，它的效果得到了很大幅度的提升。这种提升是完全不需要额外训练的。

<img src="https://pic2.zhimg.com/80/v2-cc013d18fde6fa40323574da295e88a9_1440w.webp" alt="image.png" style="zoom:70%;" />

### 零样本迁移能力

作者做了许多实验，证明了 CLIP 强大的零样本迁移能力。
对比另外一个**零样本预测模型 Visual N-Grams**，CLIP 在ImageNet 上比它有 60% 的准确度提升。

<img src="https://pic1.zhimg.com/80/v2-4ae93db1b6112b5c8bfd7fe250981aec_1440w.webp" alt="image.png" style="zoom:50%;" />
**同 Linear Probe 的 ResNet50 （那么它是全监督的）对比**，zero-shot CLIP 在许多任务上有巨大的提升。

<img src="https://pic1.zhimg.com/80/v2-c21fc3079641bd4739e6fde645142eac_1440w.webp" alt="image.png" style="zoom:70%;" />




