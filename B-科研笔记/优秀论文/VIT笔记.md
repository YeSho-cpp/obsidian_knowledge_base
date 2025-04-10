## 模型详解

<img src="https://img-blog.csdnimg.cn/20210626105321101.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NTQxMDk3,size_16,color_FFFFFF,t_70#pic_center" alt="image.png" style="zoom:80%;" />

**模型组成**：
- `Linear Projection of Flattened Patches`(Embedding层),其实是一个全连接层,维度就是$768×768$,就是论文里的D
- `Transformer Encoder`(图右侧有给出更加详细的结构)
- `MLP Head`（最终用于分类的层结构)

**总体模型流程**：
1. 输入一张图片把它分成一个个小的`patches`,然后将这些`patches`输入到(Embedding层),成为一个个**向量(token)**
2. 所以我们在最左边又加了一个`class token`,这个token用于分类,这里的token的dimension与其他一致
3. 因为`Transformer`的自注意力层`self-attention layer`是**全局性**的,自注意力是两两交互，不存在一个顺序问题，不具备[[科研名词解释#CNN的归纳偏置|CNN的偏置归纳]],所以我们在右边加上一个`Position Embedding`
	- <img src="https://article.biliimg.com/bfs/article/436eb1b2ccdf0128fe0f2528b7ed69890ec9da76.png" alt="image.png" style="zoom:50%;" />
	- <img src="https://article.biliimg.com/bfs/article/12c72e8c0c360e1c349fd2003fa9b300251405c3.png" alt="image.png" style="zoom:50%;" />
	- 上面就是`Position Embedding`的可视化效果,可以看到当前行当前列做[[对比学习#3）损失最小化|余弦相似度]],这里已经学到了位置的概念
4. 然后将这些token输入到`Transformer Encoder`中,结构如右边所示,这个结构重复堆叠L次
5. 因为所有的token都在交互,这个`cls`能从其他的`embedding`里学到有用的信息,只根据cls的输出到`MLP Head`得到分类结果
	- 通常卷积分类操作是进行多个残差网络在最后一个`featuer map`上做`GAP`(global average pulling)得到拉直的向量这就是一个分类结果
	- <img src="https://article.biliimg.com/bfs/article/3bd2bd0beef8000dca0bd4818023a16255b9affa.png" alt="image.png" style="zoom:50%;" />
6. 我们的token数量是$224^2/16^2=196$个,而一个图像块的维度是$16×16×3=768$,所以一个图片的输入就是$196×768$,加上`cls`,输入就是$197×768$


**Transformer Encoder流程**:
1. $197×768$的tensor,进入到一个`layer Norm`后出来还是$197×768$
2. 进入`Multi-Head Attention`(多头自注意力),tensor变成三份k,q,v,每个都是$197×768$,如果这个head数量是12,那么维度就是$768/12=64$了,12个去做自注意力操作,最后拼接起来
3. 经过一个残差,在经过一个`layer Norm`
4. 进入`MLP`,将维度放大4倍,再缩放回去,又经过一个残差输出



下面是整个结构的图

<img src="https://img-blog.csdnimg.cn/20210704124638234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NTQxMDk3,size_16,color_FFFFFF,t_70#pic_center" alt="image.png" style="zoom:60%;" />




查询和学号"01"同学学习的课程完全相同的其他同学的信息

```sql
select stu_id, stu_name, stu_age, stu_sex 
from t_student 
where stu_id in (
	select s_stuid 
    from t_score 
	where s_stuid <> '01' 
    group by s_stuid 
	having group_concat(s_cid ORDER BY s_cid) = (
          select group_concat(s_cid ORDER BY s_cid) from t_score where s_stuid = '01')
);
```





