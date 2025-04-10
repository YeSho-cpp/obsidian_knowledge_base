### 为什么要多个特征图融合?
首先有一个前提:
	深度学习网络浅层特征提取纹理特征（如颜色、物体表面形态、皮肤等），深层网络提取深层次语义信息。
思考以下问题:
	假设目前给你提供一个信息，一个目标有鼻子、眼睛、耳朵、四肢，该目标最大可能性是什么?
	![image.png](https://article.biliimg.com/bfs/article/354981771e261fccfe8ec38efaf0c6b1911d7987.png)

1.光凭借深层语义信息，有时候不能有助于分类
2.浅层特征感受野较小，往往能提取**细微特征**
3.浅层特征也是经过网络计算的，不用就浪费了
4.深层语义信息提取自浅层语义信息，在往深层提取的过程中==损失了一些信息==（就像研究生可能忘记了怎么去做初中的几何证明题，因为我们专注于学习现在的知识而忘了一些不常用的基础知识）


<img src="https://img-blog.csdnimg.cn/20210316213927771.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA3NDU2OA==,size_16,color_FFFFFF,t_70" alt="image.png" style="zoom:80%;" />




