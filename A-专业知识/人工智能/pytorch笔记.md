## 1.线性模型

### 步骤

+ 1.数据集(DataSet)

+ 2.准备模型(Model)

+ 3.训练(Training)

+ 4.应用推理(inferring)

### 数据集的划分

| 训练集(x,y) | 开发集 | 测试集(x) |
| :---------- | ------ | --------- |

```python
# model_selection含有选取的模型工具 train_test_split可以划分数据为训练数据和验证数据
from sklearn.model_selection import train_test_split
train_x,test_x,train_y,test_y=train_test_split(X,Y) #25%化为测试
```

**泛化**:在训练集上训练以后，对于没见过的图像它也能在正确识别

**开发集**:用于对模型的泛化能力评估，例如我们判断模型什么时候开始过拟合

```python
import numpy as np
import matplotlib.pyplot as plt
x_data=[1.0,2.0,3.0]
y_data=[2.0,4.0,6.0]
def forward(x):
    return x*w
def loss(x,y):
    y_pred=forward(x)
    return (y-y_pred)**2
w_list=[]
mse_list=[]
for w in np.arange(0,4.1,0.1): # 穷举法
    print('w=',w)
    l_sum=0
    for x_val,y_val in zip(x_data,y_data):
        y_pred_val=forward(x_val)
        loss_val=loss(x_val,y_val)
        l_sum+=loss_val
        print('\t',x_val,y_val,y_pred_val,loss_val)
    print('MSE=',l_sum/3)
    w_list.append(w)
    mse_list.append(l_sum/3)
    Output exceeds the size limit. Open the full output data in a text editor
	w= 0.0
	 	1.0 2.0 0.0 4.0
	 	2.0 4.0 0.0 16.0
	 	3.0 6.0 0.0 36.0
	MSE= 18.666666666666668
	w= 0.1
	 	1.0 2.0 0.1 3.61
	 	2.0 4.0 0.2 14.44
	 	3.0 6.0 0.30000000000000004 32.49
	MSE= 16.846666666666668
    .....
plt.plot(w_list,mse_list)
plt.ylabel('Loss')
plt.xlabel('w')
plt.show()
```

![image-20221118193556589](https://i0.hdslb.com/bfs/album/0e1262e0803b0ba6c127ba7178ce8f731227bc6f.png)

### 了解过拟合与欠拟合

**过拟合**:对于训练数据过度拟合，对于未知数据预测很差(dropout 数据增强 正则化)

![image-20230105124009376](https://i0.hdslb.com/bfs/album/dbf555d4f56a59c8e0c91f854633a5eb7825005a.png)

关于Dropout的原理可以参考这篇[博客](https://zhuanlan.zhihu.com/p/38200980)

**欠拟合**:对于训练数据过度不够，对于未知数据预测很差(需要改进模型)

过拟合处理

> 1. **丢弃一些不能帮助我们正确预测的特征**。可以是手工选择保留哪些特征，或者使用一些模型选择的算法来帮忙（例如**PCA**）
>
> 2. **正则化**: 保留所有的特征，但是减少参数的大小（**magnitude**)，关于具体的[正则化](https://zhuanlan.zhihu.com/p/75364861)
>
> 3. **交叉验证:** 重复地使用数据；把给定的数据进行切分，将切分的数据集组合为训练集与测试集，在此基础上反复地进行训练、测试以及模型选择（详见《统计学习方法》p24.）。

**批标准化（Batch Normalization，BN）** 是一种用于深度学习的技术，它的目的是通过对每一层的输入进行归一化来提高模型的训练效率和泛化能力。

这里是[深度学习中 Batch Normalization为什么效果好？](https://www.zhihu.com/question/38102762)的讨论

## 2.梯度下降算法

###  梯度下降

无论是采用`穷举法`还是`分治法`都不适合找到全局最优解，`穷举法`的问题在于效率太慢，而`分治法的`问题是容易陷入局部最优解，所以引入`梯度下降`

`梯度下降`代码

```python
x_data=[1,2,3]
y_data=[2,4,6]
w=1 # 权重初始随机为1
def forward(x):
    return x*w
def cost(xs,ys): #损失函数
    cost=0
    for x,y in zip(xs,ys):
        y_pred=forward(x)
        cost+=(y_pred-y)**2
    return cost/len(xs)
def gradient(xs,ys): #梯度
    grad=0
    for x,y in zip(xs,ys):
        grad+=2*x*(x*w-y)
    return grad/len(xs)
print('predict (before training)',4,forward(4))
cost_list=[]
epoch_list=[]
for epoch in range(100): # 总共训练100轮
    cost_val=cost(x_data,y_data)
    grad_val=gradient(x_data,y_data)
    #每一轮改变的是这个w,每次更新这个w就要用到所有的样本数
    w-=0.01*grad_val#学习率为0.01 
    print('Epoch:',epoch,'w=',w,'loss=',cost_val)
    cost_list.append(cost_val)
    epoch_list.append(epoch)
print('Predict (after traing)',4,forward(4))
plt.plot(epoch_list,cost_list)
plt.ylabel('Cost')
plt.xlabel('Epoch')
plt.show()
```

<img src="https://i0.hdslb.com/bfs/album/71302fc585f95c30249c5990ec3941d144ae90ef.png" alt="image-20221119094809094" style="zoom: 33%;" />

<img src="https://i0.hdslb.com/bfs/album/00091d27a0fa4087cc13a5929d371efca9a194ba.png" alt="image-20221119094627114" style="zoom: 80%;" />

### 随机梯度下降

<img src="https://i0.hdslb.com/bfs/album/c2a0f4e33e5fc365aac16455f4a26b478870d549.png" alt="image-20221119095842369" style="zoom:50%;" />

代码

```python
x_data=[1,2,3]
y_data=[2,4,6]
w=1 # 权重初始随机为1
def forward(x):
    return x*w
def cost(x,y):
    y_pred=forward(x)
    return (y_pred-y)**2
def gradient(x,y):
    return 2*x*(x*w-y)
print('predict (before training)',4,forward(4))
for epoch in range(100): # 总共训练100轮
    for x,y in zip(x_data,y_data):
        #对每一个有样本求梯度去更新，不是加和平均
        grad=gradient(x,y)
        w=w-0.01*grad
        print("\tgrad",x,y,grad)
        l=loss(x,y)
    print("progress:",epoch,'w=',w,'loss=',l)
print('Predict (after traing)',4,forward(4))
```

**数据噪声**:指在一组数据中无法解释的数据变动，就是一些不和其他数据相一致的数据

**鞍点**:指在一个方向是极大值，另一个方向是极小值的点，如图1x-轴方向往上曲，在y-轴方向往下曲

<img src="https://i0.hdslb.com/bfs/album/79a50d07f4bbecb3eb07e4c0b22bde094300057f.png" alt="image-20221119092019866" style="zoom:50%;" />

==如果损失函数有鞍点，梯度下降就无法更新参数了，而采用随机梯度下降，数据有的有噪声，这个随机噪声可能把函数向前推动，走出鞍点范围==

### 批量梯度下降

`梯度下降`性能低，`随机梯度`性能高，`随机梯度`时间复杂度低，`随机梯度`时间复杂度高，做一个折中，叫做`Batch(批量梯度下降)`，将所有数据分成若干组

## 3.反向传播

### 1.前置知识

矩阵的求导可以参考[矩阵求导](https://soptq.me/2020/06/19/matrix-derivation/)这篇博客

**为什么需要激活函数**:普通的容易陷入层数多和层数少没区别， 不能化简，增加这些权重没有意义，为了提高复杂程度，对每一层加一个非线性的变换函数，为什么采用激活函数，这篇[博客](https://blog.csdn.net/asdfsadfasdfsa/article/details/92831177?ops_request_misc=%7B%22request%5Fid%22%3A%22166886529116782395311517%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=166886529116782395311517&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-92831177-null-null.142^v65^control,201^v3^control,213^v2^t3_esquery_v2&utm_term=激活函数的作用&spm=1018.2226.3001.4187)说的很清楚

[pytorch矩阵乘法详解](https://blog.csdn.net/weixin_42065178/article/details/119517404)

将庞大数据看出一张图，在图上传播梯度，链式法则 ，组成全连接神经网络

### 2.链式求导

1.创建计算图

刚开始做原子操作，`前馈运算(forward)`,由边的方向沿这输入到最后的loss，从前面传回来，这个计算图的组成单元，每一个相应的量接入到函数计算模块里面，这个模块可以计算局部的梯度，输出得到`loss`关于它的量再沿着图`反向链式(backward)`回去就能求得最后loss关于输入的梯度求出来

![image-20221119221224828](https://i0.hdslb.com/bfs/album/d2b3551957fc64f34d1fe26539ffdda46c71bb70.png)

==pytorch是将局部的梯度存到变量里，而不是计算模块==

### 3.pytorch反向传播过程

在PyTorch中，Tensor是构建动态计算图的重要组成部分,包含data和grad(**都是张量**)以及grad_fn，分别存储节点的值和损失函数和该权重的梯度的值以及张量的来源和功能。

![image-20221229210313331](https://i0.hdslb.com/bfs/album/c94628856b1637e61822ae4134719f26681b72f4.png)

第一步先算损失，第二步反向传播

```python
import torch
x_data=[1,2,3]
y_data=[2,4,6]
w=torch.Tensor([1.0])
w.requires_grad=True #默认是不计算梯度，设置这个代表我们要设梯度
def forward(x):
    return x*w #这里面的x和w,y都是张量，不是标量
def loss(x,y):
    y_pred=forward(x)
    return (y_pred-y)**2
```

```python
print("predict (before training)",4,forward(4).item())
for epoch in range(100):
    for x,y in zip(x_data,y_data):
        #forward过程，创建计算图，这里面的都是张量
        l=loss(x,y) 
        l.backward() #反向传播计算requires_grad=True的，并释放掉计算图，准备下一次的图
        #.item()把张量变成标量，防止产生计算图
        print('\tgrad:',x,y,w.grad.item())
        #不能只用-.grad会建立计算图，.data不会建立计算图，
        w.data=w.data-0.01*w.grad.data 
        # 将权重梯度全部清零
        w.grad.data.zero_()
    print("progress:",epoch,l.item())
print("preditc(after training)",4,forward(4).item())
```

## 4.pytorch实现线性回归

### 详细步骤

pytorch写神经网络步骤

1.准备数据集  2.设计模型计算预测y  3.构造损失函数和优化器(利用了pytorchAPI)  4.训练周期(前馈，反馈，更新)

### 实现结构

我们不在关注人工求导数了，重点在**构造计算图**

![image-20221121105454978](https://i0.hdslb.com/bfs/album/dfca5ce3dd00a0c785791f51e2ec19910cd83ef2.png)

我们的模型类应该继承自nn.Module，它是所有神经网络模块的基类。

<img src="https://i0.hdslb.com/bfs/album/383e72717a8a76db14dbb9deb65dd1c14d205df3.png" alt="image-20221121150715324" style="zoom: 67%;" />

图示为Model的结构图

### API定义

[torch.rand()、torch.randn()、torch.randint()、torch.randperm()用法](https://blog.csdn.net/leilei7407/article/details/107710852?ops_request_misc=%7B%22request%5Fid%22%3A%22166900185916782425140305%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=166900185916782425140305&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-107710852-null-null.142^v65^control,201^v3^control,213^v2^t3_esquery_v2&utm_term=torch.randn&spm=1018.2226.3001.4187)

nn.Linear的基本定义:

```python
torch.nn.Linear(in_features, # 输入的神经元个数(维度)
           out_features, # 输出神经元个数(维度)
           bias=True # 是否包含偏置
           )
```

**Linear其实就是对输入$X_{n×i}$执行了一个线性变换，即**:

​                                       $$Y_{n×o}=X_{n×i}W_{i×o}+b$$

torch.optim.SGD:**随机梯度下降**

`torch.optim.SGD(params, lr=<required parameter>, momentum=0, dampening=0, weight_decay=0, nesterov=False)`

+ params:要训练的参数，一般我们传入的都是`model.parameters()`。他的作用是决定需要计算那些权重
+ lr:learning_rate学习率，会梯度下降的应该都知道学习率吧，也就是步长。
+ weight_decay（权重衰退）:正则化的λ

+ Linear会随机初始化一个w

常见的优化器还有Momentum(标准动量优化算法)、RMSProp、Adam，详情看这篇[文章]([Pytorch中常用的四种优化器SGD、Momentum、RMSProp、Adam - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/78622301))介绍

学习率是典型的超参数，这篇[博客]([深度学习超参数介绍及调参_Cpp编程小茶馆的博客-CSDN博客_超参数](https://blog.csdn.net/xu_fu_yong/article/details/96102999?ops_request_misc=%7B%22request%5Fid%22%3A%22166988149816800213098454%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=166988149816800213098454&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-96102999-null-null.142^v67^control,201^v3^control,213^v2^t3_esquery_v2&utm_term=超参数&spm=1018.2226.3001.4187))有超参数的详解

那么如何提高网络的拟合能力？

1.增加层
2.增加隐藏神经元个数

单纯的增加神经元个数对于网络性能的提高并不明显,
增加层会大大提高网络的拟合能力
这也是为什么现在深度学习的层越来越深的原因

### 参数选择原则

首先开发一个过拟合的模型:

(1) 添加更多的层。
(2) 让每一层变得更大。
(3) 训练更多的轮次

然后，抑制过拟合:

（1）dropout
（2）正则化
（3）图像增强

再次，调节超参数:`学习速率`，`隐藏层单元数`,`训练轮次`

`nn.Sequential`是一个有序的容器，神经网络模块将按照在传入构造器的顺序依次被添加到计算图中执行，同时以神经网络模块为元素的有序字典也可以作为传入参数

假设我们要定义一个网络，其中一层是这样的:
input–>Linear(input)–> nn.ReLU(input)–>Linear(input)–> nn.ReLU(input)–>Linear(input)

```python
class Net(nn.Module):
    def __init__(self, in_dim, n_hidden_1, n_hidden_2, out_dim):
        super().__init__()
      	self.Linear1 = nn.Linear(in_dim, n_hidden_1),
		self.Linear2=nn.Linear(n_hidden_1, n_hidden_2)
		self.Linear3=nn.Linear(n_hidden_2, out_dim)
  	def forward(self, x):
      	out= self.Linear1(x)
      	out=torch.relu(out)
      	out=self.Linear2(out)
      	out=torch.relu(out)
      	out=self.Linear3(out)
      	return out
```

此时可使用nn.Sequential化简操作

```python
class Net(nn.Module):
    def __init__(self, in_dim, n_hidden_1, n_hidden_2, out_dim):
        super().__init__()
      	self.layer = nn.Sequential(
            nn.Linear(in_dim, n_hidden_1), 
            nn.ReLU(True)，
            nn.Linear(n_hidden_1, n_hidden_2)，
            nn.ReLU(True)，
            # 最后一层不需要添加激活函数
            nn.Linear(n_hidden_2, out_dim)
             )
  	def forward(self, x):
      	x = self.layer(x)
      	return x
```

### pytorch 实现dropout层和BN层

添加了`dropout层`或者`BN层`，训练和测试的行为是不同的

为了区分添加

model.train() 训练模式 放在训练测试集上

model.eval() 预测模式 放在测试集上

```python
class net(nn.Module):
    def __init__(self) -> None:
        super().__init__()
        # 先卷积
        self.conv1=nn.Conv2d(3,16,kernel_size=3)
        self.bn1=nn.BatchNorm2d(16) #16代表输入特征数
        self.conv2=nn.Conv2d(16,32,kernel_size=3)
        self.bn2=nn.BatchNorm2d(32)
        self.conv3=nn.Conv2d(32,64,kernel_size=3)
        self.bn3=nn.BatchNorm2d(64)
        self.pool=nn.MaxPool2d((2,2))
        self.linear1=nn.Linear(64*6*6,1024)
        self.bnf1=nn.BatchNorm1d(1024)
        self.linear2=nn.Linear(1024,128)
        self.bnf2=nn.BatchNorm1d(128)
        self.linear3=nn.Linear(128,4)
        # 增加drop层
        self.drop1d=nn.Dropout(0.5) #0.5代表被舍弃神经元的概率
        self.drop2d=nn.Dropout2d(0.5) #2d代表二维处理
```



### 详细代码实现

1.准备数据集

```python
x_data=torch.Tensor([[1],[2],[3]])
y_data=torch.Tensor([[2],[4],[6]])
```

2.设计模型计算预测y  

```python
class LinearModel(torch.nn.Module): #Module会自动构建计算图实现backward
    # 构造函数 初始化对象
    def __init__(self):
        # 调用父类构造函数
        super().__init__()
        self.linear=torch.nn.Linear(1,1)
    # 前馈过程进行的计算
    def forward(self,x):
        y_pred=self.linear(x)
        return y_pred
model=LinearModel()
```

3.构造损失函数和优化器(利用了pytorchAPI) 

```python
criterion=torch.nn.MSELoss(reduction='sum')
#这里选用SGD优化器
optimizer=torch.optim.SGD(model.parameters(),lr=0.01) #随机梯度下降
```

4.训练周期(前馈，反馈，更新)

```python
for epoch in range(100):
    # 前馈步骤
    y_pred=model(x_data)
    loss=criterion(y_pred,y_data)
    print(epoch,loss.item())
    optimizer.zero_grad()
    # 反馈步骤
    loss.backward()
    # 更新
    optimizer.step() # 进行更新，根据预先的梯度，学习率
```

5.输出结果

```python
print('w=',model.linear.weight.item())
print('b=',model.linear.bias.item())
x_test=torch.Tensor([[4.0]]) # 测试集
y_test=model(x_test) #预测集，这个model已经是一个模型了
print('y_pred=',y_test.data)
```

## 5.逻辑回归

### 概念

除了线性回归以外，我们要做的很多任务是**分类任务**，比如MIST数据集:

- Traing set :60000 examples,
- Test set: 10000 examples,
- Classes: 10

这个数据集pytorch自带

```python
# MNIST样本
import torchvision
train_set=torchvision.datasets.MNIST(root='./dataset/mnist',train=True,download=True)
test_set=torchvision.datasets.MNIST(root='./dataset/mnist',train=False,download=True)  
```

<img src="https://i0.hdslb.com/bfs/album/b2e08dd956e71a2bc0964f8b9f86518a2c709d7c.png" alt="image-20221121175726423" style="zoom:50%;" />

类似的还有CIFAR数据集

- Traing set :50000 examples,
- Test set: 10000 examples,
- Classes: 10

<img src="https://i0.hdslb.com/bfs/album/6c35ccbfc9705fcdd941dfef324d31ac6d746352.png" alt="image-20221121175753379" style="zoom: 67%;" />

这些问题的最终估算是属于其中**一个**，分类问题输出的是概率，结果就是类别中概率最大的那个

| x(hours) | y(pass/fail) |
| :------: | :----------: |
|    1     |      0       |
|    2     |      0       |
|    3     |      1       |
|    4     |      ?       |

### 与线性回归区别

以前的线性模型是一个实数，现在我们分类问题要的是一个概率，要∈[0，1]，为了将实数集映射到[0,1],我们要用到`sigmod函数`
$$
\sigma(x)=\frac {1}{1+e^-y}
$$


变化后的模型是这样的:

![image-20221121161203710](https://i0.hdslb.com/bfs/album/ac61610852c28e8a2203bc0dcb3bd99d82a3cd24.png)

这里的		$loss=(\hat{y}-y)^2=(x*w-y)$变为:

​			$loss=-(ylog\hat{y}+(1-y)log(1-\hat{y}))$

在`逻辑回归`里面我们想比较的是两个==分布之间的差异==，不是==计算几何度量之间的距离==，计算分布之间差异我们用到`KL散度`:一个概率分布的相似性的度量指标

KL散度又叫做`相对熵`，关于`熵`，`信息熵`，`交叉熵`，`相对熵`可以看这篇[博客](https://blog.csdn.net/Poyunji/article/details/123771660)

Mini-Batch loss:

$loss=-\frac {1}{N}\sum\limits_{n=1}^Nylog\hat{y}_n+(1-y)log(1-\hat{y})$

<img src="https://i0.hdslb.com/bfs/album/b7628807ecd7d9cb20bb674c2a75b18f7377272f.png" alt="image-20221121170957508" style="zoom:50%;" />

对于不是`二分类`的问题，这个Mini-Batch loss应该为:

​									$H(P,Q)=-\sum\limits_{n=1}^NP(X_i)logQ(x_i)$

### 详细代码实现

1.准备数据集

```python
x_data=torch.Tensor([[1],[2],[3]])
y_data=torch.Tensor([[0],[0],[1]])
```

2.设计模型计算预测y  

```python
class LogisticRegressionModel(torch.nn.Module):
    def __init__(self):
        super().__init__()
        self.linear=torch.nn.Linear(1,1)
    def forward(self,x):
        # 这里是区别
        y_pred=torch.sigmoid(self.linear(x))
        return y_pred
model=LogisticRegressionModel()
```

3.构造损失函数和优化器(利用了pytorchAPI) 

```python
# 将MSE换成BCE(交叉熵)
criterion=torch.nn.BCELoss(reduction='sum')
optimizer=torch.optim.SGD(model.parameters(),lr=0.01)
```

4.训练周期(前馈，反馈，更新)

```python
for epoch in range(100):
    # 前馈步骤
    y_pred=model(x_data)
    loss=criterion(y_pred,y_data)
    print(epoch,loss.item())
    optimizer.zero_grad()
    # 反馈步骤
    loss.backward()
    # 更新
    optimizer.step() # 进行更新，根据预先的梯度，学习率
```

5.输出结果图像

```python
import numpy as np
import matplotlib.pyplot as plt
# 利用np.linspace切片做x轴
x=np.linspace(0,10,200)
# 转变成200行*1列
x_t=torch.Tensor(x).view((200,1))
y_t=model(x_t)
# 将y_t张量转变成numpy数组
y=y_t.data.numpy()
plt.plot(x,y)
plt.plot([0,10],[0.5,0.5],c='r') #再添加一条y=0.5的直线
plt.xlabel("Hours")
plt.ylabel("Probability of Pass")
plt.grid() # 显示网络
plt.show()
```

<img src="https://i0.hdslb.com/bfs/album/4e88af38fb2f21caddefedeb8874ba2d0bca7b68.png" alt="image-20221121175059109" style="zoom: 80%;" />

## 6.多维数据输入

### 多维特征

糖尿病数据集

| X1    | X2    | X3    | X4    | X5    | X6    | X7    | X8    | Y    |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ---- |
| -029  | 0.49  | 0.18  | -0.29 | 0.00  | 0.00  | -0.53 | -0.03 | 0    |
| -0.88 | -0.15 | 0.08  | -0.41 | 0.00  | -0.21 | -0.77 | -0.67 | 1    |
| -0.06 | 0.84  | 0.05  | 0.00  | 0.00  | -0.31 | -0.49 | -0.63 | 0    |
| -0.88 | -0.11 | 0.08  | -0.54 | -0.78 | -0.16 | -0.92 | 0.00  | 1    |
| 0.00  | 0.38  | -0.34 | -0.29 | -0.60 | 0.28  | 0.89  | -0.60 | 0    |
| -0.41 | 0.17  | 0.21  | 0.00  | 0.00  | -0.24 | -0.89 | -0.70 | 1    |
| -0.65 | -0.22 | -0.18 | -0.35 | -0.79 | -0.08 | -0.85 | -0.83 | 0    |
| 0.18  | 0.16  | 0.00  | 0.00  | 0.00  | 0.05  | -0.95 | -0.73 | 1    |
| -0.76 | 0.98  | 0.15  | -0.09 | 0.28  | -0.09 | -0.93 | 0.07  | 0    |
| -0.06 | 0.26  | 0.57  | 0.00  | 0.00  | 0.00  | -0.87 | 0.10  | 0    |

每一行为一个**样本**，每一列为一个**特征**

**X**是一个矩阵，**Y**是一个矩阵

​     $\hat{y}^{(i)}=\sigma(\sum\limits_{n=1}^8x_n^{(i)}\cdot w_n+b)=\sigma(z^{(i)})$

1-n代表特征数，上标（i）代表样本

<img src="https://i0.hdslb.com/bfs/album/8a928f222e1774e9ad78ed1510d66256dec1ccd7.png" alt="image-20221122155901352" style="zoom:50%;" />

<img src="https://i0.hdslb.com/bfs/album/980f589f38fb4a0c7d96eb697f7c4e5437b13d3e.png" alt="image-20221122155936821" style="zoom:50%;" />

```python
class Model(torch.nn.Module):
    def __init__(self):
        super().__init__()
        # (8,1)代表着它可以把任意8维的向量映射为1维向量空间上
        self.linear=torch.nn.Linear(8,1)
        self.sigmod=torch.nn.Sigmoid()
    def forward(self,x):
        x=self.sigmod(self.linear(x))
        return x
model=Model()
```

==注意这里的维度指的是特征数==

为什么采用矩阵运算？可以利用并行计算能力，gpu并行，cpu串行，gpu适合做矩阵运算，所以在训练模型中gpu比cpu快得多

### 多维变换

![image-20221122160200154](https://i0.hdslb.com/bfs/album/a3e43b4be674cdb141c8085dc2dbd0fe91b947ac.png)

对于多线形层

![image-20221122161027517](https://i0.hdslb.com/bfs/album/0b97f4dd88a30826d5de8d597d7c5aff89a84840.png)

将数据经过三个线性层最后降到1维

层1的输出结果作为层2的输入，依次类推

但是我们经常做的训练并不是线性映射，是非线性的映射。我们用多个线性变换层，通过找到最优的权重，把他们组合起来，来模拟非线性的变换，这一步主要通过`sigmod函数`增加非线性因子，所以我们所说的==神经网络实质是一种非线性的空间变换函数==

### 详细代码实现

1.准备数据集

```python
import numpy as np
xy=np.loadtxt('diabetes.csv.gz',delimiter=',',dtype=np.float32)
# 数组转换成张量，且二者共享内存，对张量进行修改比如重新赋值，那么原始数组也会相应发生改变
x_data=torch.from_numpy(xy[:,:-1])
# 拿出来的是一个向量
y_data=torch.from_numpy(xy[:,[-1]])
```

2.设计模型计算预测y  

```python
import torch
class Model(torch.nn.Module):
    def __init__(self) -> None:
        super().__init__()
        # (8,1)代表着它可以把任意8维的向量映射为1维向量空间上
        # 3个线性层
        self.linear1=torch.nn.Linear(8,6)
        self.linear2=torch.nn.Linear(6,4)
        self.linear3=torch.nn.Linear(4,1)
        self.sigmoid=torch.nn.Sigmoid()
        # 激活函数
        self.activate=torch.nn.ReLU()
    def forward(self,x):
        # 一直用x，管道上次输出作为下次输入
        x=self.sigmoid(self.linear1(x))
        x=self.sigmoid(self.linear2(x))
        x=self.sigmoid(self.linear3(x))
        return x
model=Model()
```

3.构造损失函数和优化器(利用了pytorchAPI) 

```python
criterion=torch.nn.BCELoss(size_average=True)
optimizer=torch.optim.SGD(model.parameters(),Ir=0.1)
```

4.训练周期(前馈，反馈，更新)

```python
for epoch in range(100):
    # 前馈步骤
    y_pred=model(x_data)
    loss=criterion(y_pred,y_data)
    print(epoch,loss.item())
    optimizer.zero_grad()
    # 反馈步骤
    loss.backward()
    # 更新
    optimizer.step() # 进行更新，根据预先的梯度，学习率
```

## 7.加载数据集

### mini-batch的各项定义

```python
#训练周期
for epoch in range(traing_epochs):
    # 一个周期的迭代
    for i in range(total_batch):
```

**Epoch**:对所有训练实例进行一次正向传递和一次反向传递。

**Batch-Size**:每次训练所用的样本数量

**iteration**:`Batch-Size`的数量

### DataLoader介绍:



![image-20221122164013793](https://i0.hdslb.com/bfs/album/2314f90cae29b8239c866d62290dfdc28b5861a3.png)

**DataLoader**作用是一个数据集支持索引，知道长度

1.shuffle随机打乱提高**随机性**

2.进行分组成一个可迭代`Batch`

3.**Pytorch的DataLoader()是一个 iterable**,关于iterable(可迭代对象)、iterator(迭代器)、generator(生成器)和yield和next的使用可以参考这篇[博客](https://blog.csdn.net/takedachia/article/details/123931246)

```python
input_batch,label_batch=next(iter(trainHLdataset))
```

### 详细代码实现

1.准备数据集

```python
import numpy as np
import torch
# DataLoader是一个用来帮助我们在PyTorch中加载数据的类
# Dataset是一个抽象的类。我们可以从这个类中定义我们的继承类。
from torch.utils.data import Dataset,DataLoader
class DiabetesDataset(Dataset):
    def __init__(self,filepath):
        xy=np.loadtxt(filepath,delimiter=',',dtype=np.float32)
        self.len=xy.shape[0]
        self.x_data=torch.from_numpy(xy[:,:-1])
        self.y_data=torch.from_numpy(xy[:,[-1]])
    # 为了能表达dataset[index]
    def __getitem__(self, index):
        return self.x_data[index],self.y_data[index]
    # 为了返回数据集的长度
    def __len__(self):
        return self.len
dataset=DiabetesDataset('diabetes.csv.gz')
# num_worker多线程数
train_loader=DataLoader(dataset=dataset,batch_size=32,shuffle=True,num_workers=0)
```

2.设计模型计算预测y  

```python
class Model(torch.nn.Module):
    def __init__(self):
        super().__init__()
        self.linear1=torch.nn.Linear(8,6)
        self.linear2=torch.nn.Linear(6,4)
        self.linear3=torch.nn.Linear(4,1)
        self.sigmoid=torch.nn.Sigmoid()
    def forward(self,x):
        x=self.sigmoid(self.linear1(x))
        x=self.sigmoid(self.linear2(x))
        x=self.sigmoid(self.linear3(x))
        return x
model=Model()
```

3.构造损失函数和优化器(利用了pytorchAPI) 

```python
criterion=torch.nn.BCELoss(reduction='sum')
optimizer=torch.optim.SGD(model.parameters(),lr=0.01)
```

4.训练周期(前馈，反馈，更新)

```python
for epoch in range(100):
    # 封装train_loader(x,y),i代表第几次迭代，
    for i,data in enumerate(train_loader,0):
        inputs,labels=data
        y_pred=model(inputs)
        loss=criterion(y_pred,labels)
        print(epoch,i,loss.item())
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
```

### 课后作业kaggle泰坦尼克号

1.准备数据集

对于大部分`csv`文件我们用`pandas.read_csv`,对于多特征的数据集将`feature`定义一个列表，`pd.read_csv`转变成`array`再用`torch.from_numpy`

```python
import numpy as np
import pandas as pd
import torch
from torch.utils.data import Dataset, DataLoader
# 准备数据集
class TitanicDataset(Dataset):
    def __init__(self, filepath):
        xy = pd.read_csv(filepath)
        # xy.shape（）可以得到xy的行列数
        self.len = xy.shape[0]
        # 选取相关的数据特征
        feature = ["Pclass", "Sex", "SibSp", "Parch", "Fare"]
        # np.array()将数据转换成矩阵，方便进行接下来的计算
        self.x_data = torch.from_numpy(np.array(pd.get_dummies(xy[feature])))
        self.y_data = torch.from_numpy(np.array(xy["Survived"]))
 
    # getitem函数，可以使用索引拿到数据
    def __getitem__(self, index):
        return self.x_data[index], self.y_data[index]
 
    # 返回数据的条数/长度
    def __len__(self):
        return self.len
 
# 实例化自定义类，并传入数据地址
dataset = TitanicDataset('train.csv')
# num_workers是否要进行多线程服务，num_worker=2 就是2个进程并行运行
# 采用Mini-Batch的训练方法
train_loader = DataLoader(dataset=dataset, batch_size=1, shuffle=True, num_workers=0)
```

这里之所以采用**pd.get_dummies**

<img src="https://i0.hdslb.com/bfs/album/0c2b182ee8f1db48b00afe7664bef4a1c84d6e79.png" alt="image-20221123193248565" style="zoom:50%;" />

==有的特征的值不是数字，通过**pd.get_dummies**增加特征数，将非数字值转成数字值==

<img src="https://i0.hdslb.com/bfs/album/a5986b4e5dc36fe1bca46a973fe56786f395c265.png" alt="image-20221123193445222" style="zoom:50%;" />

2.定义模型

```python
# 定义模型
class Model(torch.nn.Module):
    def __init__(self):
        super(Model, self).__init__()
        # 要先对选择的特征进行独热表示计算出维度，而后再选择神经网络开始的维度
        self.linear1 = torch.nn.Linear(6, 3)
        self.linear2 = torch.nn.Linear(3, 1)
 
        self.sigmoid = torch.nn.Sigmoid()
 
    # 前馈
    def forward(self, x):
        x = self.sigmoid(self.linear1(x))
        x = self.sigmoid(self.linear2(x))
        return x
   	# 测试函数
    def test(self, x):
        with torch.no_grad():
            x=self.sigmoid(self.linear1(x))
            x=self.sigmoid(self.linear2(x))
            y=[]
            # 根据二分法原理，划分y的值
            for i in x:
                if i >0.5:
                    y.append(1)
                else:
                    y.append(0)
            return y
# 实例化模型
model = Model()
```

**with torch.no_grad()**在该模块下，所有计算得出的tensor的requires_grad都自动设置为False，即使一个tensor（命名为x）的requires_grad = True

==对于分类问题的测试结果常常采用二分法获得==

3.定义损失函数和优化器

```python
# 定义损失函数
criterion = torch.nn.BCELoss(reduction='mean')
# 定义优化器
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)
```

* reduction = ‘none’，直接返回向量形式的 loss。
* 如果reduction = ‘sum’，返回loss之和。
* 如果reduction = ''elementwise_mean，返回loss的平均值。
* 如果reduction = ''mean，返回loss的平均值

 4.训练

```python
# 防止windows系统报错
if __name__ == '__main__':
    # 采用Mini-Batch的方法训练要采用多层嵌套循环
    # 所有数据都跑100遍
    for epoch in range(100):
        # data从train_loader中取出数据（取出的是一个元组数据）:（x，y）
        # enumerate可以获得当前是第几次迭代，内部迭代每一次跑一个Mini-Batch
        for i, data in enumerate(train_loader, 0):
            # inputs获取到data中的x的值，labels获取到data中的y值
            x, y = data
            # 这里将数值转成float
            x = x.float()
            y = y.float()
            y_pred = model(x)
            # squeeze(-1)是将n*1的shape变成n
            y_pred = y_pred.squeeze(-1)
            loss = criterion(y_pred, y)
            print(epoch, i, loss.item())
            optimizer.zero_grad()
            loss.backward()
            optimizer.step()
```

5.测试

```python
test_data=pd.read_csv('test.csv')
feature = ["Pclass", "Sex", "SibSp", "Parch", "Fare"]
test=torch.from_numpy(np.array(pd.get_dummies(test_data[feature])))
y=model.test(test.float()) #记得转成float
```



6.预测输出结果

```python
# 输出预测结果
output=pd.DataFrame({'PassengerId':test_data.PassengerId,'Survived':y})
output.to_csv('my_predict.csv',index=False)
```

要将预测的结果保存起来使用DataFrame.to_csv()

## 8.多分类问题

1.怎么用softmax的分类解决分类问题？

2.怎么用pytorch实现？

以MNIST数据为例子，**10个分类问题**，怎样设计神经网络？

分类问题结果类别中**概率最大**的那个，但是这样会造成冲突矛盾，例如p(1)=0.8,p(3)=0.9。所以我们希望输出的结果是**相互竞争**对抗并且构成的神经网络是一个**分布** 

![image-20221123203656979](https://i0.hdslb.com/bfs/album/5a007ddfb3a64050b24de87c25a4491bee09af9f.png)

**softmax函数公式**:

​                   **$P(y=i)=\frac {e^{Z_i}}{\sum\limits_{j=0}^{K-1}e^{Z_j}}$**    $i∈\{0,...,K-1\}$

原来**二分类**的loss函数公式是:

$loss=-\frac {1}{N}\sum\limits_{n=1}^Ny_nlog\hat{y}_n+(1-y)log(1-\hat{y})$

![image-20221123211237062](https://i0.hdslb.com/bfs/album/8a40ff4c435167ee5595f289a1a6f355c2537379.png)

在多分类问题中特征很多时，且值并非数值时，可以采用`pd.factorize()`

```python
>>> codes, uniques = pd.factorize(['b', 'b', 'a', 'c', 'b'])
>>> codes
array([0, 0, 1, 2, 0]...)
>>> uniques
array(['b', 'a', 'c'], dtype=object)
```

在pytorch里，对于多分类问题我们使用`nn.CrossEntropyLoss()`和`nn.NLLLoss`等来计算softmax**交叉熵**

`CrossEntropyLoss`会自动为其编码为`one-hot`编码，这样就会导致其升高一维，所以label的数据得是**一维**，不然会出现`0D or 1D target tensor expected, multi-target not supported`错误

numpy实现

```python
import numpy as np
y=np.array([1,0,0])
z=np.array([0.2,0.1,-0.1])
y_pred=np.exp(z)/np.exp(z).sum()
loss=(-y*np.log(y_pred)).sum()
loss
```

pytorch实现

```python
import torch
y=torch.LongTensor([0])
z=torch.Tensor([[0.2,0.1,-0.1]])
criterion=torch.nn.CrossEntropyLoss() #交叉熵损失包含softmax
# 这里的y的值代表第几个数字是1
loss=criterion(z,y)
loss.item()
```

```python
Y=torch.LongTensor([2,0,1])
# Y_pred1跟Y符合估计loss小
Y_pred1=torch.Tensor([[0.1,0.2,0.9],[1.1,0.1,0.2],[0.2,2.1,0.1]])
Y_pred2=torch.Tensor([[0.8,0.2,0.3],[0.2,0.3,0.5],[0.2,0.5,0.5]])
# Y_pred2跟Y差距打估计loss大
l1=criterion(Y_pred1,Y)
l2=criterion(Y_pred2,Y)
l1,l2
(tensor(0.4966), tensor(1.1720))
```

### 代码实现

```python
import torch
from torchvision import transforms #一个主要针对图像的工具
from torchvision import datasets
from torch.utils.data import DataLoader
# 优化器包
import torch.optim as optim
```

```python
batch_size=64
transform=transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize((0.1307),(0.3081))
])
```

#### torch.argmax

对于多分类问题的比较最后特征的概率最大的那个坐标可以使用`torch.argmax()`

```python
y_pred=model(input_batch)
torch.argmax(y_pred,dim=1) #dim代表维度
```

#### 模板代码

模型的训练过程，对于不同模型而言，都是类似的，因此我们将编写一个**fit**函数，输入模型、输入数据(train_dataset,test_dataset)，对数据输入在模型上训练，并且返回loss和acc变化

```python
from sklearn.model_selection import train_test_split
# 划分为训练解和测试集
train_x,test_x,train_y,test_y=train_test_split(X,Y)
traindataset=TensorDataset(train_x,train_y)
trainHLdataset=DataLoader(traindataset,batch_size=8,shuffle=True)
testdataset=TensorDataset(test_x,test_y)
testHLdataset=DataLoader(testdataset,batch_size=8)

```

这里我们分别计算训练和测试的损失和精确度，精确度采用正确数/总数 ，损失等于总损失/样本数

```python
def fit(epoch,model,trainloader,testloader):
    correct=0
    total=0
    running_loss=0
    for x,y in trainloader:
        y_pred=model(x)
        ls=loss(y_pred,y)
        opt.zero_grad()
        ls.backward()
        opt.step()
        with torch.no_grad():
            # 计算每个正确的个数
            correct+=(torch.argmax(y_pred,dim=1).type(torch.float32)==y).type(torch.float32).sum().item()
            total+=len(y)
            running_loss+=ls.item()
    epoch_loss=running_loss/len(trainloader.dataset)
    epoch_acc=correct/total
    test_correct=0
    test_total=0
    test_running_loss=0
    with torch.no_grad():
        for x,y in testloader:
            y_pred=model(x)
            ls=loss(y_pred,y)
            # 计算每个正确的个数
            test_correct+=(torch.argmax(y_pred,dim=1).type(torch.float32)==y).type(torch.float32).sum().item()
            test_total+=len(y)
            test_running_loss+=ls.item()     
    epoch_test_loss=test_running_loss/len(testloader.dataset)
    epoch_test_acc=test_correct/test_total
    print('epoch',epoch,'train_loss=',round(epoch_loss,3),'train_acc=',round(epoch_acc,3))
    print('epoch',epoch,'test_loss=',round(epoch_test_loss,3),'test_acc=',round(epoch_test_acc,3))
    return epoch_loss,epoch_acc,epoch_test_loss,epoch_test_acc
```

可以直接带入计算

```python
model=net()
loss=torch.nn.CrossEntropyLoss() #这里是多分类交叉熵
opt=torch.optim.Adam(model.parameters(),lr=0.001)
train_loss,train_acc,test_loss,test_acc=[],[],[],[]
for epoch in range(20):
    epoch_loss,epoch_acc,test_epcoh_loss,test_epoch_acc=fit(epoch,model,trainHLdataset,testHLdataset)
    train_loss.append(epoch_loss)
    train_acc.append(epoch_acc)
    test_loss.append(test_epcoh_loss)
    test_acc.append(test_epoch_acc)
```



#### transforms详解

**pytorch**读取图片用的是**python**的**PIL Image**,神经网络希望读取出来的数据比较小，最好在==[-1,1]之间==遵从一个**正态分布**这样对训练**神经网络**最好

<img src="https://i0.hdslb.com/bfs/album/ad66c09d2ca402c41cd496af011119fe09cde15a.png" alt="image-20221123220416227" style="zoom:50%;" />

将0-255的值压缩到**0-1的浮点数**，从Z变成R的**张量**

黑白灰度图是单通道，彩色图片是多通道分rgb

<img src="https://i0.hdslb.com/bfs/album/f4a11b938d9d014240491a9160efcac83ebb9d4b.png" alt="image-20221123220607584" style="zoom: 33%;" />

一般**PIL**读出来的图片一般是**W×H×C**，在pytorch要转变成**C×W×H**

我们在numpy中消除维度为1的方法是**np.squeeze()**,对于大小不同的图片可以使用**transforms.Resize()**,这些都是通过**transforms.ToTensor()**完成的,**transforms.Normalize()**:归一化

​		$Pixel_{norm}=\frac {Pixel_{orgin}-mean}{std}$

这样算的每个值服从[0,1]正态分布

下载数据集

```python
train_dataset=datasets.MNIST(root='./dataset/mnist/',train=True,download=True,transform=transform)
train_loader=DataLoader(train_dataset,shuffle=True,batch_size=batch_size)
test_dataset=datasets.MNIST(root='./dataset/mnist/',download=True,train=False,transform=transform)
test_loader=DataLoader(test_dataset,shuffle=False,batch_size=batch_size)
```

设计模型

```python
class MNIST_Net(torch.nn.Module):
    def __init__(self) -> None:
        super().__init__()
        self.linear1=torch.nn.Linear(784,512)
        self.linear2=torch.nn.Linear(512,256)
        self.linear3=torch.nn.Linear(256,128)
        self.linear4=torch.nn.Linear(128,64)
        self.linear5=torch.nn.Linear(64,10)
        self.relu=torch.nn.ReLU()
    def forward(self,x):
        x=x.view(-1,784)
        x=self.relu(self.linear1(x))
        x=self.relu(self.linear2(x))
        x=self.relu(self.linear3(x))
        x=self.relu(self.linear4(x))
        # 最后一层不做激活，交叉熵自带激活
        return self.linear5(x)
model=MNIST_Net()
```

这里激活层我们采用**ReLU**层，关于**ReLU**的优点可以看这篇[博客](https://blog.csdn.net/m0_62051288/article/details/122674207?ops_request_misc=%7B%22request%5Fid%22%3A%22166925360316800186574191%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=166925360316800186574191&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-122674207-null-null.142^v66^control,201^v3^control,213^v2^t3_esquery_v2&utm_term=relu激活函数&spm=1018.2226.3001.4187)，全连接的神经网络要求输入时一个矩阵，这里矩阵变形常采用view和reshape，它们之间区别可以看这篇[博客](https://blog.csdn.net/Flag_ing/article/details/109129752)

3.构造损失函数和优化器(利用了pytorchAPI) 

```python
criterion=torch.nn.CrossEntropyLoss()
# 模型较大，所以带了冲量
optimizer=optim.SGD(model.parameters(),lr=0.01,momentum=0.5)
```

这里的优化器多加了**Momentum(冲量)**，冲量本质一段时间内的梯度向量进行了加权平均，得到梯度更新过程中 ![w](https://private.codecogs.com/gif.latex?%5Cdpi%7B120%7D%20w) 和 ![b](https://private.codecogs.com/gif.latex?%5Cdpi%7B120%7D%20b) 的大致走向，一定程度上消除了更新过程中的不确定性因素（如摆动现象)，使得梯度更新朝着一个越来越明确的方向前进。详细看这篇[博客](https://blog.csdn.net/qq_36575363/article/details/109565732),

4.训练

```python
# 一轮循环封装成专门的训练函数
def train(epoch):
    running_loss=0
    for batch_idx,data in enumerate(train_loader,0):
        inputs,target=data
        optimizer.zero_grad()
        outputs=model(inputs)
        loss=criterion(outputs,target)
        loss.backward()
        optimizer.step()
        running_loss+=loss.item()
        # 每300次迭代输出一下
        if batch_idx % 300==299:
            print('[%d,%5d] loss: %.3f' % (epoch+1,batch_idx+1,running_loss/300))
            running_loss=0
```

5.测试

```python
def test():
    correct=0
    total=0
    with torch.no_grad():
        for data in test_loader:
            images,labels=data
            outputs=model(images)
            # torch.max返回两个值一个是最大值，一个最大值坐标
            _,predicted=torch.max(outputs.data,dim=1)
            # 这里采用.size就算多少行就是多少个数
            total+=labels.size(0)
            # 这里利用是否相等的0，1加和判断
            correct+=(predicted==labels).sum().item()
    print('Accuracy on test set:%d %%'%(100*correct/total))
if __name__=='__main__':
    for epoch in range(10):
        train(epoch)
        test()
```

```shell
[1,  300] loss: 2.176
[1,  600] loss: 0.894
[1,  900] loss: 0.440
Accuracy on test set:89 %
[2,  300] loss: 0.333
[2,  600] loss: 0.263
[2,  900] loss: 0.223
Accuracy on test set:94 %
[3,  300] loss: 0.189
[3,  600] loss: 0.168
[3,  900] loss: 0.150
Accuracy on test set:97 %
[7,  300] loss: 0.056
...
[10,  300] loss: 0.031
[10,  600] loss: 0.031
[10,  900] loss: 0.031
Accuracy on test set:97 %
```

### kaggle课后作业

## 9. 卷积神经网络(基础篇)

### CNN基础

一张图片在计算机眼里是这样的

![image-20230101172109054](https://i0.hdslb.com/bfs/album/755959214976eb843da08280be4e2df45348ca14.png)

当计算机看到一张图像（输入一张图像）时，它看的是一大堆**像素值**。

计算机可以通过寻找诸如边缘和曲线之类的**低级特点**来分类图片，继而通过一系列卷积层级建构出更为**抽象**的概念。这是 CNN（卷积神经网络）工作方式的大体概述

一张图片如何作为一个模型的输入呢？一张图片对电脑来说是一个3维的tensor，把一个3维的tensor**拉直**就可以放进模型里面

<img src="https://i0.hdslb.com/bfs/album/9b0fd84d41a821efda79688ffff16034582de3c1.png" alt="image-20230104161931718" style="zoom: 33%;" />

### CNN简化原理

CNN第一个简化是通过通过判断关键pattern来判断整个图像，只需要图片一小部分作为输入



<img src="https://i0.hdslb.com/bfs/album/98c403923b0d68d253acfc56b9a3c56af65f79a6.png" alt="image-20230104170744621" style="zoom:50%;" />

具体操作就是这样

<img src="https://i0.hdslb.com/bfs/album/65bc3c153da9222029dd67ddd017b6317eab78eb.png" alt="image-20230104171255484" style="zoom:50%;" />

CNN第二个简化是在一样的patterns的不同区域**共享**一样的参数

<img src="https://i0.hdslb.com/bfs/album/631dd62a98d6d62822c25065e7cf651606ba181d.png" alt="image-20230104212013522" style="zoom:50%;" />

<img src="https://i0.hdslb.com/bfs/album/dfe697faa25681a5e95963761a06a7a6e7e7b3ac.png" alt="image-20230104212209646" style="zoom:50%;" />

CNN就是全连接加**Recepitive Field(接受域)和parameter sharing来实现的

<img src="https://i0.hdslb.com/bfs/album/294e8d7d1e1e4761a375e3eda6cbeb6383c87a63.png" alt="image-20230104213728841" style="zoom: 33%;" />



### 二维卷积神经网络

![image-20221129162244050](https://i0.hdslb.com/bfs/album/0fd25c31ab570fbd0ffe3ea283ad0250ab6940e7.png)

**卷积层(Convolution) 保留空间特征，通道，宽高都会变，因为全连接后会丧失** 空间结构，每个卷积层提取一部分特征

**非线性层** :必要

**池化下采样(Subsampling)**:操作后的通道数是不变的，变的是图像的宽度和高度，减少数据量减低运算需求，比如图像像素太高，适当降低提取特征

**特征提取器(Feature Extraction)**::通过卷积运算找到某种特征

**分类器(Classification)**:转换成向量，通过全连接网络做分类

总之通过卷积层和池化层，CNN 模型可以使用卷积核（kernel）对图像进行卷积，提取出图像的**局部特征**。这些局部特征可以表示图像中的纹理、边缘等信息，对于图像分类很有用，而普通的全连接是**整体特征**

### 图像的卷积

卷积是指将卷积核应用到某个张量的**所有**点上，通过将卷积核在输入的张量上**滑动**而生成经过滤波处理的张量。

GRB一般是作为输入通道的，卷积一般是对一个图像块Patch操作的，图像块patch是跨越3个通道的，从图像左上角坐标开始滑动，进行遍历，然后对每一个块进行卷积运算

<img src="https://i0.hdslb.com/bfs/album/bc13fc2ac6d811615afd92fc5df8050f21a1221b.png" alt="image-20221129163850601" style="zoom:50%;" />

卷积运算的相乘

<img src="https://i0.hdslb.com/bfs/album/c394fcc97dbcceae61f02893507bd93b57d5507c.png" alt="image-20221129164139087" style="zoom: 50%;" />

通常是一个通道一个卷积核

3通道相乘相加

<img src="https://i0.hdslb.com/bfs/album/a623fc226b0aa12b4792a5ea196ca630ccd2700e.png" alt="image-20221129164248270" style="zoom:50%;" />

![image-20221129164617414](https://i0.hdslb.com/bfs/album/59f79916f3ec72c794945aeb5689d10e28cc36e5.png)

torch.nn.Conv1d，torch.nn.Conv2d和torch.nn.Conv3d的应用和相关计算可以看这篇[博客](https://blog.csdn.net/comli_cn/article/details/112556409)

这里的输出通道都是1个，如果我们要输出的通道是m个，需要m组卷积核

<img src="https://i0.hdslb.com/bfs/album/233ee35551d34b30c9f7500a761bb1f19437c0e1.png" alt="image-20221129165012671" style="zoom: 50%;" />

```python
import torch
in_channels,out_channels=5,10
width,height=100,100
kernel_size=3
batch_size=1
input=torch.randn(batch_size,in_channels,width,height)
# 创建卷积层,参数为输入通道，输出通道数和卷积核大小跟图像大小没关系
conv_layer=torch.nn.Conv2d(in_channels,out_channels,kernel_size=kernel_size)
output=conv_layer(input)
print(input.shape)
print(output.shape)
print(conv_layer.weight.shape)
```

torch.nn.Conv2d padding参数讲解

![image-20221129171307761](https://i0.hdslb.com/bfs/album/4be32014bf338d2f8930d9d6b3c2fecb18616840.png)

```python
import torch
input=[3,4,5,6,7,2,4,6,8,2,1,6,7,8,4,9,4,6,2,3,7,5,4,1]
input=torch.Tensor(input).view(1,1,5,5)
conv_layer=torch.nn.Conv2d(1,1,kernel_size=3,padding=1,bias=False)
kernel=torch.Tensor([1,2,3,4,5,6,7,8.9]).view(1,1,3,3)
# 对卷积权重数据做初始化
conv_layer.weight.data=kernel.data
output=conv_layer(input)
output
```

torch.nn.Conv2d stripe参数可以降低宽高

代表每次图像块Patch遍历滑动的步数  stripe=2

<img src="https://i0.hdslb.com/bfs/album/53544f67452eec0229fe6fdef98f196b550d3406.png" alt="image-20221129172150257" style="zoom:50%;" />

```python
import torch
input=[3,4,6,5,7,2,4,6,8,2,1,6,7,8,4,9,7,4,6,2,3,7,5,4,1]
input=torch.Tensor(input).view(1,1,5,5)
conv_layer=torch.nn.Conv2d(1,1,kernel_size=3,stride=2,bias=False)
kernel=torch.Tensor([1,2,3,4,5,6,7,8,9]).view(1,1,3,3)
conv_layer.weight.data=kernel.data
output=conv_layer(input)
output,output.shape
(tensor([[[[211., 262.],
           [251., 169.]]]], grad_fn=<ConvolutionBackward0>),
 torch.Size([1, 1, 2, 2]))
```

### feature map
**feature map的含义:**
	在每个卷积层，数据都是以三维形式存在的。你可以把它看成许多个二维图片叠在一起，其中每一个称为一个`feature map`。在输入层，如果是灰度图片，那就只有一个`feature map`；如果是彩色图片，一般就是3个feature map（红绿蓝）。层与层之间会有若干个卷积核（kernel），上一层和每个`feature map`跟每个卷积核做卷积，都会产生下一层的一个`feature map`。
**feature map尺寸计算方法:**
	INPUT为32`*`32，filter的大小即kernel size为5`*`5，stride = 1，pading=0,卷积后得到的feature maps边长的计算公式是: 
	output_h =（originalSize_h+padding`*`2-kernelSize_h）/stride +1 
	所以，卷积层的feature map的变长为:conv1_h=（32-5）/1 + 1 = 28 
	卷积层的feature maps尺寸为28`*`28. 
层与层之间会有若干个卷积核(kernel)(也称为==过滤器==)
### 下采样

**Max Pooling Layer*(最大池化层)*

分成2×2在同一通道找最大值，是在同一组中，所以通道数量不变

<img src="https://i0.hdslb.com/bfs/album/f9d706ac5bae782a51b1356fccfbc308115dc0a2.png" alt="image-20221129173358821" style="zoom:50%;" />

```python
import torch
input=[3,4,6,5,2,4,6,8,1,6,7,8,4,9,7,6]
input=torch.Tensor(input).view(1,1,4,4)
# kernel_size=2,那么默认stride=2
maxpooling_layer=torch.nn.MaxPool2d(kernel_size=2)
output=maxpooling_layer(input)
output
```

一个简单的神经网络

![image-20221129174403808](https://i0.hdslb.com/bfs/album/fcea5e4514350bd88926c9d4aaeb13dc8cabd2aa.png)

全连接神经网络

![image-20221129174822228](https://i0.hdslb.com/bfs/album/123a25586bc31c23cf89967684a8f0b355c7ab80.png)

```python
class Net(torch.nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1=torch.nn.Conv2d(1,10,kernel_size=5)
        self.conv2=torch.nn.Conv2d(10,20,kernel_size=5)
        self.pooling=torch.nn.MaxPool2d(2)
        self.fc=torch.nn.Linear(320,10)
        self.relu=torch.nn.ReLU()
    def forward(self,x):
        # Flatten data from (n,1,28,28) to (n,78)
        batch_size=x.size(0)
        x=self.relu(self.pooling(self.conv1(x)))
        x=self.relu(self.pooling(self.conv2(x)))
        # 这一步记得变化
        x=x.view(batch_size,-1)
        x=self.fc(x)
        return x
model=Net()
```



### 将计算迁到GPU中

1.模型迁移到GPU

```python
# 将计算迁到GPU
model=Net()
device=torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
# 将整个模型参数缓存都放到CUDA里面
model.to(device)
```

```python
def train(epoch):
    running_loss=0.0
    for batch_idx,data in enumerate(train_loader,0):
        inputs,target=data
        # 将数据发送到GPU
        inputs,target=inputs.to(device),target.to(device)
        optimizer.zero_grad()
        outputs=model(inputs)
        loss=criterion(outputs,target)
        loss.backward()
        optimizer.step()
        running_loss+=loss.item()
        if batch_idx% 300==299:
            print('[%d,%5d] loss:%.3f' %(epoch+1,batch_idx+1,running_loss/2000))
            running_loss=0.0
```

```python
def test():
    correct=0
    total=0
    with torch.no_grad():
        for data in test_loader:
            inputs,target=data
            # 将数据发送到GPU
            inputs,target=inputs.to(device),target.to(device)
            outputs=model(inputs)
            _,predicted=torch.max(outputs.data,dim=1)
            total+=target.size(0)
            correct+=(predicted==target).sum().item()
    print('Accuracy on test set:%d %% [%d %d]'%(100*correct/total,correct,total))
```

## 10.使用预训练网络（迁移学习）数据增强
### 预训练网络

预训练网络是一个保存好的之前已在大型数据集（大规模图像分类任务）上**训练好**的卷积神经网络，如果这个原始数据集足够大且足够**通用**，那么预训练网络学到的特征的空间层次结构可以作为有效的提取视觉世界特征的模型。

即使新问题和新任务与原始任务完全不同学习到的特征在不同问题之间是可移植的，这也是深度学习与浅层学习方法的一个重要优势。它使得深度学习对于小数据问题非常的有效。

### Pytorch内置预训练网络

Pytorch库中包含`VGG16`、`VGG19`、`densenet`、`ResNet`、`mobilenet`、`Inception v3`、`Xception`等经典的模型架构。

### ImageNet

ImageNet是一个**手动标注好类别**的图片数据库(为了机器视觉研究)，目前已有22,000个类别。

### VGG

`VGG`全称是Visual Geometry Group,属于牛津大学科学工程系，其发布了一些列以VGG开头的卷积网络模型，可以应用在**人脸识别、图像分类**等方面，分别从VGG16～VGG19。

在2014年，VGG模型架构由Simonyan和Zisserman提出，在“极深的大规模图像识别卷积网络”（Very DeepConvolutional Networks for Large Scale ImageRecognition）这篇论文中有介绍。

VGG研究卷积网络深度的初衷是想搞清楚卷积**网络深度**是如何影响大规模图像分类与识别的**精度和准确率**的，最初是VGG-16, 号称非常深的卷积网络全称为(GG-Very-Deep-16 CNN) 。

VGG模型结构简单有效，前几层仅使用3×3卷积核来增加网络深度，通过max pooling（最大池化）依次减少每层的神经元数量，最后三层分别是2个有4096个神经元的全连接层和一个softmax层。

![image-20230106112813907](https://i0.hdslb.com/bfs/album/3c826733d6199a6e71b3af4119492ecb3d5423ca.png)

VGG在加深网络层数同时为了避免参数过多，在所有层都采用3x3的小卷积核，卷积层步长被设置为1。VGG的输入被设置为224x244大小的RGB图像，在训练集图像上对所有图像计算RGB均值，然后把图像作为输入传入VGG卷积网络，使用3x3或者1x1的filter，卷积步长被固定1。

VGG全连接层有3层，根据卷积层+全连接层总数目的不同可以从VGG11 ～ VGG19，最少的VGG11有8个卷积层与3个全连接层，最多的VGG19有16个卷积层+3个全连接层，此外VGG网络并不是在每个卷积层后面跟上一个池化层，还是总数5个池化层，分布在不同的卷积层之下。

<img src="https://i0.hdslb.com/bfs/album/fe0b338a5ce27a0cd9d6cdbbd6d7706fae35c7c4.png" alt="image-20230106112958711" style="zoom:50%;" />

**conv**:表示卷积层
**FC**:表示全连接层(dense)
**Conv3** :表示卷积层使用3x3 filters
**conv3-64**:表示 深度64
**maxpool**:表示最大池化

VGG两大缺点

1. 网络架构weight数量相当大，很消耗磁盘空间。
2. 训练非常慢
    由于其全连接节点的数量较多，再加上网络比较深，VGG16
    有533MB+，VGG19有574MB。这使得部署VGG比较耗时

<img src="https://i0.hdslb.com/bfs/album/d40e585290bdea990f2b2fe8d13477e43816b446.png" alt="image-20230106113309456" style="zoom:50%;" />

预训练的模型通过上图方式应用到新的分类器中，丢弃原有的，冻结保留好原先的参数再应用到新的分类器

```python
model=torchvision.models.vgg16(pretrained=True) #使用vgg16的模型预训练
for p in model.features.parameters():
    p.requires_grad=False #冻结卷积层
model.classifier[-1]=4  #修改成新的分类器
opt=optim.Adam(model.classifier.parameters(),lr=0.001) #只优化分类器部分
```



### 微调

所谓微调:
共同训练新添加的**分类器层**和部分或者**全部卷积层**。这允许我们“微调”基础模型中的**高阶特征**表示，以使它们与特定任务更相关。即让高级部分的卷积训练新的分类器

只有分类器**已经训练好**了，才能微调卷积基的卷积层。如果有没有这样的话，刚开始的训练误差很大，微调之前这些卷积层学到的表示会被破坏掉。

微调步骤:

一、在预训练卷积基上添加自定义层
二、冻结卷积基所有层
三、训练添加的分类层
四、解冻卷积基的一部分层

五、联合训练解冻的卷积层和添加的自定义层

![](https://zh-v2.d2l.ai/_images/finetune.svg)

```python
# 训练好后解冻的卷积
for p in model.parameters():
    p.requires_grad=True
extend_epoch=30
opt=optim.Adam(model.parameters(),lr=0.0001)
```

在预训练模型上只更新新添加的层的参数的代码

```python
if param_group:
        params_1x = [param for name, param in net.named_parameters()
             if name not in ["fc.weight", "fc.bias"]]
        trainer = torch.optim.SGD([{'params': params_1x},
                                   {'params': net.fc.parameters(),
                                    'lr': learning_rate * 10}],
                                lr=learning_rate, weight_decay=0.001) # 只更新新添加的层的参数
    else:
        trainer = torch.optim.SGD(net.parameters(), lr=learning_rate,
                                  weight_decay=0.001)
```



### 数据增强

卷积[神经网络](https://so.csdn.net/so/search?q=神经网络&spm=1001.2101.3001.7020)非常容易出现过拟合的问题，而数据增强的方法是对抗过拟合问题的一个重要方法。数据增强是指在不增加真实数据的情况下，通过对现有数据进行**变换**或者**增添噪声**等方式，生成新的训练数据的过程。

数据增强的目的是为了使训练的模型更具有**泛化**能力，从而更好地拟合真实数据。

#### 常用的数据增强方法

1.对图片进行一定比例缩放
2.对图片进行随机位置的截取
3.对图片进行随机的水平和竖直翻转
4.对图片进行随机角度的旋转
5.对图片进行亮度、对比度和颜色的随机变化

这些方法 `pytorch`都已经为我们内置在了 torchvision 里面，

```python
from torchvision import datasets,transforms
transforms.RandomCrop #随机位置裁剪
transforms.CenterCrop #中心裁剪
transforms.RandomHorizontalFlip(p=1) # 随机水平翻转 p动作概率
transforms.RandomVerticalFlip(p=1) # 随机上下翻转
transforms.RandomRotation(45)(im) # 随机45度旋转
transforms.ColorJitter(brightness=0.6)(im) #亮度 随机从 0 ~ 2 之间亮度变化，1 表示原图
transforms.ColorJitter(contrast=1)(im) #对比度
transforms.ColorJitter(hue=0.5)(im) # 颜色 随机从 -0.5 ~ 0.5 之间对颜色变化
transforms.RandomGrayscale(p=0.5) # 随机灰度变化
```

添加数据增强一般放在训练的transforms中

```python
# 对训练的transform进行处理
train_transform=transforms.Compose([
    transforms.Resize(224),
    transforms.RandomCrop(192),#随机裁剪
    transforms.RandomHorizontalFlip(), #随机水平翻转
    transforms.RandomRotation(0.2),# 随机角度旋转
    transforms.ColorJitter(brightness=0.5),# 调节亮度对比度
    transforms.ColorJitter(contrast=0.5),
    transforms.ToTensor(),
    transforms.Normalize(mean=(0.5, 0.5, 0.5), std=(0.5, 0.5, 0.5))
])
test_transform=transforms.Compose([
    transforms.Resize((192,192)),
    transforms.ToTensor(),
    transforms.Normalize(mean=(0.5, 0.5, 0.5), std=(0.5, 0.5, 0.5))
])
train_dataset=torchvision.datasets.ImageFolder(train_dir,train_transform)
test_dataset=torchvision.datasets.ImageFolder(test_dir,test_transform)
train_loader=DataLoader(train_dataset,batch_size=16,shuffle=True,num_workers=4)
test_loader=DataLoader(test_dataset,batch_size=16,shuffle=False,num_workers=4)
train_dataset.classes,train_dataset.class_to_idx,len(train_dataset)
```

### 学习速率衰减

如下图所示当损失达到最低时应该让学习率减小以免跳过最优解，为此我们采用**学习速率衰减**

![image-20230106213139213](https://i0.hdslb.com/bfs/album/72a2fa7bde185f34c1c2a157291df973df85c25d.png)

学习速率衰减是指在深度学习中，对学习速率进行逐步减小的过程。

在训练深度学习模型时，一般都会设置初始的学习速率，然后在训练过程中，通过学习速率衰减来逐渐减小学习速率。学习速率衰减的目的是为了让模型在训练初期快速收敛，并且在训练后期保持较低的学习速率，从而更好地拟合训练数据。

在实际训练深度学习模型时，学习速率衰减的具体方式有很多，比如按照`时间步数衰减`、`按照迭代次数衰减`、`使用自适应学习速率等等`。

在pytorch内置了很多学习速率衰减

```python
from torch.optim import lr_scheduler #这个模块用于学习速率衰减
# 每间隔5步衰减0.9
exp_lr_scheduler=lr_scheduler.StepLR(opt,step_size=5,gamma=0.9)
# 按照迭代次数衰减 在第20、50、80次迭代进行衰减
lr_scheduler.MultiStepLR(opt,[20.50,80],gamma=0.9)
# optimizer:优化器的实例。
# mode:'min' 或 'max'，表示调度器是根据监测指标的最小值还是最大值来决定是否降低学习率。
# factor:学习率调整的因子。如果在多次评估之后监测指标没有改善，学习率将被乘以这个因子。
# patience:在经过这么多次评估之后，如果监测指标没有改善，才会降低学习率。
re_lr_scheduler=lr_scheduler.ReduceLROnPlateau(opt,'min',factor=0.1,patience=5)
```

### 模型权重保存

保存模型

`state_dict`就是一个简单的Python字典，它将模型中的可训练参数（比如weights和biases，batchnorm的running_mean、torch.optim参数等）通过将模型每层与层的参数张量之间一一映射，实现保存、更新、变化和再存储。

```python
PATH='my_model.pth'
torch.save(model.state_dict(),PATH) # 保存模型参数
# 创建新模型
new_model=net()
new_model.load_state_dict(torch.load(PATH)) # 加载新的权重
# 测试一次
```

如果要保存最好的那一次权重，需要在测试批次中，添加保存最佳模型参数，并让模型加载这个最佳的模型参数

```python
train_loss,train_acc,test_loss,test_acc=[],[],[],[]
best_pth=model.state_dict().copy() # 保存最好的模型参数字典用copy方法，不然是引用
best_acc=0 #用来记录最好的准确率
for epoch in range(20):
    epoch_loss,epoch_acc,test_epcoh_loss,test_epoch_acc=fit(epoch,model,train_loader,test_loader)
    # 开始比较准确率
    if test_epoch_acc>best_acc:
        best_acc=test_epoch_acc
        best_pth=model.state_dict().copy()
    train_loss.append(epoch_loss)
    train_acc.append(epoch_acc)
    test_loss.append(test_epcoh_loss)
    test_acc.append(test_epoch_acc)
# 模型加载这个最佳的模型参数
model.load_state_dict(best_pth) #关键一步
```

## 深度学习硬件

### 硬件知识

[计算机构成、中央处理器、**如何提升cpu的利用率？**，**网络和总线**，**cpu和gpu的对比**，**如何提升GPU的利用率？**](https://www.bilibili.com/h5/note-app/view?cvid=15911374&pagefrom=comment)

[DSP:数字信号处理、可编程阵列(FPGA)、**AI ASIC**、**Systolic Array**](https://www.bilibili.com/h5/note-app/view?cvid=19741157&pagefrom=comment&richtext=true)

### 多GPU运行

* 一台机器可以有多个GPU(1-16)
* 在训练和与预测时,我们将一个小批量计算**切分到**多个GPU上达到加速目的
* 常见切分方案有
  * 数据并行:将小批量分成n块,每个GPU拿到完整参数计算一块数据的梯度
    * 通常性能更好
  * ![image-20230214172246607](https://img1.imgtp.com/2023/02/14/7yH1xLxA.png)
  * 模型并行:将模型分成n块,每个GPU拿到一块模型计算它的前向和方向结果
    * 通常用于模型大到单GPU放不下
  * 通道并行(数据+模型并行)

### 分布式训练

![image-20230215004337287](https://img1.imgtp.com/2023/02/15/pvL4w29m.png)

* 每个计算服务器读取小批量中的一块
* 进一步将数据切分到每个GPU上
* 每个worker从参数服务器那里获取模型参数
* 复制参数到每个GPU上，每个计算自己的梯度
* 所有GPU上的梯度求和，传回服务器，每个服务器对梯度进行更新

![image-20230215004955489](https://img1.imgtp.com/2023/02/15/dOei5vxz.png)

<img src="https://img1.imgtp.com/2023/02/15/nxe4FYka.png" alt="image-20230215005121940" style="zoom:50%;" />

**总结**

* 当一个模型能用单卡计算时,通常使用数据并行扩展到多卡上
* 模型并行则在超大模型上

### 关键API

```python
d2l.try_gpu(i) #运用第i块gpu
# 将一个小批量数据均匀分布在多个GPU上
devices=[torch.device('cuda:0'),torch.device('cuda:1')]
split=nn.parallel.scatter(data,devices) # nn.parallel.scatter 是 PyTorch 中的一个函数，用于将一个大的张量或一个列表中的元素分散到多个设备上进行并行计算。
def split_batch(X,y,devices):
  """将x和y拆分到多个设备上"""
  assert X.shape[0]==y.shape[0]# 断言，假了直接异常
  return (nn.parallel.scatter(X,devices),nn.parallel.scatter(y,devices))
torch.cuda.synchronize() # 可以确保在调用之前的所有CUDA操作已经结束
```

`nn.DataParallel` 是 PyTorch 中的一个模型并行工具，可以将一个模型分散到**多个 GPU** 上进行训练。具体来说，`nn.DataParallel` 可以将一个模型复制到多个 GPU 上，并将一个大的输入张量分割成多个小的张量，每个小的张量被送到不同的 GPU 上进行前向传播和反向传播，最后将**多个 GPU 的梯度累加**到一起，更新模型参数。使用 `nn.DataParallel` 可以大大提高模型的训练速度。

使用 `nn.DataParallel` 可以非常方便地将一个单 GPU 的模型扩展到多 GPU 上进行训练。具体来说，使用 `nn.DataParallel` 只需要在模型初始化时对模型进行**包装**，然后将包装后的模型**传递给优化器**即可。下面是一个使用 `nn.DataParallel` 进行模型并行训练的例子:

```python
import torch
import torch.nn as nn
from torch.utils.data import DataLoader

# 定义模型
class Model(nn.Module):
    def __init__(self):
        super(Model, self).__init__()
        self.linear = nn.Linear(10, 1)

    def forward(self, x):
        return self.linear(x)

# 创建数据集和数据加载器
dataset = torch.randn(100, 10)
loader = DataLoader(dataset, batch_size=10)

# 创建模型并行对象
model = Model()
model_parallel = nn.DataParallel(model, device_ids=[0, 1, 2])

# 创建优化器
optimizer = torch.optim.SGD(model_parallel.parameters(), lr=0.1)

# 训练模型
for epoch in range(10):
    for batch in loader:
        optimizer.zero_grad()
        output = model_parallel(batch)
        loss = output.mean()
        loss.backward()
        optimizer.step()
```

在上面的代码中，`nn.DataParallel` 的参数 `device_ids` 指定了使用哪些 GPU 进行并行训练。在每个批次的训练过程中，`nn.DataParallel` 会自动将输入数据分割成多个小的张量，并将它们分别发送到指定的 GPU 上进行前向传播和反向传播。当梯度计算完成后，`nn.DataParallel` 会自动将各个 GPU 的梯度累加到一起，并使用累加后的梯度更新模型参数。

## 残差网络

### 存在的问题

#### 增加层的缺陷

在到达一定深度后加入更多层，模型可能产生梯度消失或爆炸问题

![image-20230108222823011](https://i0.hdslb.com/bfs/album/f14f98eb905c441f463228b5f223f7c36bb41da5.png)

从上图可以看出随机层数的增加，精度达到饱和，56层的误差大于20层的误差，模型开始**退化**了(退化会让训练误差也变大与**过拟合**区别)

#### 梯度消失

假设有一个预测模型:y=f(x)

* x:输入  
* f:表示神经网络模型
* y:输出

<img src="https://i0.hdslb.com/bfs/note/0d11b9d34bfe76d132e8a862c51de6271498238f.png" alt="image-20230108222823011" style="zoom:50%;" />

w:权重

蓝色部分

* 表示原有模型某一层的 w 的更新计算 **（输出 y 中省略了损失函数）**
* η:学习率

- y 对 w 的梯度不能太小，如果太小的话，η 无论多大都不会起作用，并且也会影响数值的稳定性

紫色部分:

* y'=g(f(x)) 表示使用**堆叠**的方式对原有的模型进行加深之后的模型
* 后面的部分表示 y' 对w的梯度,，经过链式法则展开之后:第二项 y‘ 关于 w 的梯度和之前蓝色部分的结果是一样的，没有任何变化；第一项 g(y) 关于 y 的梯度是新加的层的输出对输入的导数，**它和预测值与真实值之间的差别有关系**，==假设预测的值和真实值之间的差别比较小的话，第一项的值就会变得特别小（假设所加的层的拟合能力比较强，第一项就会变得特别小，在这种情况下，和第二项相乘之后，乘积的值就会变得特别小，也就是梯度就会变得特别小，就只能增大学习率，但可能增大也不是很有用，因为这是靠近底部数据层的更新，如果增加得太大，很有可能新加的层中的w就已经很大了，这样的话可能会导致数值不稳定）==
* 正是因为乘法的存在，所以如果中间有一项比较小的话，可能就会导致整个式子的乘积比较小，越到底层的话乘积就越小

绿色部分:

- y‘' = f(x) + g( f(x) ) 表示使用**残差连接**的方式对原有的模型进行加深之后的模型输出
- 使用加法的求导对模型表达式进行展开得到两项，第一项和前面所说的一样，就是蓝色的部分
- 对于这两项来说，就算第二项的值比较小，但还是有第一项的值进行补充（大数加上一个小数还是一个大数，但是大数乘以一个小数就可能变成小数），正是由于**跨层数据通路**的存在，模型底层的权重相比于模型加深之前不会有大幅度的缩小

### 现象解释

**问题:随着神经网络的不断加深，一定会带来好处吗？**

![](https://i0.hdslb.com/bfs/note/0ac7768cd6615d573bc815a13146a13f1bb6c754.png)

- 蓝色五角星表示**最优值**

- 标有Fi的闭合区域表示函数，闭合区域的面积代表**函数的复杂程度**，在这个区域中能够找到一个**最优的模型**（可以用区域中的一个点来表示，该点到最优值的距离可以用来衡量模型的好坏）

- 从上图中可以看出，随着函数的复杂度的不断增加，虽然函数的区域面积增大了，但是在该区域中所能找到的最优模型（该区域内的某一点）离最优值的距离可能会越来越远（也就是模型所在的区域随着函数复杂度的增加，逐渐偏离了原来的区域，离最优值越来越远）（**非嵌套函数（**non-nested function））

- 解决上述问题（模型走偏）的方法:每一次增加函数复杂度之后函数所覆盖的区域会包含原来函数所在的区域（**嵌套函数（nested function）**），只有当较复杂的函数类包含复杂度较小的函数类时，才能确保提高它的性能，如下图所示

  ![](https://i0.hdslb.com/bfs/note/32bc762e35801add4533f08119ab33836405dd08.png)

- 也就是说，增加函数的复杂度只会使函数所覆盖的区域在原有的基础上进行**扩充**，而不会偏离原本存在的区域

- 对于深度神经网络，如果能将新添加的层训练成**恒等映射（identify function）f(x) = x**，新模型和原模型将同样有效；同时，由于新模型可能得出更优的解来拟合训练数据集，因此添加层似乎更容易降低训练误差

  

### 核心原理

残差网络的核心思想是:==每个附加层都应该更容易地包含原始函数作为其元素之一==

由此，**残差块（residual blocks）**诞生了

![](https://i0.hdslb.com/bfs/note/c2e5b2e4475660b29a5166856b0c224e2ce52faf.png)

- 之前增加模型深度的方法都是**层层堆叠**的方法，ResNet的思想是在堆叠层数的同时不会增加模型的复杂度

- 上图中左侧表示一个正常块，右侧表示一个残差块

- **x**:原始输入

- **f(x)**:我们想要的理想映射（也是激活函数的输入）

- 对于正常块中来说，虚线框中的部分需要直接拟合出**理想映射 f(x)**；而对于残差块来说，同样的虚线框中的部分需要拟合出**残差映射 f(x) - x**

- 残差映射在现实中往往更容易优化

- 如果以恒等映射 f(x) = x 作为所想要学出的理想映射 f(x)，则只需要将残差块中虚线框内**加权运算的权重**和**偏置参数**设置为 0，f(x) 就变成恒等映射了

- 当f(x)-x越接近0时，相当于构造一条恒等映射，==设g(x)=f(x)-x,f(x)=g(x)+x,解决了上面的梯度消失问题==

- 在残差块中，输入可以通过**跨层数据线路**更快地向前传播 

  

![](https://i0.hdslb.com/bfs/note/b4cc2514f12775cff404a6d58d797f26e05763df.png)

- 左边是ResNet的第一种实现**（不包含1 \* 1卷积层的残差块）**，它直接将输入加在了叠加层的输出上面

- 右边是ResNet的第二种实现**（包含1 \* 1卷积层的残差块）**，它先对输入进行了1 * 1的卷积变换通道（改变范围），再加入到叠加层的输出上面

- ResNet沿用了VGG完整的3 * 3卷积层设计

- 残差块中首先有2个相同输出通道数的3 * 3卷积层，每个卷积层后面接一个批量归一化层和ReLu激活函数；通过跨层数据通路，跳过残差块中的两个卷积运算，将输入直接加在最后的ReLu激活函数前（==这种设计要求2个卷积层的输出与输入形状一样，这样才能使第二个卷积层的输出（也就是第二个激活函数的输入）和原始的输入形状相同，才能进行相加==）

- ==如果想要改变通道数，就需要引入一个额外的1 * 1的卷积层来将输入变换成需要的形状后再做相加运算（如上图中右侧含1 * 1卷积层的残差块==）

  ### ResNet架构

  ![](https://i0.hdslb.com/bfs/note/66451bde43056600da2c221f2fedb2175c1703ec.png)

- 第一种是**高宽减半的ResNet块**。第一个卷积层的步幅等于2，使得**高宽减半，通道数翻倍**（如上图下半部分所示）
- 第二种是**高宽不减半的RexNet块**，如上图上半部分所示，重复多次，所有卷积层的步幅等于1

通过**ResNet块数量**和**通道数量**的不同，可以得到不同的ResNet架构**ResNet-18**架构如下图所示

![](https://i0.hdslb.com/bfs/note/bd946a334e3f2e06e0326a83c9f9839a0d7cc058.png)

- ResNet架构类似于VGG和GoogLeNet的总体架构，但是替换成了ResNet块（ResNet块的每个卷积层后增加了批量归一化层）
- ResNet的前两层和GoogLeNet中的一样，也分成了**5个stage**:在输出通道数为64、步幅为2的7 * 7卷积层后，接步幅为2的3 * 3的最大汇聚层
- GoogLeNet在后面接了4由**Inception块**组成的模块；ResNet使用了4个由**残差块**组成的模块，每个模块使用若干个同样输出通道数的残差块，第一个模块的通道数同输入通道数一致；由于之前已经使用了步幅为2的最大汇聚层，所以无需减小高和宽；之后每个模块在第一个残差块里将上一个模块的通道数翻倍，并将高和宽减半
- 通过配置**不同的通道数和模块中的残差块数**可以得到不同的ResNet模型:**ResNet-18**:每个模块都有4个卷积层（不包含恒等映射的1 * 1卷积层），再加上第一个7 * 7卷积层和最后一个全连接层，一共有18层；还有更深的152层的**ResNet-152**

```python
class ResNetBasicBlock(nn.Module):
    def __init__(self,in_channels,out_channels,stride):
        super().__init__()
        # 两次卷积和BN
        self.conv1=nn.Conv2d(in_channels,out_channels,kernel_size=3,stride=stride,padding=1,bias=False)
        self.bn1=nn.BatchNorm2d(out_channels)
        self.conv2=nn.Conv2d(out_channels,out_channels,kernel_size=3,stride=stride,padding=1,bias=False)
        self.bn2=nn.BatchNorm2d(out_channels)
        self.stride=stride
    def forward(self,x):
        residual=x #保留原来的输入
        out=self.conv1(x)
        out=F.selu(self.bn1(out),inplace=True) #这里inplace是为了实现效率
        out=self.conv2(out)
        out=self.bn2(out)
        out+=residual #实现f(x)=g(x)+x
        return F.relu(out)
```



**总结**

- 残差块使得很深的网络**更加容易训练**（不管网络有多深，因为有跨层数据通路连接的存在，使得始终能够包含小的网络，因为跳转连接的存在，所以会先将下层的小型网络训练好再去训练更深层次的网络），甚至可以训练一千层的网络（只要内存足够，优化算法就能够实现）
- 学习**嵌套函数**是神经网络的理想情况，在深层神经网络中，学习另一层作为**恒等映射**比较容易
- 残差映射可以更容易地学习同一函数，例如将权重层中的参数近似为零
- 利用**残差块**可以训练出一个有效的深层神经网络:输入可以通过层间的**残余连接**更快地向前传播
- 残差网络对随后的深层神经网络的设计产生了深远影响，无论是卷积类网络还是全连接类网络，几乎现在所有的网络都会用到，因为只有这样才能够让网络搭建的更深

## 稠密连接网络（DenseNet）

### DenseNet原理

回想一下任意函数的泰勒展开式（Taylor expansion），它把这个函数分解成越来越高阶的项。在x接近0时，

![image-20230210174121337](https://img1.imgtp.com/2023/02/10/PDwCU3ce.png)

同样，ResNet将函数展开为

![image-20230210174103057](https://img1.imgtp.com/2023/02/10/ycPXXCEO.png)


也就是说，ResNet将f分解为两部分:一个简单的线性项和一个复杂的非线性项。 那么再向前拓展一步，如果我们想将f拓展成超过两部分的信息呢？ 一种方案便是DenseNet。

![image-20230210174223523](https://img1.imgtp.com/2023/02/10/gDzo1p4C.png)

ResNet和DenseNet的**关键区别**在于，DenseNet输出是***连接***（用图中的[,]表示）而不是如ResNet的简单相加。DenseNet更进
一步，它引入了每层与所有后续层的连接，即每一层都接收所有前置层的特征平面作为输入。

![image-20230210174445003](https://img1.imgtp.com/2023/02/10/HI6JkZCS.png)

![image-20230210174502268](https://img1.imgtp.com/2023/02/10/pb66jpYu.png)

### DenseNet结构

稠密网络主要由2部分构成:==*稠密块*（dense block）==和==*过渡层*（transition layer）==。 前者定义如何连接输入和输出，而后者则控制通道数量，使其不会太复杂，连接两个相邻的DenseBlock，并且通过Pooling使特征图大小降低

此结构主要有以下改进:对比于ResNet的**Residual Block**，创新性地提出**DenseBlock**，在每一个Dense Block中，任何两层之间都有直接的连接，也就是说，网络每一层的输入都是前面所有层输出的**并集**，每个层的特征图大小相同，层与层之间采用密集连接方式。

该层所学习的特征图也会被直接传给其后面所有层作为输入。通过密集连接，缓解梯度消失问题，加强特征传播，鼓励特征复用，极大的减少了参数量。

DenseNet 比其他网络效率更高，其关键就在于网络每层计算量的减少以及特征的重复利用。DenseNet则是让l层的输入直接影响到之后的所有层，DenseNet 每一层中 [x0,x1,...,xl−1] 就是将之前的 **feature map** 以通道的维度进行合并。由于每一层都包含之前所有层的输出信息，因此其只需要很少的特征图就够了。DneseNet的参数量较其他模型大大减少。

![image-20230210175411412](https://img1.imgtp.com/2023/02/10/ixRCdz8j.png)


 如何把一个PIL图像转成Numpy数组，并判断是否是灰度图片(==灰度只有宽高,没有channel==)，并将灰度图片扩展成三维,并将Numpy数组转换成PIL图像

```python
 def __getitem__(self, index) :
    img=self.imgs[index]
    label=self.labels[index]
    pil_img=Image.open(img) # 这里的img是图片地址
    np_img=np.asarray(pil_img,dtype=np.uint8) # 把一个PIL图像转成Numpy数组,np.uint8灰度范围
    if len(np_img.shape==2):#判断是否是灰度
      img_data=np.repeat(np_img[:,:,np.newaxis],3,axis=2) # 在第三个轴上重复三次
      pil_img=Image.fromarray(img_data) # 将Numpy数组转换成PIL图像
    img_tensor=transform(pil_img)
    return img_tensor,label
```



## 图像处理

### 常见图像处理的任务

1. **分类**

给定一幅图像，我们用计算机模型预测图片中有什么对象。

![image-20230211170030788](https://img1.imgtp.com/2023/02/11/mt1zBkFX.png)

2. **分类+定位**

   不仅需要我们知道图片中的对象是什么，还要在对象的附近画一个边框，确定该对象所处的位置。

   ![image-20230211170202574](https://img1.imgtp.com/2023/02/11/soyAdGTE.png)

3. 语义分割

   区分到图中每一点像素点，而不仅仅是矩形框框住。

   ![image-20230211170248741](https://img1.imgtp.com/2023/02/11/XF2FEOba.png)

4. 目标检测

​		目标检测简单来说就是回答图片里面有什
​		么？分别在哪里？（把它们用矩形框框住）是回答		图片里面有什么？分别在哪里？（把它们用矩形框		框住）

​		![image-20230211170429078](https://img1.imgtp.com/2023/02/11/xh1nhIDR.png)

5. 实例分割

   实例分割是目标检测和语义分割的结合。相对目标检测的边界框，实例分割可精确到物体的边缘；相对语义分割，实例分割需要标注出图上同一物体的
   不同个体。

   ![image-20230211170511438](https://img1.imgtp.com/2023/02/11/0KdZV2i4.png)

### 图像定位

对于单纯的**分类**问题，比较容易理解，给定一幅图片，我们输出一个标签类别，我们已经跟熟悉。而**定位**有点复杂，需要输出四个数字（x，y，w，h），图像中某一个点的坐标（x，y），以及图像的宽度和高度，有了这四个数字，我们可以很容易地找到物体的边框。

**边缘框（boundingbox）**

- 在目标检测中，通常使用边界框来描述对象的空间位置
- 边界框是**矩形**的

边缘框可以用**四个数字**来定义（两种常用的表示方法）

- **（左上x，左上y，右下x，右下y）**
- **（中心x，中心y，宽，高）**

**正方向**

- 对于 x 轴来说，向右为 x 轴的正方向，即 x 的值从左到右依次增大
- 对于 y 轴来说，向下为 y 轴的正方向，y 的值从上到下依次增大

<img src="https://img1.imgtp.com/2023/02/15/14KqDQYB.png" alt="image.png" style="zoom:50%;" />

<img src="https://img1.imgtp.com/2023/02/11/Yn4vlvTJ.png" alt="image.png" style="zoom:75%;" />

![](https://img1.imgtp.com/2023/02/14/t6ywwePb.png)

#### anchor

- 提出多个被称为anchor的区域
- 预测每anchor里面是否含有关注的物体
- 如果有，则预测从这个anchor到GT anchor的偏移

**IoU**

![](https://pic1.zhimg.com/80/v2-4d64a6222851a6de434048d23ee59fb4_1440w.jpg)

**赋予anchor标号（其中一种）**

每个anchor是一个训练样本

将每个anchor，要么标注成bg，要么关联上一个真实的边框

会生成大量的anchor，出现了大量的负样本。

计算9个anchor和4个GT的IOU，选择最大的IOU，是x23，也就是将anchor2用于预测GT3。将x23所在的行和列删除。在剩下的IOU中选取最大的，x71，依次类推，为所有的GT找到一个合适的anchor。

<img src="https://pic1.zhimg.com/80/v2-900ba9a37becf7920173e8ec04dec858_1440w.jpg" alt="image.png" style="zoom:75%;" />



### 目标检测常用算法

#### **区域卷积神经网络(RNN)**

![](https://i0.hdslb.com/bfs/note/d577f3c035597fa3f3cad94b947df200cd6b5b66.png)

- 首先从输入图像中选取若干个**提议区域**（**锚框**是选取方式的一种），并标注它们的**类别**和**边界框（如偏移量）**。然后用卷积神经网络来对每个提议区域（锚框）进行前向传播以抽取特征。最后用每个提议区域的特征来预测类别和边界框。

R-CNN 模型的四个步骤:

1. 对输入图像使用**选择性搜索**来选取多个高质量的提议区域。这些提议区域通常是在**多个尺度**下选取的，并具有**不同的形状和大小**；每个提议区域都将被标注**类别和真实边框**
2. 选择一个预训练的卷积神经网络，并将其在输出层之前截断。将每个提议区域变形为网络需要的输入尺寸，并通过前向传播输出抽取的**提议区域特征**
3. 将每个提议区域的特征连同其标注的类别作为一个样本。训练多个**支持向量机**对目标分类，其中每个支持向量机用来判断样本是否属于某一个**类别**
4. 将每个提议区域的特征连同其标注的边界框作为一个样本，训练**线性回归模型**来预测**真实边界框**

==每次选择的锚框的大小是不同的，在这种情况下，怎样使这些大小不一的锚框变成一个batch？==

**RoI pooling（兴趣区域池化层）**

- R-CNN 中比较关键的层，作用是**将大小不一的锚框变成统一的形状**
- 给定一个锚框，先将其均匀地分割成 n * m 块，然后输出每块里的最大值，这样的话，**不管锚框有多大，只要给定了 n 和 m 的值，总是输出 nm 个值**，这样的话，不同大小的锚框就都可以变成同样的大小，然后作为一个小批量，之后的处理就比较方便了

![](https://i0.hdslb.com/bfs/note/9d7d2cb8c875c42a1b87834fa38502f70d018a26.png)

- 上图中对 3 * 3 的黑色方框中的区域进行 2 * 2 的兴趣区域池化，由于 3 * 3 的区域不能均匀地进行切割成 4 块，所以会进行取整（最终将其分割成为 2 * 2、1 * 2、2 * 1、1 * 1 四块），在做池化操作的时候分别对四块中每一块取最大值，然后分别填入 2 * 2 的矩阵中相应的位置

==兴趣区域汇聚层（RoI Pooing）与一般的汇聚层有什么不同？==

- 在一般的汇聚层中，通过设置**汇聚窗口、填充和步幅的大小**来**间接**控制输出形状
- 在兴趣区域汇聚层中，对每个区域的输出形状是可以**直接**指定的

#### **Fast R-CNN**

- R-CNN 每次拿到一张图片都需要抽取特征，如果说一张图片中生成的锚框数量较大，抽取特征的次数也会相应的增加，大大增加了计算量

- 因此，R-CNN 的主要性能瓶颈在于，对于每个提议区域，卷积神经网络的前向传播是**独立**的，没有共享计算（==这些提议区域通常有重叠，独立的特征提取会导致重复计算==）

  ![](https://i0.hdslb.com/bfs/note/66a6d7b1dc3414493a3e7114c12d31bc483a7539.png)

- Faste R-CNN 的改进是:在拿到一张图片之后，首先使用 CNN 对图片进行特征提取（不是对图片中的锚框进行特征提取，而是**对整张图片进行特征提取**，仅在整张图像上执行卷积神经网络的前向传播），最终会得到一个 7 * 7 或者 14 * 14 的 feature map
- 抽取完特征之后，再对图片进行锚框的选择（selective search），搜索到原始图片上的锚框之后将其（按照一定的比例）映射到 CNN 的输出上
- 映射完锚框之后，再使用 **RoI pooling** 对 CNN 输出的 feature map 上的锚框进行特征抽取，生成固定长度的特征（将 n * m 的矩阵拉伸成为 nm 维的向量），之后再通过一个全连接层（==这样就不需要使用SVM一个一个的操作，而是一次性操作了==）对每个锚框进行预测:**物体的类别和真实的边缘框的偏移**
- 上图中黄色方框的作用就是将原图中生成的锚框变成对应的向量
- ==Fast R-CNN 相对于 R-CNN 更快的原因是==:Fast R-CNN 中的 CNN 不再对每个锚框抽取特征，而是对整个图片进行特征的提取（这样做的好处是:不同的锚框之间可能会有**重叠**的部分，如果对每个锚框都进行特征提取的话，**可能会对重叠的区域进行多次重复的特征提取操作**），然后再在整张图片的feature中找出原图中锚框对应的特征，最后一起做预测

- 为了精确地检测目标结果，Fast R-CNN 模型通常需要在选择性搜索中生成大量的提议区域
- 因此，Faster R-CNN 提出将选择性搜索替换为**区域提议网络**（region proposal network，RPN），模型的其余部分保持不变，从而**减少区域的生成数量，并保证目标检测的精度**

![](https://i0.hdslb.com/bfs/note/3af980584a194a504b5c4722a5a0efd6bdfe59ca.png)

- Faster R-CNN 的改进:使用 **RPN 神经网络**来替代 selective search 
- RoI 的输入是**CNN 输出的 feature map** 和**生成的锚框**
- RPN 的输入是 CNN 输出的 feature map，输出是一些比较高质量的锚框（可以理解为一个**比较小而且比较粗糙的目标检测算法**: CNN 的输出进入到 RPN 之后再做一次卷积，然后生成一些锚框（可以是 selective search 或者其他方法来生成初始的锚框），再训练一个**二分类问题**:预测锚框**是否框住了真实的物体**以及**锚框到真实的边缘框的偏移**，最后使用 **NMS** 进行去重，使得锚框的数量变少）
- RPN 的作用是生成大量结果很差的锚框，然后进行预测，最终输出比较好的锚框供后面的网络使用（预测出来的比较好的锚框会进入 RoI pooling，后面的操作与 Fast R-CNN 类似）
- 通常被称为**两阶段的目标检测算法**:RPN 做小的目标检测（粗糙），整个网络再做一次大的目标检测（精准）
- Faster R-CNN 目前来说是用的比较多的算法，**准确率比较高，但是速度比较慢**

#### Mask R-CNN

- 如果在训练集中还标注了每个目标在图像上的**像素级位置**，Mask R-CNN 能够有效地利用这些相近地标注信息进一步提升目标检测地精度

  ![](https://i0.hdslb.com/bfs/note/a80b045c48d1eda6f340ba23870cc60447629176.png)

Mask R-CNN 是基于 Faster R-CNN 修改而来的，改进在于

- 假设有每个像素的标号的话，就可以对每个像素做预测（**FCN**）
- 将兴趣区域汇聚层替换成了**兴趣区域对齐层**（RoI pooling -> RoI align），使用**双线性插值**（bilinear interpolation）保留特征图上的空间信息，进而更适于像素级预测:对于pooling来说，假如有一个3 * 3的区域，需要对它进行2 * 2的RoI pooling操作，那么会进行取整从而切割成为不均匀的四个部分，然后进行 pooling 操作，这样切割成为不均匀的四部分的做法对于目标检测来说没有太大的问题，因为目标检测不是像素级别的，偏移几个像素对结果没有太大的影响。但是**对于像素级别的标号来说，会产生极大的误差**；RoI align 不管能不能整除，如果不能整除的话，会直接将像素切开，切开后的每一部分是原像素的加权（它的值是原像素的一部分）
- 兴趣区域对齐层的输出包含了所有与兴趣区域的形状相同的特征图，它们不仅被用于**预测每个兴趣区域的类别和边界框**，还通过额外的全卷积网络**预测目标的像素级位置**

**模型精度的比较**

![](https://i0.hdslb.com/bfs/note/031cda3ea571cf31b57d1e4155287439e3cec2b5.png)

**总结**

- R-CNN 是最早、也是最有名的一类基于锚框和 CNN 的目标检测算法（R-CNN 可以认为是使用神经网络来做目标检测工作的奠基工作之一），它对图像选取若干提议区域，使用卷积神经网络对每个提议区域执行前向传播以抽取其特征，然后再用这些特征来预测提议区域的类别和边框
- Fast/Faster R-CNN持续提升性能:Fast R-CNN **只对整个图像做卷积神经网络的前向传播**，还引入了**兴趣区域汇聚层**（RoI pooling），从而为具有不同形状的兴趣区域抽取相同形状的特征；Faster R-CNN 将 Fast R-CNN 中使用的选择性搜索替换为**参与训练的区域提议网络**，这样可以在减少提议区域数量的情况下仍然保持目标检测的精度；Mask R-CNN 在 Faster R-CNN 的基础上引入了一个**全卷积网络**，从而借助目标的**像素级位置**进一步提升目标检测的精度
- Faster R-CNN 和 Mask R-CNN 是在追求高精度场景下的常用算法（Mask R-CNN 需要有像素级别的标号，所以相对来讲局限性会大一点，在无人车领域使用的比较多）

#### **单发多框检测（SSD）**

- 对每个像素生成多个以它为中心的多个锚框

  ![](https://i0.hdslb.com/bfs/note/393fb24dc90f86d8a2ea5c254ab623e7acbbf8b0.png)

![](https://i0.hdslb.com/bfs/note/924c747e8134ee2b55e5752dfba9724a2573aede.png)

![](https://i0.hdslb.com/bfs/note/f5c0c5a8e98db98da7ed57bc85e242197090d469.png)

- 输入图像之后，首先进入一个基础网络来抽取特征，抽取完特征之后对每个像素生成大量的锚框（==每个锚框就是一个样本，然后预测锚框的类别以及到真实边界框的偏移==）
- SSD 在给定锚框之后**直接对锚框进行预测**，而不需要做两阶段（==为什么 Faster RCNN 需要做两次，而 SSD 只需要做一次？==SSD 通过做不同分辨率下的预测来提升最终的效果，越到底层的 feature map，就越大，越往上，feature map 越少，因此**底层更加有利于小物体的检测，而上层更有利于大物体的检测**）
- SSD 不再使用 RPN 网络，而是**直接在生成的大量样本（锚框）上做预测**，看是否包含目标物体；如果包含目标物体，再预测该样本到真实边缘框的偏移

#### YOLO

- yolo 也是一个 single-stage 的算法，只有一个**单神经网络**来做预测
- yolo 也需要锚框，这点和 SSD 相同，但是 SSD 是对每个像素点生成多个锚框，所以在绝大部分情况下两个相邻像素的所生成的锚框的**重叠率**是相当高的，这样就会导致很大的重复计算量。
- yolo 的想法是**尽量让锚框不重叠**:首先将图片均匀地分成 S * S 块，每一块就是一个锚框，每一个锚框预测 B 个边缘框（考虑到一个锚框中可能包含多个物体），所以最终就会产生 S ^ 2 * B 个样本，因此速度会远远快于 SSD
- yolo 在后续的版本（V2,V3,V4...）中有持续的改进，但是核心思想没有变，真实的边缘框不会随机的出现，真实的边缘框的比例、大小在每个数据集上的出现是有一定的规律的，在知道有一定的规律的时候就可以使用**聚类算法**将这个规律找出来（给定一个数据集，**先分析数据集中的统计信息，然后找出边缘框出现的规律**，这样之后在生成锚框的时候就会有先验知识，从而进一步做出优化）

#### **center net**

- 基于**非锚框**的目标检测
- center net 的优点在于简单
- center net 会对**每个像素**做预测，用 FCN 对每个像素做预测（类似于图像分割中用 FCN 对每个像素标号），**预测该像素点是不是真实边缘框的中心点**（==将目标检测的边缘框换算成基于每个像素的标号，然后对每个像素做预测，就免去了一些锚框相关的操作==）

### 图像语义分割

- 在**图片分类**中，其主要任务是给定一张图片，识别图片中主体物体
- **语义分割**可以识别并理解图像中每一个像素的内容（将图片中的每个像素分类到对应的类别），其语义区域的标注和预测是**像素级**的

**图像分割（ image segmentation ）与实例分割（ instance segmentation ）**

**图像分割**

- 分割在计算机视觉中应用的时间比较长，最早是进行图片分割，给定一张图片，通过**聚类**或者其他方法，将语义上比较像的像素放在一起，可能不会明确某一块像素到底是什么，而只是像素在颜色或者像素上比较相似，然后进行聚类
- 图像分割将图像划分为若干组成区域，这类问题的方法通常利用**图像中像素之间的相关性**。它在训练时不需要有关图像像素的标签信息，在预测时也无法保证分割出的区域具有所希望得到的语义

**实例分割**

- 实例分割也叫同时检测并分割（ simultaneous detection and segmentation ），它研究如何识别图像中各个目标实例的像素级区域
- 实例分割与语义分割的不同之处在于:==实例分割不仅需要区分语义，还要区分不同的目标实例==

**语义分割**

- 语义分割和一般分割的不同之处在于它就明确每一个像素的标号（ label ）到底是什么，它属于是有监督的学习，而一般的分割可以通过聚类来实现无监督的学习
- 相比于图片分类和目标检测，语义分割更加精细，因为需要对每一个像素的类别进行判断，对每一个像素生成一个标号

![](https://i0.hdslb.com/bfs/note/4ee9ea1d34391ef45e342049522ca18e7aba7c45.png)

**总结**

- 语义分割通过将图像划分为属于不同语义类别的区域，来识别并理解图像中像素级别的内容
- 由于语义分割的输入图像和标签在像素上一一对应，输入图像会被**随机裁剪**为固定尺寸而不是缩放



## 转置卷积、FCN和样式迁移

### 转置卷积

转置卷积出现的理由:

- **卷积层**和**汇聚层**通常会减少下采样输入图像的空间维度（高和宽）
- 卷积通常来说不会增大输入的高和宽，**要么保持高和宽不变**，**要么会将高宽减半**，很少会有卷积将高宽变大的
- 语义分割的问题在于需要对输入进行**像素级别的输出**，但是卷积通过不断地减小高宽，不利于像素级别的输出，所以需要另外一种卷积能够将高宽变大

**工作原理**

- 转置卷积的工作原理有点类似于卷积

![](https://i0.hdslb.com/bfs/note/b64528af1ee7a4512e54281dd47b3d0bafc9b5fb.png)

- 假设有一个 2×2 的输入和一个 2×2 的核
- 这个核会在输入上以**步幅为 1** 进行滑动且没有填充，对于输入的每一个元素，它会跟核上的每一个元素按元素做乘法，然后逐次写回到对应的位置（写回到一个更大的矩阵中，除了写回的位置，其他元素初始化为 0 ）
- 这样的话，输入有多少个元素，就会得到多少个乘积的结果（比输入更大的矩阵），最后将这些结果按元素位置进行相加，最终就能够得到输出了
- 上图中的 stride 为 1 （输入的相邻元素跟核按元素乘积的结果写回到更大的矩阵的对应位置时相隔 1 列），**stride 如果特别大的话就能够将输出的高宽变得特别大，达到成倍地增加高宽的目的**

**转置卷积和卷积的异同**

**1、工作原理**

- 常规卷积是输入跟核进行按元素的乘法**并相加**，最后得到对应位置的值，通过核在输入上进行滑动从而得到输出
- 转置卷积是输入的单个元素跟核按元素做乘法**但不相加**，保持核的大小，然后按元素写回到一个更大的矩阵的对应位置（输入的每个元素会生成与核大小相同的矩阵写入到一个更大的矩阵的对应位置，所以输出的高宽相对于输入来讲是变大的）

**2、输入输出**

- 常规卷积通过卷积核 **“减少”** 输入元素
- 转置卷积通过卷积核 **“广播”** 输入元素，从而产生大于输入的输出

**3、填充**

- 常规卷积中将填充应用于输入（如果将高和宽两侧的填充数指定为 1 时，常规卷积的输入中将**增加**第一和最后的行和列）
- 转置卷积中将填充应用于输出（如果将高和宽两侧的填充数指定为 1 时，转置卷积的输出中将**删除**第一和最后的行和列）

**4、步幅**

- 常规卷积中，步幅所指定的是卷积核在**输入**上的滑动距离
- 转置卷积中，步幅所指定的是卷积核每次运算结果写回到**中间结果（输出）**矩阵中对应位置的滑动距离，以下两张图是**卷积核为 2 × 2 ，步幅分别为 1 和 2 的转置卷积运算**

![](https://i0.hdslb.com/bfs/note/375a5076e283cf6eb7fd3fd8e4bd017ae0bdcbc4.png)

- 上图运算中转置卷积的**步幅为 1**

![](https://i0.hdslb.com/bfs/note/57b7f3b876f74848f9ea2c69f47f927ac88a818b.png)

- 上图运算中转置卷积的**步幅为 2**

![image-20230218164048500](https://img1.imgtp.com/2023/02/18/1aIhTWug.png)

示例代码

```python
def trans_conv(X,k):
  h,w=k.shape
  Y=torch.zeros((X.shape[0]+h-1,X.shape[1]+w-1))
  for i in range(X.shape[0]):
    for j in range(X.shape[1]):
      Y[i:i+h,j:j+w]+=X[i,j]*k
  return Y
X = torch.tensor([[0.0, 1.0], [2.0, 3.0]])
K = torch.tensor([[0.0, 1.0], [2.0, 3.0]])
trans_conv(X, K)
#tensor([[ 0.,  0.,  1.],[ 0.,  4.,  6.],[ 4., 12.,  9.]])
```

重新排列输入和核

* 当填充为0步幅为1时:
  * 将输入填充k-1(k是核窗口)
  * 将核矩阵上下、左右翻转
  * 然后做正常卷积（填充0、步幅1）

![image-20230221104139141](https://img1.imgtp.com/2023/02/21/AB7iReR5.png)

* 当填充为p步幅为1时:

  * 将输入填充k-p-1(k是核窗口)

  * 将核矩阵上下、左右翻转

  * 然后做正常卷积（填充0、步幅1）

![image-20230221104216293](https://img1.imgtp.com/2023/02/21/F6oNa5wn.png)

当填充为p步幅为s时:

* 在行和列之间插入s-1行或列

* 将输入填充k-p-1(k是核窗口)

* 将核矩阵上下、左右翻转

* 然后做正常卷积（填充0、步幅1)

![image-20230221104428995](https://img1.imgtp.com/2023/02/21/5Kr6da3e.png)

![image-20230221104516565](https://img1.imgtp.com/2023/02/21/n1mb5AuC.png)

### 全连接卷积神经网络（FCN）

* **Fully Convolutional Network**，FCN
* 语义分割是对图像中的每个像素进行分类，输出的类别预测与输入图像在**像素级别**上具有一一对应关系:通道维的输出即为该位置对应像素的类别预测
* FCN 采用卷积神经网络实现了**从图像像素到像素类别的变换**，区别于图像分类和目标检测中的卷积神经网络，==全连接卷积神经网络通过引入转置卷积将中间层特征图的高和宽变换回输入图像的尺寸。==

**工作原理**

它用转置卷积层来替换 CNN 最后的全连接层，从而可以实现每个像素的预测。

![](https://i0.hdslb.com/bfs/note/103e3a219d273f997dfe5b81dd109374b4232747.png)

1、CNN 可以认为是在 ImageNet 上面预训练好的模型

* 全连接卷积神经网络先使用卷积神经网络抽取图像特征
* CNN 模型的最后两层要么就是全连接层，这样可以做到 label 的语义信息，全连接层下面通常是一个全局平均池化层:全连接层将 224`*`224 的图片变成 7`*`7 的高宽，全局平均池化层再将 7`*`7 变成 1`*`1 ，不管怎么样将通道中的信息做平均
* 这对于图片分类来说没有什么问题，但是对于需要空间信息来说就不是那么好了，所以全连接卷积神经网络的 CNN 其实就是去掉了全连接层和最后的全局平均池化层，所以如果输入是 224`*`224 的图片的话，输出就是 7`*`7 的高宽，通道数可能是 512`*`512 


2、1`*`1 的卷积层

* 通过 1`*`1 卷积层将通道数变换为类别个数
* 不会对空间信息做变化，主要是用来==降低维度（降低通道数），从而降低计算量==


3、transposed conv（转置卷积层）

* 转置卷积层就是将图片放大，将特征图的高和宽变换为输入图像的尺寸，从而使模型输出与输入图像的高和宽相同，并且最终输出通道包含了该空间位置像素的类别预测
* 假设 CNN 是将图片缩小的话，一般来说，对于 ImageNet 的 224`*`224 的图片来说是缩小 32 倍==（高宽均缩小 32 倍）==，得到 7`*`7 的高宽
* 转置卷积层就是将图片扩大 32 倍，将 7`*`7 的高宽还原称为 224`*`224 ，通道数 K 等价于类别数==（对每个像素的类别预测存储在通道信息中）==，这样的话，不管对于高宽为多少的图片，都会得到通道数为类别数且高宽相同==（与输入图片的原始尺寸相同）==的预测，这样就能实现对每个像素做标号和预测
* 在图像处理中，有时需要将图片放大[（**上采样，upsampling**），双线性插值（**bilinear interpolation**）](https://blog.csdn.net/zhanly19/article/details/99718242)是常用的上采样方法//之一，也常用于初始化转置卷积层==（双线性插值的上采样可以通过转置卷积层实现）==

```python
# 双线性插值内核
def bilinear_kernel(in_channels, out_channels, kernel_size):
    factor = (kernel_size + 1) // 2
    if kernel_size % 2 == 1:
        center = factor - 1
    else:
        center = factor - 0.5
    og = (torch.arange(kernel_size).reshape(-1, 1),
          torch.arange(kernel_size).reshape(1, -1))
    filt = (1 - torch.abs(og[0] - center) / factor) * \
           (1 - torch.abs(og[1] - center) / factor)
    weight = torch.zeros((in_channels, out_channels,
                          kernel_size, kernel_size))
    weight[range(in_channels), range(out_channels), :, :] = filt
    return weight
conv_trans = nn.ConvTranspose2d(3, 3, kernel_size=4, padding=1, stride=2,
                                bias=False)
conv_trans.weight.data.copy_(bilinear_kernel(3, 3, 4)) # 将卷积核权重设置为一个大小为3x3x4x4的双线性插值核
# 这里使用了copy_()方法，将bilinear_kernel函数返回的权重值复制到卷积层的权重张量中。这种操作可以确保新创建的层中的权重张量与指定的张量共享内存，以便有效地利用GPU的计算资源。
W = bilinear_kernel(num_classes, num_classes, 64) # 初始化神经网络中的一个转置卷积层的权重。
net.transpose_conv.weight.data.copy_(W) # 作用是将W的值复制到转置卷积层的权重参数中，以初始化该层的权重。
```

### 样式迁移

- 计算机视觉的应用之一，将样式图片中的样式（比如油画风格等）迁移到内容图片（比如实拍的图片）上，得到合成图片
- 可以理解成为一个滤镜，但相对于滤镜来讲具有更大的灵活性，一个滤镜通常只能够改变图片的某个方面，如果要达到理想中的风格，可能需要尝试**大量不同的组合**，这个过程的复杂程度不亚于模型调参

- **奠基性工作**
- 使用神经网络修改内容图片，使其在样式上接近风格图片

![](https://i0.hdslb.com/bfs/note/43809e2bd6725bf0dd285ab43de4bb3f6bbbb64c.png)

**工作原理**

![](https://i0.hdslb.com/bfs/note/7fe424724d2f709567e2b9964bc7d3244882e2d9.png)

1、首先**初始化合成图片**（例如将其初始化为内容图片）

- 输入中有一张**内容图片（Content Image）和一张**样式图片（Style Image）
- ==模型所要训练的不是卷积神经网络的权重，而是合成图片，它是样式迁移过程中唯一需要更新的变量，即样式迁移所需迭代的参数模型==

2、然后选择一个**预训练的卷积神经网络**来抽取图片的特征（该卷积神经网络的模型参数在训练中不用更新）

- 内容图片、样式图片之后和**合成图片（Synthesised Image）**之前各有一个卷积神经网络，上图中只画了三层，看起来有三个三层的卷积神经网络，==实际上三个卷积神经网络都是一样的（它们的权重是一样的）==

3、这个深度神经网络凭借多个层逐级抽取图像的特征，因此可以选择其中某些层的输出作为**内容特征**或者**样式特征**（上图中的卷积神经网络第二层输出内容特征，第一层和第三层输出样式特征）

- 对于一张输入图片来讲，每一层的卷积神经网络都会有一个输出（特征），整个基于 CNN 的样式迁移的目的是训练出一张合成图片，使得合成图片和内容图片放进同样一个卷积神经网络的时候，合成图片在某一层的**输出**能够匹配上内容图片在某一层的损失**（内容损失，Content Loss）**，即它们在内容上是相近的；同理，合成图片和样式图片所使用的是同一个卷积神经网络，在某些层的输出（特征）在样式上能够匹配的上。如果训练出一张合成图片同时满足以上需求的话，就可以认为它既保留了内容图片的内容，又保留了样式图片的样式
- 一般来说，越靠近输入层，越容易抽取图片的**细节信息**；反之，越容易抽取图片的**全局信息**
- 为了避免合成图片过多地保留内容图片的**细节**，==选择靠近输出的层（即内容层）来输出图片的内容特征==
- 选择不同层的输出（即风格层）来匹配局部和全局的样式
- 在使用卷积神经网络抽取特征时，只需要用到**从输入层到最靠近输出层的内容层或者样式层之间的所有层**
- 因为在训练的时候无需改变预训练的卷积神经网络的模型参数，所以可以在训练开始之前就提取出内容特征和风格特征

4、通过**前向传播（实线箭头方向）**计算样式迁移的损失函数，并通过**反向传播（虚线箭头方向）**迭代模型参数，即不断更新合成图片

- 样式迁移常用的损失函数由三部分组成:（1）**内容损失**通过**平方误差函数**衡量合成图片与内容图片**在内容特征上的差异**，使合成图片与内容图片在内容特征上接近；（2）**样式损失**也是通过**平方误差函数**衡量合成图片与样式图片**在样式特征上的差异**，使合成图片与样式图片在样式特征上接近；（3）**全变分损失**有助于减少合成图片中的噪点，有时学到的合成图像中有大量高频噪点（==即有特别亮或者特别暗的颗粒像素==），常用**全变分去噪（Total Variation Denoising）**，通过降低全变分损失，能够尽可能使临近的像素值相似，来进行去噪
- 样式迁移的损失函数是**内容损失**、**样式损失**和**总变化损失**的加权和，通过调节这些**权重超参数**，可以权衡合成图片在保留内容、样式迁移以及去噪三方面的相对重要性
- 对于给定的输入，如果简单地调用前向传播函数，只能获得最后一层的输出，因为还需要中间层的输出，所以需要进行**逐层计算**，保留**内容层**和**风格层**的输出
- ==在样式迁移中，合成图片是训练期间唯一需要更新的变量==，因此可以将合成图片视为模型参数，模型的前向传播只需要返回模型参数即可

5、最后当模型训练结束时，输出样式迁移的模型参数即为最终的合成图片

- 因为合成图片是样式迁移所需迭代的模型参数，所以只能在训练的过程中抽取合成图片的内容特征和样式特征
- ==合成图片保留了内容图片的内容，并同时迁移了样式图片的样式==

1、样式迁移常用的损失函数由 3 部分组成**:内容损失、样式损失和全变分损失**

- **内容损失**使合成图片与内容图片在**内容特征**上接近
- **样式损失**使合成图片与样式图片在**样式特征**上接近
- **全变分损失**有助于减少合成图片中的**噪点**

2、可以通过**预训练好的卷积神经网络**来抽取图像的特征，并通过**最小化损失函数**来不断更新**合成图片**来作为模型参数

3、使用**格拉姆矩阵**表达样式层输出的样式

## 序列模型和文本预处理

**序列数据**

- 现实生活中很多数据都是有时序结构的，比如电影的评分（既不是固定的也不是随机的，会随着时间的变化而变化）
- 在统计学中，对超出已知观测范围进行预测称为**外推法（extrapolation）**，在现有的观测值之间进行估计称为**内插法（interpolation）**

**统计工具**

- 处理序列数据选用**统计工具**和**新的深度神经网络架构**

![](https://i0.hdslb.com/bfs/note/04b2a15e9f0273a8e8bd2bb356027c8306ebc6c0.png)

- **不独立的随机变量**:变量之间存在某种关联（在此之前都是假设独立的随机变量，变量之间是没有关联的）

![](https://img1.imgtp.com/2023/02/21/LMwjOIWA.png)

- 黄色表示从 x1 一直到 xT 的方向（xi 的概率依赖于 x1,...,x(i-1) 的概率,i >= 2 ）,也就是说如果想要知道一个时序序列 T 时刻发生的事情，则需要知道在 T 时刻之前所有时刻发生的事情
- 红色表示先计算 xT 再依次算到 x1（也就是黄色的反方向，xi 的概率依赖于 x(i+1),...,xT 的概率,i <= T ），即**反序**，也就是已知未来 T 时刻发生的事情，反推出过去所有时刻所发生的事情（这种情况在==某些时候是成立的，但是在物理上不一定可行，对于真实的事件，无法从未来的事件反推过去的事件，因为未来所发生的事件是根据过去所发生的事件所产生的==）

**自回归模型**

- 为了实现对 t 时刻的 x 值的预测，可以使用回归模型

问题:输入数据的数量，输入 x(t-1) , ... , x1 本身因 t 而异，输入数据的数量会随着数据量的增加而增加

针对上述问题提出了两种策略:

**1、自回归模型（autoregressive models）**

- 假设在现实情况下相当长的序列 x(t-1) , ... , x1 可能是不必要的，因此只需要考虑满足**某个长度为 τ 的时间跨度**即可，即使用观测序列 x(t-1) , ... , x(t-τ) 
- 这样做的好处是保证了在 t > τ 时，参数的数量是**固定**的
- 之所以叫做自回归模型是因为该模型是对自己执行回归

**2、隐变量自回归模型（latent autoregressive models）**

![](https://i0.hdslb.com/bfs/note/556cb8e1295cdfcb91f117eec024893da1f5591b.png)

- 隐变量自回归模型中**保留了对过去观测的总结 ht** ，并且**同时更新预测 xt_hat 和总结 ht**
- xt_hat = P(xt_hat|ht )
- ht = g(h(t-1) , x(t-1) )
- 因为 ht 从未被观测到，所以这类模型也被称为隐变量自回归模型

以上两种情况都存在一个问题:如何生成训练数据？

经典方法:**使用历史观测预测下一个未来观测**

- 但是时间并不是停滞不前的，总会有未来的观测所对应的时间变成历史时刻

常用假设:虽然特定值 xt 可能会改变，但是序列本身的动力学不会改变

- 新的动力学一定受新的数据影响，不可能用目前所掌握的数据来预测新的动力学（统计学家成不变的动力学为静止的（ stationary ））
- 整个序列的估计值计算如下

![](https://i0.hdslb.com/bfs/note/6c531e7b35024bfb06024903b7d4be7f8eee8d96.png)

### 方案 A -马尔科夫假设

![](https://i0.hdslb.com/bfs/note/cadf6e9f27b8f7e34134b16228a8b3bb52d11d8b.png)

- 假设当前数据只跟 *τ 个过去数据点相关**（==每一次预测一个新的数据，只需要看过去 τ 个数据就可以了，τ 可以自由选择，τ 越小模型越简单，τ 越大模型越复杂）==**，这样假设的好处是 τ 的值是固定的，不会随着时间的增大而增大**（==这样做也比较符合现实的逻辑，过去事件距离预测事件的时间越长，他们之间的关联程度就越小==）*
- *可以在过去数据上训练一个 MLP 模型就可以了:假设 x 是一个标量，f 的作用就是每次给定一个 τ 层的特征（这个特征是一个向量），然后再去预测一个标量*
- *==给定 τ ，使得数据由变长数据变成了定长数据==*

### 方案 B -潜变量模型

![](https://i0.hdslb.com/bfs/note/60a272894e2c4eb1d55139c97e81dbaabe6c6afc.png)

- 引入一个潜变量 ht 表示过去信息 ht = f( x1 , x2 , ... , x(t-1) )，这样的话， xt = p( xt | ht )
- 一旦引入了潜变量 h ,h 是可以不断更新的，h 与前一个时刻的潜变量和前一个时刻的 x 相关，等价于建立了两个模型:一个模型是根据前一个时刻的潜变量 h 和 x，计算新的潜变量 h' ；第二个模型是根据新的潜变量 h' 和前一个时刻的 x ，计算新的 x。==这样就拆分成了两个新的模型，每个模型只和 1 个 或者 2 个模型相关，使得计算更加容易==

如果将一个序列转换为模型的“特征-标签”（feature-label）对，如果将时间跨度固定为 τ ，将数据映射为数据对 yt = xt 和 xt = `[ x(t-τ) , ... , x(t-1) ]`，这时候会发现原有的数据样本少了 τ 个（==因为按照这种映射关系，序列中的前 τ 个样本并没有足够的历史数据作为 xt 中的元素==），一般所采用的办法有两种:

1、在序列足够长的情况下，可以直接将前面的 τ 项**丢弃**（这里的丢弃指的是不再作为 yt ，也就是说 t > τ；这些项还是可以作为 xt 中的元素）

2、**用零填充序列**

**总结**

- 时序模型中，当前数据跟之前观察到的数据相关
- 自回归模型**使用自身过去数据来预测未来**
- 马尔科夫模型假设当前只跟当前少数数据相关，每次都使用**固定长度的过去信息**来预测现在，从而简化模型
- 潜变量模型使用潜变量来概括历史信息，使得模型拆分成两块:一块是**根据现在观测到的数据来更新潜变量**；另一块是**根据更新后的潜变量和过去的数据来更新将来要观测到的数据**
- 内插法（==在现有观测值之间进行估计==）和外推法（==对超出已知观测范围进行预测==）在实践的难度上差别很大。因此对于已有的序列数据，在训练时始终要尊重其时间顺序，最好不要基于未来的数据进行训练
- 序列模型的估计需要专门的统计工具，两种比较流行的是**自回归模型**和**隐变量自回归模型**
- 对于时间是向前推进的因果模型，正向估计通常比反向估计更容易
- 对于直到时间步 t 的观测，其在时间步 t+k 的预测输出是“ k 步预测”。随着时间 k 值的增加，会造成误差的快速累积和预测质量的急速下降

### 文本预处理

- 序列数据往往存在多种形式，**文本**是其中常见的形式之一，例如一篇文章可以被简单地看作是一串**单词序列**，甚至是一串**字符序列**
- 将文本当做时序序列，将文本中的字或者字符、词当成样本，样本之间是存在时序信息的，因此文本是一个很长的时序序列
- 文本预处理的核心思想是==如何将文本中的词转化成能够训练的样本==

**常见的文本预处理步骤**

**1、读取数据集:将文本作为字符串加载到内存中**

- 将数据集读取到**由多条文本行组成的列表**中，其中每一条文本行都是一个字符串
- 将非大小写字符全部变成空格（这虽然是一种有损的操作，但是能够使后续的操作变得更加简单）
- 去掉回车
- 将所有字母全部变成小写

**2、词元化:将字符串拆分为词元（如单词和字符）**

- **tokenize** 是 NLP 中一个比较常见的操作:将一个句子或者是一段文字转化成 token（字符串、字符或者是词）
- 将文本行列表（ lines ）作为输入，列表中的每个元素都是一个文本序列（比如一条文本行）
- 将每个文本序列拆分成一个词元列表，**词元**（ token，英文中 token 一般有两种表示单元:一种是一个**词**作为一个基本单元，词相对来说，会让机器学习的模型更简单一点；一种是一个**字符串**作为一个基本单元，好处是样本数量比较少，坏处是还需要学习字符串的构成，字符串是如何由词构成的）是文本的基本单位
- 中文的话会有所不同，因为在中文的段落中，词与词之间的间隔不是使用空格来进行间隔的，所以在中文中如果想使用词来表示 token 的话，还需要对其进行分词，分词相对来讲不是很容易
- 通过拆分，文本序列就被拆分成了许多 **token 列表**，这些列表要么是空，要么是有许多 token 在其中
- 最后返回一个由词元列表组成的列表，其中每个词元都是一个字符串（ string ）

**3、建立词表，将拆分的词元映射到数字索引:将文本转换为数字索引序列，方便模型操作**

- 词元的类型是字符串，而模型需要的输入是数字（模型训练使用的都是 tensor ，而 tensor 都是基于下标的），因此这种类型不方便模型使用，所以需要构建一个字典，通常也叫做**词汇表（vocabulary）**，用来将字符串类型的 token （要么是 word ，要么是 char ）映射到从 0 开始的数字索引中
- 首先将训练集中所有的文档合并到一起，然后对它们的**唯一词元**进行统计，得到最终的统计结果 -- **语料（ corpus ）**
- 然后根据每个唯一词元的**出现频率**，为其分配一个**数字索引**，对于出现次数较少的词元，通常会被移除，以降低复杂性（**min_freq**:在 NLP 中，有很多词是不出现的，如果使用词的话，这些词可能在文本中就出现了几次，在这种情况下如果要进行训练的话可能比较困难，这里的 min_freq 指的是一个 token 在文本序列中出现的最少次数，如果少于这个数字的话，会将这些出现频率较低的 token 全部视为 “unknown”）
- 语料库中**不存在或者是已删除的任何词元**都将映射到一个特定的未知词元 **“`<unk>`”** 
- 还可以选择增加一个列表，用于保存保留下来的词元，比如**填充词元（ “`<pad>`” ）**；**序列开始词元（ “`<bos>`” ）**；**序列结束词元（ “`<eos>`” ）**

### 语言模型

- 语言模型是 NLP 中最经典的模型，假设给定长度为 T 的文本序列 x1,x2, ... ,xT（可能是词序列，也可能是字符序列），xt （1 <= t <= T）可以被认为是文本序列在时间步 t 处的观测或者标签。**语言模型的目标是估计序列的联合概率 p(x1,x2, ... xT)**
- 序列模型的核心就是整个序列文本所出现的概率

- 目前所面对的问题是如何对一个**文档**，甚至是一个**词元序列**进行建模
- 为了训练语言模型，需要计算**单词的概率**以及给定前面几个单词后出现**某个单词的条件概率**（==这些概率本质上就是语言模型的参数==）

假设训练数据集是一个大型的文本语料库，训练集中词的概率可以根据**给定词的相对词频**来计算:一种稍微不太精确的方法是统计单词在数据集中的出现次数，然后将其除以整个语料库中的单词总数

- 由于**连续单词对**的出现频率出现频率低得多，特别是对于一些不常见的单词组合，要想找到足够的出现次数来获得准确的估计可能都不容易
- 对于三个或者更多的单词组合，情况可能会变得更糟，许多合理的三个单词所组成的单词组合可能是存在的，但是在数据集中可能找不到
- 对于上述情况，如果数据集很小，或者单词非常罕见，那么这类单词出现一次的机会可能都找不到，除非能够提供某种解决方案将这些单词组合指定为非零计数，否则将无法在语言模型中使用它们
- 一种常见的策略是执行某种形式的**拉普拉斯平滑（Ｌａｐｌａｃｅ　ｓｍｏｏｔｈｉｎｇ）**，在所有的计数中添加一个小常量。但是这样的模型很容易变得无效，==因为需要存储所有的计数，而且完全忽略了单词的意思，最后，长单词序列大部分是没出现过的，因此一个模型如果只是简单地统计先前看到的单词序列频率，那么模型面对这种问题肯定是表现不佳的。==

**语言模型的应用**

- 做预训练模型（如 BERT，GPT-3）:给定**大量的文本**做预训练，然后训练模型预测整个文本出现的概率，因此能够得到比较多的训练数据（==文本不需要进行标注，因此会比图像便宜，只需要拿出一定量的文本即可==）来做比较大的模型
- 生成文本，给定前面几个词，不断使用 xt ~ p( xt | x1,x2, ... x(t-1) ) 来生成后续文本（给定前面几个词，不断地采样下一个词，然后一直预测下去），**对模型的要求比较高，否则误差会不断累积**
- 判断多个序列中哪个更常见:使用语言模型判断哪一个序列出现的概率更高（==在打字的时候，输入法自动补全也是根据语言模型来判断句子出现的概率，还可以针对特定用户打字的习惯来进行个性化定制，根据用户之前的使用习惯来进行补全和纠错==）

**使用计数来建模**

- 语言模型可以使用计数来进行建模



假设序列长度为 2 ，可以预测

![img](https://img1.imgtp.com/2023/02/22/eSJrpWhi.png)

- **n:**总词数（或者 token 的个数），也就是采集到的所有样本
- **n(x):**x 在整个词中出现的个数
- **n(x,x'):**连续单词对的出现次数
- **P(x):**统计 x 在数据集中的出现次数，然后将其除以整个语料库中的总词数（这种方法对于频繁出现的单词效果还是不错的）

拓展到序列长度为 3 的情况

![img](https://img1.imgtp.com/2023/02/22/iOYYuEIm.png)

**N 元语法**

- 当序列很长时，因为文本量不够大，很可能 n( x1, ... ,xT) <= 1
- 在序列长度比较长的情况下，可以使用**马尔科夫假设**
- ==对于 N 元语法来说，所要看的子序列的长度是固定的，N 越大，对应的依赖关系越长，精度越高，但是空间复杂度比较大==
- **二元语法、三元语法比较常见**

如果

![img](https://img1.imgtp.com/2023/02/22/VhFygHYX.png)

成立，则序列上的分布满足**一阶马尔科夫性质**，且==阶数越高，对应的依赖关系就越长==

**一元语法（unigram）**

- 马尔科夫假设中的 τ 为 0 ，也就是说每次计算 xt 的概率时，不用考虑 xt 之前的数据
- 使用一元语法计算 p（ x1,x2,x3,x4）:可以认为这个序列中每个词是**独立**的

![img](https://img1.imgtp.com/2023/02/22/2UCeX8xr.png)

**二元语法（bigram）**

- 马尔科夫假设中的 τ 为 1 ，也就是说每次计算 xt 的概率时，只依赖于 x( t-1 )，也就是说**每一个词和前面一个词是相关的**

![img](https://img1.imgtp.com/2023/02/22/55dfUJ47.png)

**三元语法（trigram）**

- 马尔科夫假设中的 τ 为 2 ，也就是说==每次计算 xt 的概率时，只依赖于 x( t - 1 ) 和 x( t - 2 )==，也就是说**每一个词和前面两个词是相关的**

![img](https://img1.imgtp.com/2023/02/22/xXfJnyVg.png)

**N 元语法的好处**

- 最大的好处是**可以处理比较长的序列**。如果序列很长的话，很难把它存下来，不可能将序列中任何长度的序列都存下来，这是一个**指数级的复杂度**
- 所以==对于任意长度的序列，N 元语法所扫描的子序列长度是固定的==:比如说对于二元语法来说，每次只看长为 2 的子序列，首先将长度为 2 的任何一个词 n(x1,x2)（都来自序列中，假设整个字典中有 1000 个词，则长为 2 的词有 1000*1000=1000，000 种可能性）存下来，然后将每一个词和另外一个词组成的词在文本中出现的概率 n(x1,x2) 全部存起来，再把一个词 n(x1) 出现的概率存起来，最后把　ｎ　（也就是１０００）存起来
- 查询一个任意长度的序列的时间复杂度为　ｏ（Ｔ），Ｔ　是**序列长度**
- Ｎ　元语法和　Ｎ　是一个指数关系，随着　Ｎ　的增大，需要存的东西就会变得很大，所以一元语法使用的不多（一元语法完全忽略掉了时序信息），二元语法、三元语法使用的比较多
- 使用马尔科夫假设的　Ｎ　元语法的好处:如果将词存起来，就可以使得计算复杂度变成　ｏ（Ｔ）而不是　ｏ（Ｎ）　，ｏ（Ｔ）　很重要，因为语料库通常会很大，判断一个句子的概率的情况下，每秒钟可能需要做一百万次左右，在语音识别或者是输入法补全的时候需要进行实时的计算，因此计算复杂度非常关键，==对于　Ｎ　元语法来讲，Ｎ　越大，精度越高，但是随着　Ｎ　的增大，空间复杂度也会增大==，即使是这样，二元语法、三元语法也是非常常见的模型

**如何随机生成一个小批量数据的特征和标签以供读取？**

１、==文本序列可以是任意长的，因此任意长的序列可以被划分为具有**相同时间步数**的子序列==

２、训练神经网络时，将这些具有相同步数的小批量子序列输入到模型中

- 假设神经网络一次处理具有　ｎ　个时间步的子序列

![img](https://img1.imgtp.com/2023/02/22/alNB4sGv.png)

- 上图展示了从原始文本序列中获得子序列的所有不同的方式，其中　ｎ　等于　５　，并且每个时间步的词元对应于一个字符

３、由于可以选择**任意偏移量**来指示初始位置，因此具有相当大的自由度

- 如果只选择一个偏移量，那么用于训练网络的、所有可能的子序列的覆盖范围将是有限的。因此可以从随机偏移量开始划分序列，来同时获得**覆盖性（ｃｏｖｅｒａｇｅ）**和**随机性（ｒａｎｄｏｍｎｅｓｓ）**

**随机采样（ｒａｎｄｏｍ　ｓａｍｐｌｉｎｇ）**

- ==随机采样中，每个样本都是在原始的长序列上任意捕获的子序列==
- 在迭代过程中，来自两个**相邻的、随机的、小批量中的**子序列不一定在原始序列上相邻
- 语言建模的目标是**基于当前所看到的词元预测下一个词元**，因此标签是**移位了一个词元**的原始序列

**顺序分区（sequenｔｉａｌ　ｐａｒｔｉｔｉｏｎｉｎｇ）**

- 顺序分区:迭代过程中，除了对原始序列可以随机抽样外，还可以**保证两个相邻的小批量中的子序列在原始序列上也是相邻的**，这种策略在基于小批量的迭代过程中**保留了拆分的子序列的顺序**

**总结**

１、语言模型是自然语言处理的关键，语言模型其实就是**估计文本序列的联合概率**，也是　ＮＬＰ　领域最常见的应用

２、使用统计方法时通常采用　**n元语法**，每次看一个长为 n 的子序列来进行计数，对于给定的长序列拆分成很多个连续的长度为　Ｎ　的子序列，就能够计算**文本序列的联合概率**了。ｎ元语法通过截断相关性，为处理长序列提供了一种实用的模型（==长序列的问题在于它们很少出现或者从不出现==）

3、**齐普夫定律（Ｚｉｐｆ＇ｓ　ｌａｗ）**支配着单词的分布，这个分布不仅适用于一元语法，还适用于其他　ｎ　元语法

![img](https://img1.imgtp.com/2023/02/22/BthfLAEQ.png)



![img](https://img1.imgtp.com/2023/02/22/nhwrpIth.png)

- 想要通过**计数统计**和**平滑**来建模单词是不可行的，因为这样建模会大大高估尾部单词（即不常用的单词）的频率

4、通过**拉普拉斯平滑法**可以有效地处理结构丰富而频率不足的低频词词组

5、读取长序列的主要方式是**随机采样**和**顺序分区**。在迭代过程中，后者可以==保证来自两个相邻的小批量中的子序列在原始序列上也是相邻的==。



## 卷积神经网络(高级篇)

之前的卷积神经网络和多层感知机全连接网络它们的网络架构是串行的，一层输出作为下一层输入  ，但是复杂的神经网络不是串行的，下面结束两种复杂的网络结构

### GoogLeNet

GoogLeNet在2014年由Google团队提出（与VGG网络同年，注意GoogLeNet中的L大写是为了致敬LeNet），斩获当年ImageNet竞赛中Classification Task (分类任务) 第一名。

**GoogleNet的创新点:**

1. 引入了**Inception**结构(融合不同尺度的特征信息)
2. 使用1×1的卷积核进行**降维以及映射处理**（虽然VGG网络中也有，但该论文介绍的更详细）
3. 添加两个**辅助分类器**帮助训练
4. 丢弃全连接层，使用**平均池化层**（大大减少模型参数，除去两个辅助分类器，网络大小只有vgg的1/20)

减少代码冗余:函数/类，对于神经网络我们也可以将重复的块封装起来，这些封装的块叫做**Inception**

<img src="https://i0.hdslb.com/bfs/album/49cbdb5b97c1a543bb461681bbf6f0f85f34486e.png" alt="image-20221201170530726" style="zoom:50%;" />



#### Inception Module 原始结构

GoogLeNet 提出了一种==并联结构==，下图是论文中提出的inception原始结构，将特征矩阵**同时输入到多个分支**进行处理，并将输出的特征矩阵**按深度进行拼接**，得到最终输出。

* inception的作用:增加网络**深度(隐藏层数量)**和**宽度**(神经元数量)的同时减少参数。

![](https://img-blog.csdnimg.cn/20200717121029783.png?#pic_center)

注意:每个分支所得特征矩阵的高和宽必须相同（通过**调整stride和padding**），以保证输出特征能在深度上进行拼接。

**inception+降维**

在 inception 的基础上，还可以加上降维功能的结构，如下图所示，在原始 inception 结构的基础上，在分支2，3，4上加入了**卷积核大小为1x1的卷积层**，目的是为了降维（减小深度），减少模型训练参数，减少计算量。

![](https://i0.hdslb.com/bfs/note/126bf0398e908b04af4fcad04a0ee1c187919917.png)

总的来说，**上图中标记为白色的卷积层可以认为是用来改变通道数的，要么改变输入要么改变输出**；**标记为蓝色的卷积层可以认为是用来抽取信息的**，第1条路中标记为蓝色的卷积层不抽取空间信息，只抽取通道信息，第2、3条路中标记为蓝色的卷积层是用来抽取空间信息的，第4条路中标记为蓝色的最大池化层也是用来抽取空间信息的，增强鲁棒性

经过Inception块之后，最后输出的通道数由输入的192变成了64+128+32+32=256，每个通道都会识别一些特定的模型，所以应该把重要的通道数留给重要的通道（==这里的意思应该是类似于:输入进来之后被复制成了四份，然后经过四条不同的路，最终进行通道数的合并，在输出通道数固定的情况下，四条路的最终输出的通道数是不一样的，所以可以将有限的输出通道数分配给不同的路径，有一点像权重，就比如上图中给第二条路分配了128个输出通道数，接近一半的通道数都留给了3 * 3的卷积层，因为3 * 3的卷积层计算量不大同时能够很好地抽取信息，剩下通道数的一半分给了1 * 1的卷积层，然后再剩下给第3、4条路平分==），大致的设计思路就是这样，但是具体所使用的数值也是调出来的

**Inception代码实现**:

```python
class Inception(nn.Module):
    # c1--c4是每条路径的输出通道数
    def __init__(self,in_channes,c1,c2,c3,c4,**kwargs):
        super().__init__(**kwargs)
        # 线路1，单1×1卷积层
        self.p1_1=nn.Conv2d(in_channes,c1,kernel_size=1)
        # 线路2，1x1卷积层后接3x3卷积层
        self.p2_1=nn.Conv2d(in_channes,c2[0],kernel_size=1)
        self.p2_2=nn.Conv2d(c2[0],c2[1],kernel_size=3,padding=1)
        # 线路3，1x1卷积层后接5x5卷积层
        self.p3_1=nn.Conv2d(in_channes,c3[0],kernel_size=1)
        self.p3_2=nn.Conv2d(c3[0],c3[1],kernel_size=5,padding=2)
        # 线路4，3x3最大汇聚层后接1x1卷积层
        self.p4_1=nn.MaxPool2d(kernel_size=3,stride=1,padding=1)
        self.p4_2=nn.Conv2d(in_channes,c4,kernel_size=1)
    def forward(self,x):
        p1=F.relu(self.p1_1(x))
        p2=F.relu(self.p2_2(F.relu(self.p2_1(x))))
        p3=F.relu(self.p3_2(F.relu(self.p3_1(x))))
        p4=F.relu(self.p4_2(self.p4_1(x)))
        # 在通道维度上连结输出
        return torch.cat((p1,p2,p3,p4),dim=1)
```

**1×1卷积核的降维功能:**

同样是对一个深度为512的特征矩阵使用64个大小为5x5的卷积核进行卷积，不使用1x1卷积核进行降维的 话一共需要819200个参数，如果使用1x1卷积核进行降维一共需要50688个参数，明显少了很多。

![](https://img-blog.csdnimg.cn/20200717122403870.png?#pic_center)

注:==CNN参数个数 = 卷积核尺寸×卷积核深度 × 卷积核组数 = 卷积核尺寸 × 输入特征矩阵深度 × 输出特征矩阵深度==

**辅助分类器（Auxiliary Classifier）**

AlexNet 和 VGG 都只有1个输出层，GoogLeNet 有3个输出层，其中的两个是辅助分类层。![](https://img-blog.csdnimg.cn/20200717161450737.png?#pic_center)

如下图所示，网络主干右边的 两个分支 就是 辅助分类器，其结构一模一样。
在训练模型时，将两个辅助分类器的损失乘以权重（论文中是0.3）加到网络的整体损失上，再进行反向传播。

> 引用:[GoogLeNet(Inception V1)](https://www.cnblogs.com/itmorn/p/11230388.html)
>
> 辅助分类器的两个分支有什么用呢？
>
> 作用一:可以把他看做inception网络中的一个小细节，它确保了即便是隐藏单元和中间层也参与了特征计算，他们也能预测图片的类别，他在inception网络中起到一种调整的效果，并且能防止网络发生过拟合。
> 作用二:给定深度相对较大的网络，有效传播梯度反向通过所有层的能力是一个问题。通过将辅助分类器添加到这些中间层，可以期望较低阶段分类器的判别力。在训练期间，它们的损失以折扣权重（辅助分类器损失的权重是0.3）加到网络的整个损失上。

```python
# 现在，我们逐一实现GoogLeNet的每个模块。第一个模块使用64个通道、7×7卷积层。
b1=nn.Sequential(nn.Conv2d(1,64,kernel_size=7,stride=2,padding=3),nn.ReLU(),nn.MaxPool2d(kernel_size=3,stride=2,padding=1))
# 第二个模块使用两个卷积层:第一个卷积层是64个通道、卷积层；第二个卷积层使用将通道数量增加三倍的卷积层。 这对应于Inception块中的第二条路径。
b2 = nn.Sequential(nn.Conv2d(64, 64, kernel_size=1),
                   nn.ReLU(),
                   nn.Conv2d(64, 192, kernel_size=3, padding=1),
                   nn.ReLU(),
                   nn.MaxPool2d(kernel_size=3, stride=2, padding=1))
# 第三个模块串联两个完整的Inception块。 第一个Inception块的输出通道数为64+128+32+32=256,四个路径之间的输出通道数量比为64:128:32:32=2:4:1:1。第二个和第三个路径首先将输入通道的数量分别减少到96/192=1/2和16/192=1/12，然后连接第二个卷积层。第二个Inception块的输出通道数增加到128+192+96+64=480，四个路径之间的输出通道数量比为128:192:96:64=4:6:3:2。第二条和第三条路径首先将输入通道的数量分别减少到128/256=1/2和32/256=1/8。
b3 = nn.Sequential(Inception(192, 64, (96, 128), (16, 32), 32),
                   Inception(256, 128, (128, 192), (32, 96), 64),
                   nn.MaxPool2d(kernel_size=3, stride=2, padding=1))
# 第四模块更加复杂， 它串联了5个Inception块， 其输出通道数分别是192+208+48+64=512、160+224+64+64=512、128+256+64+64=512、112+288+64+64=528和256+320+128+128=832。这些路径的通道数分配和第三模块中的类似，首先是含3×3卷积层的第二条路径输出最多通道，其次是仅含1×1卷积层的第一条路径，之后是含5×5卷积层的第三条路径和含3×3最大汇聚层的第四条路径。其中第二、第三条路径都会先按比例减小通道数。这些比例在各个Inception块中都略有不同。
b4 = nn.Sequential(Inception(480, 192, (96, 208), (16, 48), 64),
                   Inception(512, 160, (112, 224), (24, 64), 64),
                   Inception(512, 128, (128, 256), (24, 64), 64),
                   Inception(512, 112, (144, 288), (32, 64), 64),
                   Inception(528, 256, (160, 320), (32, 128), 128),
                   nn.MaxPool2d(kernel_size=3, stride=2, padding=1))
# 第五模块包含输出通道数为256+320+128+128=832和384+384+128+128=1024的两个Inception块。其中每条路径通道数的分配思路和第三、第四模块中的一致，只是在具体数值上有所不同。需要注意的是，第五模块的后面紧跟输出层，该模块同N iN一样使用全局平均汇聚层，将每个通道的高和宽变成1。最后我们将输出变成二维数组，再接上一个输出个数为标签类别数的全连接层。
b5 = nn.Sequential(Inception(832, 256, (160, 320), (32, 128), 128),
                   Inception(832, 384, (192, 384), (48, 128), 128),
                   nn.AdaptiveAvgPool2d((1,1)),
                   nn.Flatten())
net=nn.Sequential(b1,b2,b3,b4,b5,nn.Linear(1024,10))
```



**Inception的变种**

Inception-BN（V2）:使用batch normalization

Inception-V3:修改了Inception块

- 替换5 * 5为多个3 * 3卷积层
- 替换5 * 5为多个1 * 7和7 * 1卷积层
- 替换3 * 3为多个1 * 3和3 * 1卷积层
- 更深

![](https://i0.hdslb.com/bfs/note/b4e542db578f0e3a5ae8c24b3c8e491369893eb0.png)

![](https://i0.hdslb.com/bfs/note/a6ab010c4981fb470ca09a5efe7dae819a5731ed.png)

![](https://i0.hdslb.com/bfs/note/8e04e7fb01b84762a3cfdd8714a22cc77c8117c0.png)

**总结**

- Inception块有四条不同超参数的卷积层和池化层的路来抽取不同的信息（等价于一个有4条路径的子网络，通过不同窗口形状的卷积层和最大汇聚层来并行抽取信息，并使用1 * 1卷积层减少每像素级别上的通道维数从而降低模型的复杂度），它的一个主要优点是**模型参数小，计算复杂度低**
- GoogLeNet使用了9个Inception块**（每个Inception块中有6个卷积层，所有Inception块中一共有54个卷积层）**，这些Inception块与其他层**（卷积层、全连接层）**串联起来，其中Inception块的通道数分配之比是在Imagenet数据集上通过大量的实验得来的
- GoogLeNet是第一个达到上百层的网络，但是不是深度是100，直到ResNet的出现才达到了模型的深度达到100层，这里的上百层指的是通过设计并行的通道来使得模型达到数百层
- Inception后续也有一系列的改进，GoogLeNet V3和GoogLeNet V4目前依旧在被使用，GoogLeNet一开始的精度其实不高，在BN、V3、V4之后精度才慢慢提升上去了，现在也是比较常用的模块，它**以较低的计算复杂度提供了类似的测试精度**
- GoogLeNet的问题是**特别复杂，通道数的设置没有一定的选择依据，以及内部构造比较奇怪**，这也是GoogLeNet不那么受欢迎的原因所在

torch.cat的作用

`torch.cat`是PyTorch中的一个函数，用于沿着一个维度连接输入的多个张量。它可以接受任意数量的张量作为输入，并返回一个新的张量，其中包含所有输入张量中的元素。

```python
import torch
a = torch.ones(2, 3)
b = torch.zeros(2, 3)
c = torch.cat((a, b), dim=0)
c
>>>tensor([[1., 1., 1.],
        [1., 1., 1.],
        [0., 0., 0.],
        [0., 0., 0.]])
```

nn.Flatten()的作用

`nn.Flatten()` 是 PyTorch 中的一个模块，它实现了对输入张量的展平操作。

展平操作是指将输入张量中的所有元素按顺序排列在一起，形成一个一维的张量。它会将多维的张量压缩到一维, 例如:

```python
import torch.nn as nn
x = torch.randn(2, 3, 4)
flatten = nn.Flatten()
y = flatten(x)
print(y.shape)
>>> torch.Size([2, 24]) #就是变成二维
```

展平操作通常在卷积层和全连接层之间使用，它可以将卷积层的多维输出展平为一维，以便全连接层可以处理。



```python
```







**Residual net**

梯度消失:权重小于1，反向传播相乘梯度趋于0，权重无法迭代

**Residual net**有跳链接

<img src="https://i0.hdslb.com/bfs/album/0197670d778fcf5504b5608519167bce2c6b2505.png" alt="image-20221202112231694" style="zoom:50%;" />

通过+x使得梯度大于1来解决梯度消失问题

![image-20221202112820355](https://i0.hdslb.com/bfs/album/721e71b703538c0961ee6753718dc1ce78a11323.png)

跳链接要求输入输出张量一样

Residual Block结构图

<img src="https://i0.hdslb.com/bfs/album/a697c42d2d0172ab6a8b735225795226bb1dab60.png" alt="image-20221202114224690" style="zoom:33%;" />

```python
class ResidualBlock(nn.Module):
    def __init__(self,channels) -> None:
        super().__init__()
        self.channels=channels
        # 输入输出通道数相等
                        self.conv1=nn.Conv2d(channels,channels,kernel_size=3,padding=1)
        self.conv2=nn.Conv2d(channels,channels,kernel_size=3,padding=1)
    def forward(self,x):
        y=nn.ReLU(self.conv1(x))
        y=self.conv2(y)
        # 这里跳链接+x
        return nn.ReLU(x+y)
```

整个经过Residual Block处理的结构图

<img src="https://i0.hdslb.com/bfs/album/7845d6667d0a085742db669d35d9a016e858f1a5.png" alt="image-20221202150854838" style="zoom:50%;" />

```python
class Net(torch.nn.Module):
    def __init__(self) -> None:
        super().__init__()
        self.conv1=torch.nn.Conv2d(1,16,kernel_size=5)
        self.conv2=torch.nn.Conv2d(16,32,kernel_size=5)
        self.mp=torch.nn.MaxPool2d(2)
        # 这里应用两次ResidualBlock去除梯度消失
        self.rblock1=ResidualBlock(16)
        self.rblock2=ResidualBlock(32)
        self.fc=torch.nn.Linear(512,10)
    def forward(self,x):
        in_size = x.size(0)
        x = self.mp(F.relu(self.conv1(x)))
        x = self.rblock1(x)
        x = self.mp(F.relu(self.conv2(x)))
        x = self.rblock2(x)
        x = x.view(in_size, -1)
        x = self.fc(x)
        return x
```

消除梯度消失后正确率达到了99%

<img src="https://i0.hdslb.com/bfs/album/6c4fdb425240bb482edf13f2c0dcba4837cea887.png" alt="image-20221202151915294" style="zoom: 33%;" />

## 循环神经网络

RNN 跟传统神经网络最大的区别在于每次都会将前一次的输出结果，带到下一次的隐藏层中，一起训练。如下图所示:

![](https://easyai.tech/wp-content/uploads/2022/08/f0116-2019-07-02-rnn-1.gif)

3D效果看起来其实是添加了隐藏层神经元(蓝色部分)的不同时刻的状态![image-20230226160037014](https://img1.imgtp.com/2023/02/26/vfy2N22V.png)

![image-20230223204954251](https://img1.imgtp.com/2023/02/23/QbVrYjL1.png)

![](https://easyai.tech/wp-content/uploads/2022/08/575e2-2019-07-02-input-5.gif)

潜变量自回归模型

使用潜变量 ht 总结过去的信息 

![](https://i0.hdslb.com/bfs/note/dd2c64216c535e17245625c06fd5d21aab7d48fa.png)

* n 元语法模型中，单词 $x_t$ 在时间步 t 的条件概率仅取决于前面 n-1 个单词
* $x_t$ 是和 $h_t$ 与 $x_{t-1}$ 相关的
  t 时刻的潜变量 $h_t$ 是和 $h_{t-1}$ 和 $x_{t-1}$ 相关的

==隐藏层和隐状态的区别==:

* 隐藏层是在从输入到输出的路径上（以**观测角度**来理解）的隐藏的层
* 隐状态是在给定步骤所做的任何事情（以**技术角度**来定义）的输入，并且这些状态只能通过先前时间步的数据来计算 i

**循环神经网络是具有隐状态的神经网络**

假设有一个观察 x 和一个隐变量 $h_t$ ，根据 $h_t$就能够生成输出 $o_t$ 

![](https://i0.hdslb.com/bfs/note/08616883b934b1022184483dc61728068c631036.png)

* t 时刻的输出 $o_t$ 是根据 $h_t$ 输出的，$h_t$ 使用的是 $x_{t-1}$ 中的内容
* 在计算损失的时候，是比较 $o_t$ 和 $x_t$ 之间的损失
  $x_t$ 是用来更新 $h_t$ 使得观察 $x_t$ 向后移动
* 捕获并保留序列直到当前时间步的历史信息（如当前时间步下神经网络的状态或记忆）的隐藏变量被称为隐状态(hidden state) 

![](https://i0.hdslb.com/bfs/note/ee0d7c73ea52cfb823185841f5d4e07e2c693a40.png)

* 首先拼接当前时间步 t  的输入 $X_t$ 和前一时间步 t-1 的隐状态 $H_(t-1)$
* 然后将拼接的结果送入带有激活函数 Φ 的全连接层
* 全连接层的输出就是当前时间步 t 的隐状态 $H_t$ 

循环神经网络与 MLP（多层感知机）的区别就在于多了一个**时间轴**，假设没有这个时间关系的话，循环神经网络就会退化成为 MLP

* 保存了前一个时间步的隐藏状态 h(t-1)
* 引入了一个新的权重参数 $W_{hh}$ 来描述如何在当前时间步中使用前一个时间步的隐藏变量 

* 假设 $h_t$是一个隐藏状态，Φ 是激活函数。$W_{hx}$ 是 MLP 隐层的权重，$x_(t-1)$ 是 t-1 时刻的输入，$b_h$ 是对应的偏移量
* $h_t$ 不仅和 x 相关，还和前一时刻的 h 相关
* 输出是由隐藏状态乘上权重再加上一个偏移量得到的
  所以，

1、循环神经网络就是在 MLP 的基础上加了一项 $W_{hh}*h_{t-1}$ 使得它能够于前一时刻的 $h_{t-1}$ 产生关联，假设忽略掉时间轴的情况下，本身和 MLP 是没有任何区别的

2、循环神经网络并没有对 x 进行建模，所有 x 之间的时序信息都存储在潜变量 h 当中

* ==实际上就相当于是将时序信息存储在了$W_{hh}$== 中

3、因为在当前时间步中，隐状态使用的定义与前一个时间步中使用的定义相同，因此隐藏变量的计算是循环的，于是基于循环计算的隐状态神经网络被命名为**循环神经网络**(recurrent neural network)，循环神经网络中执行隐藏变量计算的层称为**循环层**(recurrent layer) 

**循环神经网络在语言模型中的使用**

基于循环神经网络的字符级语言模型

* 语言模型的目标是根据过去的的和当前的词元预测下一个词元（==因此需要将原始序列移位一个词元作为标签==）
* 为简化训练，这里所使用的是字符级语言模型，将文本词元化为字符而不是单词 

训练步骤

输入序列和标签分别是“machine”和“achine” 

![](https://i0.hdslb.com/bfs/note/f72eee05140b82f50946c6cd4cdf2a57503aa4ea.png)

在训练过程中，对每个时间步的输出层的输出进行 `softmax `操作，然后利用交叉熵损失计算模型输出和标签之间的误差

* 以上图中第3个时间步的输出 $O_3$为例，由于隐藏层中隐状态的循环计算，$O_3$ 由文本序列 “m” 、“a” 和 “c” 确定
* 由于训练数据中输入文本序列的下一个字符是 “h” ，因此第三个时间步的损失将取决于下一个字符的概率分布，而下一个字符是基于特征序列 “m”、“a”、“c” 和这个时间步的标签 “h” 生成的 

![](https://i0.hdslb.com/bfs/note/e0a9cc366f02296eb90651241e21b6758bc527d8.png)

语言模型中，假设当前输入是“你”的话，那么会更新隐变量，然后预测“好”，接下来观测到了“好”，然后更新隐变量，再输出逗号

* 输出是用来匹配观察的，但是在生成 t 时刻的输出 $o_t$ 时，不能够看到 t 时刻的观察 $x_t$ ，也就是说当前时刻的输出是用来预测当前时刻的观察，但是输出发生在观察之前

**困惑度（perplexity）**

语言模型实际上就是一个分类模型，假设字典大小是 m 的话，语言模型实际上就是 m 类的分类问题，每次预测下一个词的时候，实际上就是在预测下一个词的类别

衡量分类问题的好坏可以用交叉熵，因此，衡量一个语言模型的好坏可以用平均交叉熵 

![](https://i0.hdslb.com/bfs/note/0bf512832b32bc1a584a96259d3cdf9644315fed.png)

* n:序列长度，长为 n 的序列就相当于是做 n 次预测，也就是做了 n 次分类，因此衡量一个语言模型的好坏可以在交叉熵的基础上取平均
* p:语言模型的预测概率，由语言模型给出
* $x_t$:真实词，在时间步 t 从该序列中观察到的实际词元

由于历史原因 NLP 使用困惑度 exp(π) 来衡量，平均每次可能的选项，也就是对上面的平均交叉熵做指数运算

* 为什么做指数运算？好处是做完指数运算之后结果会变大，数值变大之后就能够很容易看出来改进所带来的好处

  困惑度的最好的理解是“下一个词元的实际选择数的调和平均数”，也可以认为是下一个词的候选数量，这样在直观上更容易理解

* 在最好的情况下，模型总是完美地估计标签词元的概率为1。 在这种情况下，模型的困惑度为1
* 在最坏的情况下，模型总是预测标签词元的概率为0。 在这种情况下，困惑度是正无穷大，表示根本预测不出下一个词到底是什么
* 在基线上，该模型的预测是词表的所有可用词元上的均匀分布。 在这种情况下，困惑度等于词表中唯一词元的数量。 事实上，如果在没有任何压缩的情况下存储序列， 这将是我们能做的最好的编码方式。 因此，这种方式提供了一个重要的上限， 而任何实际模型都必须超越这个上限 

**梯度裁剪**

迭代中计算这 T 个时间步上的梯度，在反向传播过程中产生长度为 O(T) 的矩阵乘法链，导致数值不稳定

一连串的矩阵相乘的话会导致结果要么很小，要么很大:如果结果很小的话可能会导致训练不动；如果结果很大的话，可能会发生梯度爆炸导致结果出错
梯度裁剪能够有效预防梯度爆炸

如果梯度长度超过 θ ，那么拖影回长度 θ 
![](https://i0.hdslb.com/bfs/note/6c9364ea6c37587e227f229ab4b4ba9846a25ced.png)

* g:所有层上的梯度全部放在向量 g  当中
* 假设 g 的长度 L 太长导致超过了 θ ，那么就需要将长度 L 降回到 θ ，也就是说只要是 g 的长度是正常的就不做任何操作，如果 g 的长度超出了给定的范围，就将其裁剪回给定的范围中，这样就能够有效地避免梯度爆炸

**更多应用**

![](https://i0.hdslb.com/bfs/note/ac002807c025772c2c52c96b26860d7a3c8c3e9f.png)

**一对一**

* 也就是最简单的 MLP :给定一个样本，然后输出一个标签

**一对多**

* 文本生成:给定一个词，然后生成一个一个的词

**多对一**

* 文本分类:给定一个序列，在最后的时刻输出，得到具体的分类

**多对多**

* 问答、机器翻译:给定一个序列，先不输出，然后在序列输入完毕之后输出答案
* Tag 生成:给定一个序列，然后输出每个词的 Tag ，比如输入一个句子，然后输出句子中每个词是名词、动词还是形容词等 

**总结**

1、对隐藏状态使用循环计算的神经网络称为**循环神经网络（RNN**），==循环神经网络的输出取决于当下输入和前一时间的隐变量==

* 循环神经网络的隐藏状态可以捕获**当前时间步序列的历史信息**
* 隐变量是用来存储**历史信息和下一个历史信息的转换规则**，所以在拿到过去的输入和当前的隐藏状态就能够预测当前的输出

* Whh 拥有一定的**时序预测**目的

2、应用到语言模型中时，循环神经网络根据当前词预测下一次时刻词

* 根据当前的输入更新当前时刻的隐藏状态就能够预测下一个时刻的输出
* RNN 是一个隐变量模型，**隐变量是一个向量**

3、通常使用困惑度来衡量语言模型的好坏

* 取平均值，然后进行指数操作，就得到了困惑度
* ==困惑度实际上衡量了语言模型对下一个词的预测所选取的候选词数量，这个候选词的数量越少越好==

4、为了解决梯度爆炸的问题，RNN 一般是需要进行**梯度裁剪**的，这也是最简单的方法

5、==循环神经网络模型的参数数量不会随着时间步的增加而增加==

6、使用循环神经网络可以创建字符级语言模型 

![image-20230224180452632](https://img1.imgtp.com/2023/02/24/Y6ehDs5i.png)

### 门控循环单元GRU

* GRU 是最近几年提出来的,在 **LSTM** 之后,是一个稍微简化的变体,通常能够提供同等的效果,并且计算速度更快


在某些情况下,希望存在某些机制能够实现:

* 希望某些机制能够在一个记忆元里**存储重要的早期信息**
* 希望某些机制能够**跳过隐状态**表示中的此类词元
* 希望某些机制能够**重置内部状态**表示


做 RNN 的时候处理不了太长的序列

* 因为序列信息全部放在隐藏状态中,当时间到达一定长度的时候,隐藏状态中会累积过多的信息,不利于**相对靠前的信息的抽取**

![](https://i0.hdslb.com/bfs/note/27fde0d1f7a2114a1e5b05d4a6c3f208c53a2126.png)

在观察一个序列的时候,不是每个观察值都同等重要

* 对于一个猫的图片的序列突然出现一只老鼠,老鼠的出现很重要,第一次出现猫也很重要,但是之后再出现猫就不那么重要了
* 在一个句子中,可能只是一些**关键字或者关键句**比较重要
  视频处理中,其实帧与帧之间很多时候都差不多,但是在切换场景的时候,每次的切换是比较重要的

在 RNN 中没有特别关心某些地方的机制,对于它来讲仅仅是一个序列,而门控循环单元可以通过一些**额外的控制单元**,使得在构造隐藏状态的时候能够**挑选出**相对来说更加重要的部分(**注意力机制**在这方面强调得更多一点)

* **更新门(update gate)**:能关注的机制,能够将信息尽量放在隐藏状态中,==控制新状态中有多少个是旧状态的副本==
* **重置门(reset gate)**:能遗忘的机制,能够遗忘输入或者隐藏状态中的一些信息,==控制"可能还想记住"的过去状态的数量==

**门**

![](https://i0.hdslb.com/bfs/note/f784480af7019d7ea228c21c1e76f544f14f3bb9.png)

* 上图表示门控循环单元模型,输入是由**当前时间步的输入**和**前一时间步的隐状态**给出;重置门和更新门的输出是由**使用 sigmoid** 激活函数的两个全连接层给出
* **$X_t$** :输入
* **$H_{t-1}$**:隐藏状态
* **$R_t$** :重置门.如果是 RNN 的话,所表示的是使用 sigmoid 作为激活函数对应的隐藏状态的计算
* **$Z_t$** :更新门.计算方式和 RNN 中隐藏状态以及 $R_t$ 的计算方式是一样的
  门可以认为是和隐藏状态同样长度的向量,它的计算方式和 RNN 中隐藏状态的计算方式是一样的 

**候选隐状态(candidate hidden state)**

* 并不是真正的隐藏状态,只是用来生成真正的隐藏状态

![](https://i0.hdslb.com/bfs/note/c69cf355134558d64e4e79d5689e05470154a1e5.png)

* Rt 与 H(t-1) 按元素乘法,对于一个样本来讲, Rt 和 H(t-1)是一个长度相同的向量,所以可以按照元素做乘法
* Rt 是一个取值为 0~1 的值, Rt 越靠近 0 , Rt 与 H(t-1)按元素乘法得到的结果就越接近 0 ,也就相当于将==上一时刻的隐藏状态忘掉==
* 极端情况下,如果 Rt 全部变成 0 的话,就相当于从当前时刻开始,前面的信息全部不要,隐藏状态变成 0 ,==从初始化状态开始,任何预先存在的隐状态都会被重置为默认值==
* 另外一个极端情况:如果 Rt 全是 1 的话,就表示,将当前时刻之前所有的信息全部拿过来做更新,就等价于 RNN 中隐藏状态的更新方式
* 实际上 Rt 是一个可以学习的参数,所以它会根据前面的信息来学习哪些信息是能够进入到下一轮隐藏状态的更新,哪一些信息需要进行舍弃,这些操作都是自动进行的,因此被叫做控制单元 

**隐状态**

* 门控循环单元与普通的循环神经网络之间的关键区别在于:前者支持隐状态的门控,这意味着模型有专门的机制来确定应该何时更新隐状态,以及应该何时重置隐状态(这些机制都是可学习的)
* 真正的隐藏状态的计算方式如下所示

![](https://i0.hdslb.com/bfs/note/e25c08ca27857a7a65d549e9b53a0264b21d5a75.png)

* Zt 也是一些取值为 0~1 的一些数字组成的
* 假设 Zt 都等于 1 ,即 Ht 等于 H(t-1),相当于不使用 $X_t$ 来更新隐藏状态,直接将过去的状态当成现在的状态,模型**只保留旧状态**,此时,来自 $X_t$ 的信息基本上被忽略.==当整个子序列的所有时间步的更新门都接近于 1 ,则无论序列的长度如何,在序列起始时间步的旧隐状态都将很容易保留并传递到序列结束==
* 假设 Zt 都等于 0 , Ht 等于候选隐状态 Ht tittle .相当于回到了 RNN 的情况,不看过去的隐藏状态,只看现在更新的隐藏状态,==能够帮助处理循环神经网络中的梯度消失问题,并且能够更好地捕获时间步距离很长的序列的依赖关系==

**总结**

![](https://i0.hdslb.com/bfs/note/c0cbaeb8380587e54bc7c1e8be435bc43b6ed952.png)

* GRU 中引入了两个额外的门,每个门可以学习的参数和 RNN 一样多,整个可学习的权重数量是 RNN 的**三倍**
* Rt 和 Zt 都是控制单元,用来输出取值为 0~1 的数值
* Rt 用来衡量在更新新的隐藏状态的时候,要用到==多少过去隐藏状态==的信息
* Zt 用来衡量在更新新的隐藏状态的时候,需要用到==多少当前$X_t$相关==的信息
* 当 Zt 全为 0 , Rt 全为 1 时,等价于 RNN
* 当 Zt 全为 1 时,直接忽略掉当前 Xt
* GRU 通过引入 Rt 和 Zt ,从而能够在各种极端情况之间进行调整
* ==门控循环神经网络可以更好地捕获时间步距离很长的序列上的依赖关系==
* 重置门有助于==捕获序列中的短期依赖关系==
* 更新门有助于==捕获序列中的长期依赖关系==
* 重置门打开时,门控循环单元包含基本循环神经网络
* 更新门打开时,门控循环单元可以跳过子序列

### 长短期记忆网络(LSTM)

之前的RNN是一种short 记忆

![image-20230226160323278](https://img1.imgtp.com/2023/02/26/V2MV2UaY.png)

- 长期以来，隐变量模型存在着**长期信息保存和短期输入缺失**的问题，解决这个问题最早的方法之一就是 LSTM
- 发明于90年代
- 使用的效果和 GRU 相差不大,但是使用的东西更加复杂

LSTM在RNN的基础上增加了一条时间链，计算当前隐藏层除了输入$X_t$、前一时刻$S_{t-1}$还要有当前时刻的==日记信息==$C_t$

![image-20230226160720572](https://img1.imgtp.com/2023/02/26/vXZwvWW6.png)

展开来是这样的

![image-20230226160846015](https://img1.imgtp.com/2023/02/26/Gw03SJ63.png)

函数f1是橡皮擦根据昨天的记忆$S_{t-1}$和今天的输入$X_t$来决定修改==日记==中哪些内容，而函数f2就是增加日记内容，公式如下:

$f_1=sigmoid(w_1 \begin{bmatrix} s_t-1 \\ x_t \end{bmatrix}+b_1)$   $f_2=sigmoid(w_2 \begin{bmatrix} x_t-1 \\ x_t \end{bmatrix}+b_2)*tanh(\hat{w_2}\begin{bmatrix} S_t-1 \\ X_t \end{bmatrix}+\hat{b_2})$  

$c_t=f_1*c_{t-1}+f_2$     $c_t$除了继续向下更新，还会更新当前短期记忆$S_t$

**门**

- 长短期记忆网络的设计灵感来自于**计算机的逻辑门**
- 长短期记忆网络引入了记忆元(memory cell)，或简称为单元(cell)（有些文献认为记忆元是隐状态的一种特殊类型，它们与隐状态具有相同的形状，其设计目的是用于记录附加的信息）
- 长短期记忆网络有三个门:**忘记门**（重置单元的内容，通过专用机制决定什么时候记忆或忽略隐状态中的输入）、**输入门**（决定何时将数据读入单元）、**输出门**（从单元中输出条目）,门的计算和 GRU 中相同,但是命名不同
- 忘记门(forget gate):**将值朝 0 减少**
- 输入门(input gate):**决定是否忽略掉输入数据**
- 输出门(output gate):**决定是否使用隐状态**

![](https://i0.hdslb.com/bfs/note/d14c5c5ef36c9103d735ba816b32c22c6e6dbd9b.png)

**候选记忆单元（candidate memory cell）**

![](https://i0.hdslb.com/bfs/note/897195cf10a4b487480c65f368b54c138589f0de.png)

**记忆单元**

![](https://i0.hdslb.com/bfs/note/bb0acc7a555691f7d901a3cfde53638e041d2855.png)

- 在长短期记忆网络中，通过**输入门**和**遗忘门**来控制输入和遗忘（或跳过）:输入门 It 控制**采用多少来自 Ct tilde 的新数据**，而遗忘门 Ft 控制**保留多少过去的记忆元 C(t-1) 的内容**
- 如果遗忘门始终为 1 且输入门始终为 0 ，则过去的记忆元 C(t-1) 将随时间被保存并传递到当前时间步（引入这种设计是为了缓解梯度消失的问题，并更好地捕获序列中的长距离依赖关系） 
- 上一时刻的记忆单元会作为状态输入到模型中
- LSTM 和 RNN/GRU 的不同之处在于: LSTM 中的状态有两个, C 和 H

**隐状态**

![img](https://img1.imgtp.com/2023/02/26/eQlAvcxr.png)

- 在长短期记忆网络中，隐状态 Ht 仅仅是**记忆元 Ct 的 tanh 的门控版本**，因此确保了 Ht 的值始终在 -1~1 之间
- tanh 的作用:将 Ct 的值限制在 -1 和 1 之间
- Ot 控制是否输出, Ot 接近 1 ,则能**有效地将所有记忆信息传递给预测部分**; Ot 接近 0 ,表示**丢弃当前的 Xt 和过去所有的信息，只保留记忆元内的所有信息，而不需要更新隐状态**

**总结**

![img](https://img1.imgtp.com/2023/02/26/ejc4dnhL.png)

![img](https://img1.imgtp.com/2023/02/26/MaOgNWr0.png)

### 深层循环神经网络

**回顾:循环神经网络**

![img](https://img1.imgtp.com/2023/02/26/r5TLxwgV.png)

如何将循环神经网络变深，以获得更多的非线性性？

- 通过**添加多个隐藏层**的方式来实现（和 MLP 没有本质区别），每个隐藏状态都连续地传递到**当前层的下一个时间步**和**下一层的当前时间步**

![img](https://img1.imgtp.com/2023/02/26/zUyaX88R.png)

![img](https://img1.imgtp.com/2023/02/26/9M8PTfvG.png)

- 类似于多层感知机，**隐藏层数目**和**隐藏单元数目**都是超参数（它们是可以进行调整的）
- 使用**门控循环单元**或**长短期记忆网络**的隐状态替代上图中深度循环神经网络中的隐状态计算，就能够很容易地得到**深度门控循环神经网络**或**长短期记忆神经网络**

**总结**

1、深度循环神经网络使用多个隐藏层来获得更多的非线性性

- GRU、RNN、LSTM 在结构上都是相同的，只是隐状态 H 的计算方式有区别，所以它们加深神经网络的原理都是相同的

2、在深度循环神经网络中，隐状态的信息被传递到**当前层的下一时间步**和**下一层的当前时间步**

3、存在许多不同风格的深度循环神经网络，如**长短期记忆网络**、**门控循环单元**或**经典循环神经网络**

4、深度循环神经网络需要大量的**调参**（如学习率和修剪）来确保合适的收敛，模型的初始化也需要谨慎

### 双向神经网络

- 对于序列来讲，通常假设的目标是:==在给定观测的情况下（如在时间序列的上下文中或语言模型的上下文中），对下一个输出进行建模。虽然这是一个典型的场景，但并不是唯一的==
- 对于序列模型来讲，可以从前往后看，也可以从后往前看，在某些情况下是可以的，有的情况下是不行的

**双向模型**

![img](https://img1.imgtp.com/2023/02/26/nKRq8kKP.png)

- 双向循环神经网络的隐藏层中有两个隐状态（**前向隐状态和反向隐状态**，==通过添加反向传递信息的隐藏层来更灵活地处理反向传递的信息==）:以输入 X1 为例，当输入 X1 进入到模型之后，当前的隐藏状态（右箭头，前向隐状态）放入下一个时间步的状态中去；X2 更新完隐藏状态之后，将更新后的隐藏状态传递给 X1 的隐藏状态（左箭头，反向隐状态），将两个隐藏状态（前向隐状态和反向隐状态）合并在一起，就得到了需要送入输出层的隐状态 Ht （==在具有多个隐藏层的深度双向循环神经网络中，则前向隐状态和反向隐状态这两个隐状态会作为输入继续传递到下一个双向层（具有多个隐藏层的深度双向循环神经网络其实就是多个双向隐藏层的叠加））==，最后输出层计算得到输出 Ot 

![img](https://img1.imgtp.com/2023/02/26/djblXBBm.png)

- 实现:将输入复制一遍，一份用于做前向的时候，正常的隐藏层会得到一些输出；另一份用于做反向的时候，反向的隐藏层会得到另外一些输出，然后进行前后顺序颠倒。将正向输出和颠倒顺序后的反向输出进行**合并**（concat），就能得到最终的输出了

**模型的计算代价及其应用**

双向循环神经网络的一个关键特性是:**使用来自序列两端的信息来估计输出**

- 使用过去和未来的观测信息来预测当前的观测，因此并不适用于预测下一个词元的场景，因为在预测下一个词元时，并不能得知下一个词元的下文，因为不会得到很好的精度
- ==如果使用双向循环神经网络预测下一个词元，尽管在训练的时候能够利用所预测词元过去和未来的数据（也就是所预测词元的上下文）来估计所预测的词元，但是在测试的时候，模型的输入只有过去的数据（也就是所预测词所在位置之前的信息），所以会导致精度很差==

此外，双向循环神经网络的**计算速度非常慢**

- ==主要原因是网络的前向传播需要在双向层中进行前向和后向递归，并且网络的反向传播也以依赖于前向传播的结果，因此梯度求解将有一个非常长的链==

双向层在实际中的时用的比较少，仅仅应用于部分场合:

- **填充缺失的单词**
- **词元注释**（如命名实体识别）
- **作为序列处理流水线中的一个步骤对序列进行编码**（如机器翻译）

**训练**

![img](https://img1.imgtp.com/2023/02/26/DPAnPa8p.png)

**推理**

![img](https://img1.imgtp.com/2023/02/26/TSv9vOa1.png)

- 双向 LSTM 不适合做推理，几乎不能用于预测下一个词，因为为了得到隐藏状态 H ，既要看到它之前的信息，又要看到之后的信息，因为在推理的时候没有之后的信息，所以是做不了推理的
- 双向循环神经网络主要的作用是对句子做特征提取，比如在做翻译的时候，给定一个句子去翻译下一个句子，那么可以用双向循环神经网络来做已给定句子的特征提取；或者是做改写等能够看到整个句子的应用场景下，做整个句子的特征提取

**总结**

1、双向循环神经网络通过**反向更新的隐藏层**来利用方向时间信息

2、在双向循环神经网络中，**每个时间步的隐状态由当前时间步的前后数据同时决定**

3、双向循环神经网络与概率图模型中的“前向-后向”算法具有相似性

4、双向循环神经网络主要用于序列编码和给定双向上下文的观测估计，通常用来对序列抽取特征、填空，而不是预测未来

5、由于**梯度链更长**，因此双向循环神经网络的**训练代价非常高**
## 编码器-解码器

**回顾CNN**

![img](https://i0.hdslb.com/bfs/note/616a5f41a6de7c40765d47d7b6d87593e3d4b506.png)

在 CNN 中，输入一张图片，经过多层的卷积层，最后到输出层判别图片中的物体的类别

- CNN 中使用**卷积层**做**特征提取**，使用 **Softmax 回归**做**预测**
- 从某种意义上来说，特征提取可以看成是编码， Softmax 回归可以看成是解码
- 编码器:将输入编程成**中间表达形式（特征）**
- 解码器:将中间表示解码成输出

**回顾RNN**

![img](https://i0.hdslb.com/bfs/note/c286ff66d45e39e918c07812cc662a00150b0e98.png)

对于 RNN 来讲，输入一个句子，然后对其进行向量输出

- 如果将最终 RNN 的输出当成中间表示的话，这部分也可以当成是编码器
- 最后通过全连接层得到最终的输出的话，这部分可以看成是解码器
- 编码器:将**文本表示**成**向量**
- 解码器:将**向量**表示成**输出**

**编码器-解码器架构**

![img](https://i0.hdslb.com/bfs/note/9d24069e99f999f0d3a21af119f892041a5e234d.png)

机器翻译是序列转换模型的一个核心问题，它的输入和输出都是长度可变的序列



一个模型被分为两块:

- 编码器（encoder）处理输入:**接受一个长度可变的序列作为输入，并将其转换为具有固定形状的编码状态。**编码器在拿到输入之后，将其表示成为中间状态或者中间表示（如**隐藏状态、特征图**）
- 解码器（decoder）生成输出:**解码器将固定形状的编码状态映射到长度可变的序列。**最简单的解码器能够直接将中间状态或者中间表示翻译成输出；解码器也能够结合一些额外的输入得到输出

**总结**

- 使用编码器-解码器架构的模型，编码器负责表示输入，解码器负责输出
- “编码器－解码器”架构可以将长度可变的序列作为输入和输出，因此适用于**机器翻译**等序列转换问题。
- 编码器将长度可变的序列作为输入，并将其转换为具有固定形状的编码状态。
- 解码器将具有固定形状的编码状态映射为长度可变的序列。

## 序列到序列学习(seq2seq)


![img](https://img1.imgtp.com/2023/02/26/T02fjbCs.png)

- 上图展示的是 DNA 转录，它也是一种**序列到序列**的学习

**机器翻译**

- seq2seq 最早是用来做机器翻译的，给定一个源句子，自动翻译成目标语言

![](https://i0.hdslb.com/bfs/note/feb4bce0affe4ecd8c0fccdd4863fcc2039f2105.png)

- 给定一个源语言的句子，自动翻译成目标语言
- 机器翻译中的输入序列和输出序列都是**长度可变**

**seq2seq**

**编码器-解码器（encoder-decoder）架构**

![](https://i0.hdslb.com/bfs/note/be9afe361069964b3af9b98892257fc94306e685.png)

- seq2seq 指的是一个特定的模型，它的编码器是一个 **RNN**（循环神经网络），使用长度可变的序列作为输入，将其转换为固定形状的隐状态；然后将**最终的隐藏状态**传给解码器，==隐藏状态包括了整个源句子（输入序列）的信息==；解码器使用另外一个 RNN ，基于**输入序列的编码信息和输出序列已经看见的或者生成的词元**来预测下一个词元，从而连续生成输出序列的词元
- 编码器将长度可变的输入序列转换成**形状固定的上下文变量**，并且将输入序列的信息在该上下文变量中进行编码
- 编码器可以是**单向**的循环神经网络，其中的隐藏状态只依赖于输入子序列，这个子序列是由**输入序列的开始位置到隐藏状态所在的时间步的位置（包括隐藏状态所在的时间步）**组成
- 编码器也可以是**双向**的循环神经网络，其中隐藏状态依赖于两个输入子序列，两个子序列是由**隐藏状态所在的时间步的位置之前的序列和之后的序列（包含隐藏状态所在的时间步）**，因此隐藏状态对整个序列的信息都进行了编码。双向不能做语言模型，但是双向可以做翻译；双向可以做编码器，但不能做解码器，解码器需要做预测，编码器不需要
- `<bos>` 表示**序列开始词元**，代表一个句子的开始，==它是解码器的输入序列的第一个词元==
- `<eos>` 表示**序列结束词元**，代表一个句子的结束（==解码器输出的句子长度是可以变化的，一旦输出序列生成此词元，模型就会停止预测==）
- RNN 做编码器可以输入**任意长度的序列**，最后返回最后时刻的隐藏状态，使用 RNN 编码器最终的隐状态来初始化解码器的隐状态，解码器一直输出，直到看到**句子的结束标志**为止
- seq2seq 也可以做**可变长度到可变长度**的句子之间的翻译

![](https://i0.hdslb.com/bfs/note/880c7560d257ebe2d13a1b4d1f4bcdb5a2cfd799.png)

隐藏状态是怎么传递的？

- 编码器是**没有输出的 RNN**
- 编码器**最后时间步的隐藏状态**作为解码器的初始隐藏状态

**训练**

![](https://i0.hdslb.com/bfs/note/93c3f0466afb8e032173a515001fe96e19f9c1f4.png)

- 训练时将**特定的开始词元（“`<bos>`”）和原始的输出序列（不包括序列结束词元“`<eos>`”）**拼接在一起作为解码器的输入，这也称为**强制教学**（teacher forcing，因为原始的输出序列（词元的标签）被送入了解码器）
- 也可以将**来自上一个时间步的预测得到的词元**作为解码器的当前输入
- 训练和推理是不同的:编码器是相同的，但是在训练的时候，解码器是知道目标句子的，它知道真正的翻译是什么样子的，所以解码器的输入（==每个 RNN 时刻的输出==）所使用的实际上是真正的目标句子的输入，所以就算是在训练的时候翻译错了，下一个时刻的输入还是正确的输入，也就是说，在训练的时候所使用的是真正的目标句子来帮助训练，这样就降低了预测长句子的难度

**推理**

![](https://i0.hdslb.com/bfs/note/5cf668617520408f94783a2c8a1a9c9649e35987.png)

- 推理的时候没有真正的目标句子作为参考，每一个时刻只能将**上一个时刻的输出**作为这一时刻的输入，以此来不断地进行预测，即每个解码器当前时间步的输入都来自前一个时间步的预测词元，因此能够一个词元接一个词元地预测输出序列
- 和训练类似，序列开始词元（“`<bos>`”）在初始时间步就被输入到了解码器中
- 当输出序列的预测遇到序列结束词元（“`<eos>`”）时，预测就结束了

**BLEU（bilingual evaluation understudy）**

- 可以通过与真实标签序列进行比较来评估预测序列
- BLEU 最先是用于评估机器翻译的结果，但是它已经被广泛用于测量许多应用的输出序列的质量
- ==原则上说，对于预测序列中的任意 n 元语法（n-grams），BLEU 的评估都是这个 n 元语法是否出现在标签序列中==
- BLEU 的值**越大越好**，最大值为 **1** ，越小的话效果越差

BLEU的定义

![](https://i0.hdslb.com/bfs/note/906f8f7245dd0a3ee5b5b1c123321ac5a2249c3d.png)

1、pn 表示 **n-grams 的精度**，它是两个数量的比值:第一个是**预测序列与标签序列中匹配的 n 元语法的数量**，第二个是**预测序列中 n 元语法的数量的比率**

![](https://i0.hdslb.com/bfs/note/c7f0d066da9b278464852f1753fbde5277e337ac.png)

- p1:考虑预测序列中所有的 1-gram，有 5 个 1-gram，即 A、B、B、C、D，所以分母为 5 ；再考虑这 5 个 1-gram ，是不是每一个 1-gram 都在标签序列中出现过（预测序列中除了第二个 B 并没有出现，因为在标签序列中 B 只出现了一次，其它都出现了，所以 p1 = 4/5）
- p2:考虑预测序列中所有的 2-gram，有 4 个 2-gram，即AB、BB、BC、CD，所以分母为 4 ；再考虑这 4 个 2-gram ，是不是每一个 2-gram 都在标签序列中出现过（预测序列中除了第二个 BB 并没有出现，因为在标签序列中 B 只出现了一次，其它都出现了，所以 p2 = 3/4）
- p3:考虑预测序列中所有的 3-gram，有 3 个 3-gram，即ABB、BBC、BCD，所以分母为 3 ；再考虑这 3 个 3-gram ，是不是每一个 3-gram 都在标签序列中出现过（预测序列中只有 BCD 在标签序列中出现了一次，其它都没有出现了，所以 p3 = 1/3）
- p4:考虑预测序列中所有的 4-gram，有 2 个 4-gram，即ABBC、BBCD，所以分母为 2 ；再考虑这 2 个 4-gram ，是不是每一个 4-gram 都在标签序列中出现过（预测序列中所有的 4-gram 都没有在标签序列中出现过，所以 p4 = 0/2）

2、len(label)、len(pred) 分别表示**标签序列中的词元数**和**预测序列中的词元数**，两个序列的词元数可以是不一样的

- 如果预测的长度比标签（真实）的长度少很多的话，len(label)/len(pred) 就会大于 1 ，整个指数项就会变成一个很小的数
- 所以说真实的标签很长，预测的长度很短的话，会导致前面的指数项比较小，因为预测的长度很短的话，就会越容易命中真实的标签，所以前半部分的指数项是为了**惩罚较短的预测序列，防止预测的长度过短**

3、pn 都是一个小于等于 1 的数，**当预测序列和标签序列完全相同时，BLEU 为 1**

- ==由于 n 元语法越长则匹配难度越大，所以 BLEU 为更长的 n 元语法的精确度分配了更大的权重==

## 束搜索（beam search）

### 贪心搜索（greedy search）

在 seq2seq 中使用了贪心搜索来预测序列（对于输出序列的每一时间步，将==当前时刻具有最高条件概率==的词元输出，作为下一个序列，一旦输出序列包含了“`<eos>`”或者达到最大长度，则输出完成）

但是贪心搜索的结果可能不是最优的，举例如下

![](https://i0.hdslb.com/bfs/note/d53e87c8df89d44ce4793d5c437cd24db8a2e176.png)

- 上图中将两次选择各部分的概率分别进行相乘，会发现虽然右边并没有选择最优，但是最终生成 sequence 的概率比贪心搜索所得到的概率更高（因为当前步所选择的最优的词从整个句子来看的话不一定是最优的，所以贪心搜索可能是==效率最高==的，但是==不一定是最优的==）

### 穷举搜索（exhaustive search）

如果目标是获取最优序列，可以考虑使用穷举搜索:穷举地列出==所有可能的输出序列==及其条件概率，然后计算输出条件概率最高的一个（遍历所有的序列，将所有的序列的概率计算出来，最后选出最好的那个序列一定是最优的，也就是将所有的序列都使用模型评估一下）

如果输出字典大小为 n ，序列最长为 T ，那么需要考察 n ^ T 个序列

- n = 10000 , T = 10 : n^T = 10^4
- 计算上不可行，现有的计算机几乎不可能计算它
- **贪心搜索是最快的，但不是最好的；穷举搜索是最好的，但是计算量巨大，计算起来比较困难**

**束搜索（beam search）**

- 如果只看重精度的话，显然穷举搜索算法是最优的；如果更注重计算成本的话，则贪心搜索算法是最优的；束搜索的实际应用介于这两个极端之间
- 在每个时刻，保存最好的 k 个候选序列，对每个候选新加一项（ n 种可能），在 kn 个选项中选出最好的 k 个

举例:

![](https://i0.hdslb.com/bfs/note/7675091f6e06abc529183fb5192c0a1f7337bbdc.png)

- 束搜索是贪心搜索的改进版本，它和贪心算法的区别在于，贪心算法每次都选择最好的一个词元来组成序列，而束搜索每次选择 k 个作为候选
- k 是超参数，称为**束宽（beam size）**，在时间步 1 ，算法所选择的是具有条件概率最高的 k 个词元，这 k 个词元将分别是 k 个候选输出序列的第一个词元；在随后的每个时间步，基于上一时间步的 k 个候选输出序列，将继续挑出具有最高条件概率的 k 个候选输出序列

时间复杂度: O(knT)

- 对于每一个时刻 T 都需要做 kn 次搜索
- k = 5 , n = 10000 , T = 10 : knT = 5 x 10^5
- 束搜索的计算量介于贪心搜索和穷举搜索之间
- 贪心搜索可以看作是一种**束宽为 1 的特殊类型的束搜索**
- ==通过灵活地选择束宽，束搜索可以在正确率和计算代价之间进行权衡==

每个候选的最终分数是

![](https://i0.hdslb.com/bfs/note/7d0055fa8f39a19d41bfffc4f1af034225cf83d7.png)

- 最后不仅能够得到 k 个选择，还能够将搜索过程中得到的子序列也保存下来，基于这些序列获得最终候选输出序列集合，然后按照上述公式挑选其中**条件概率乘积最高的序列**作为输出序列
- 在计算概率的时候会发现，长句子的概率会低于短句子的概率（因为是各部分概率的乘积，而概率的值都是小于 1 的，所以句子越长，概率就越低，所以一个句子的概率会低于它的子句的概率），所以这里引入了一个 **1 / (L ^ α)**，等价于提高最终长句子所得到的分数，否则模型会倾向于学习长度较短的句子
- **L** 是最终候选序列的长度
- 通常 **α** = 0.75

**总结**

1、序列搜索策略包括==贪心搜索、穷举搜索和束搜索==

- 贪心搜索所选取序列的**计算量最小**，但是**精度相对较低**
- 穷举搜索所选取序列的**精度最高**，但是**计算量最大**
- 束搜索通过**灵活选择带宽**，在**正确率**和**计算代价**之间进行权衡

2、束搜索在每次搜索时保存 k 个最好的候选

- k = 1 时是贪心搜索，每一步总是选择最好的词元作为候选
- ==束搜索的 k 一般取 5 或者 10 ，通常来说 k 的值选的越大，所选择的范围也就越大，但是可能最后实施的效果也就越差==（在做语音特别是实时翻译的时候，k 的取值不能太大，如果取值比较大的话，会大大降低模型的实时性）

## 注意力机制

![tutieshi_640x363_9s](https://article.biliimg.com/bfs/article/d4976d4c6f627cc2aed84d2428e8eaa69eba28e1.gif)

RNN模型改进了传统神经网络，建立了网络隐层间的时序关联，每一时刻的隐层状态`St`不仅取决于`Xt`还有上一时刻状态的`St-1 `

![tutieshi_640x347_13s](https://article.biliimg.com/bfs/article/6d551b32c7c028e7baead869babbf08dae9a695e.gif)

两个RNN结构的组合可以形成`Encoder-Decoder`模型，先对一句话编码，然后再解码，就能实现机器翻译，但是这种不管输入多长，都==统一压缩成相同长度编码**c**的做法==，会导致翻译精度下降。

![tutieshi_640x296_12s](https://article.biliimg.com/bfs/article/cdd316b11cfc04aaaef62b33c7f88820ccb7683a.gif)

attention机制就是通过每个时间输入不同的c来解决这个问题，其中的系数$\alpha  t_i$表明$t$时刻所有的输入的权重,以$c_{t}$的视角看过去在它眼中就是不同视角的**注意力**,因此被称为**attention分布**,网络结构定了,我们可以通过神经网络数据==训练==,得到最好的attention==权重矩阵==,通过attention机制的引入,我们打破了只能利用Encoder-Decoder形成单一向量的限制,让==每一时刻模型==都能动态地看到全局信息,将注意力集中到对当前单词翻译最重要的信息上。

### 注意力

从**心理学**来讲，动物需要在复杂环境下有效关注值得关注的点。

在心理学上有框架:

* 人会主动、有意识的去选择注意点。
* 注意点可以理解为值得关注的点、引人注意的点。

![](https://zh-v2.d2l.ai/_images/eye-coffee.svg)

>  由于突出性的非自主性提示（红杯子），注意力==不自主地==指向了咖啡杯

喝咖啡后，我们会变得兴奋并想读书,注意力在基于==自主性提示==去辅助选择时将更为谨慎

![](https://zh-v2.d2l.ai/_images/eye-book.svg)

> 依赖于任务的意志提示（想读一本书），注意力被自主引导到书上

卷积、全连接、池化层都只考虑不随意线索（**没有明确的目标）**

* 池化操作通常是将感受野范围中的**最大值**提取出来（最大池化）
* 卷积操作通常是对输入全部通过**卷积核**进行操作，然后提取出一些比较明显的特征 

#### 查询、键和值

首先，考虑一个相对简单的状况， 即只使用非自主性提示。 要想将选择偏向于感官输入， 则可以简单地使用参数化的全连接层， 甚至是非参数化的最大汇聚层或平均汇聚层。

因此，“**是否包含自主性提示**”将注意力机制与全连接层或汇聚层区别开来。 在注意力机制的背景下，自主性提示被称为***查询***（query）。 给定任何查询，注意力机制通过***注意力汇聚*（attention pooling）** 将选择引导至***感官输入***（sensory inputs，例如中间特征表示）。 在注意力机制中，这些感官输入被称为***值*（value**）。 更通俗的解释，每个值都与一个***键*（key）**配对， 这可以想象为感官输入的==非自主提示==。可以通过设计注意力汇聚的方式， 便于给定的查询（**自主性提示**）与键（**非自主性提示**）进行匹配， 这将引导得出最匹配的值（感官输入)

![](https://zh-v2.d2l.ai/_images/qkv.svg)

**非参注意力池化层**

![](https://i0.hdslb.com/bfs/note/4067b0d12325d28976feb866fde40e776cf9db2a.png)

* 非参:不需要学习参数
* x -- key
* y -- value
* f（x）-- 对应所要查询的东西
* （x，y） -- key-value对（候选）
* 平均池化:之所以是最简单的方案，是因为不需要管所查询的东西（也就是f（x）中的 x ），而只需要无脑地对 y 求和取平均就可以了 

Nadaraya-Watson 核回归:

* 核:K 函数，它可以认为是衡量 x 和 xi 之间距离的函数
* 数据就是给定的数据，对于新给定的值来讲，只需要在给定的数据中进行查询就可以了（选择和新给定的值比较相近的数据，然后将这些数据对应的 value 值然后进行加权求和，从而得到最终的 query），所以不需要学习参数

K 的选择:**高斯核**![](https://i0.hdslb.com/bfs/note/94165f1b73d54f9297eb3b0d92dc3c422441ea64.png)



* u:代表 x 和 xi 之间的距离
* exp:作用是将最终的结果变成大于 0 的数
* softmax:得到 0 到 1 之间的数作为权重
* 在上式的基础上添加一个可以学习的 w  

![](https://i0.hdslb.com/bfs/note/4cfeae3bebc991e8c9f04861eb0e6a7f2a053a03.png)

总结



1、心理学认为人通过随意线索和不随意线索选择注意点

2、注意力机制中，通过query（随意线索）和 key（不随意线索）来有偏向性地选择输入，一般可以写作

![](https://i0.hdslb.com/bfs/note/0526cb3ca315971cfdd40baef4e429a3f95a3971.png)

* f（x）的 key 和所有的不随意线索的 key 做距离上的计算（α（x，xi），通常称为注意力权重），分别作为所有的 value 的权重
* 这并不是一个新兴的概念，早在 60 年代就已经有非参数的注意力机制了

**注意力分数**

![](https://i0.hdslb.com/bfs/note/321b6ae3af9b90073dce2aecfe98a38e3222cb7e.png)

- **α（x,xi）**:注意力权重（权重一般是一组大于等于零，相加和为 1 的数）
- **注意力分数**:高斯核的指数部分（相当于是注意力权重归一化之前的版本）
- 上图所展示的是:假设已知一些 key-value 对和一个 query，首先将 query 和每一个 key 通过**注意力分数函数 a** 和 **softmax** 运算得到注意力权重**（与 key 对应的值的概率分布）**，将这些注意力权重再与已知的 value 进行**加权求和**，最终就得到了输出 

**如何将 key 和 value 拓展到更高的维度**

![](https://i0.hdslb.com/bfs/note/ccca5936e3d4857ca8203062e397bc7bca1f24c7.png)

- 假设 query 是一个长为 q 的向量，ki 是长为 k 的向量，vi 是长为 v 的向量（这里 key 和 value 的长度可以不同）
- 其中 query 和 ki 的注意力权重**（标量）**是通过注意力评分函数 a 将两个向量映射成标量，再经过 softmax 运算得到的 

**掩蔽 softmax 操作（masked softmax operation）**

softmax 操作用于输出一个概率分布作为注意力权重，但是在某些情况下，并非所有的值都应该被纳入到注意力汇聚中

- 在处理文本数据集的时候，为了提高计算效率，可能会采用**填充**的方式使每个文本序列具有相同的长度，便于以相同形状的小批量进行加载，因此可能会存在一些文本序列被填充了没有意义的特殊词源（比如“`<pad>`”**词元）
- 因此，==为了仅仅将有意义的词元作为值来获取注意力汇聚，可以指定一个有效序列长度（即词元的个数），任何超出有效长度的位置都被掩蔽并置于 0，便于在计算 softmax 的时候过滤掉超出指定范围的位置，这也就是掩蔽 softmax 操作==

### 使用注意力机制的 seq2seq

![](https://i0.hdslb.com/bfs/note/b4488444204d3384b3c13fc70b44180705f36a73.png)

- 在机器翻译的时候，每个生成的词可能相关于源句子中不同的词
- Seq2Seq 模型中不能对此直接建模。Seq2Seq 模型中编码器向解码器中传递的信息是**编码器最后时刻的隐藏状态**，解码器只用到了编码器最后时刻的隐藏状态作为初始化，从而进行预测，所以解码器看不到编码器**最后时刻的隐藏状态之前的其他隐藏状态**
- 源句子中的所有信息虽然都包含在这个隐藏状态中，但是要想在翻译某个词的时候，每个解码步骤使用编码相同的上下文变量，但是**并非所有输入（源）词元都对解码某个词元有用**。将注意力关注在源句子中的对应位置，这也是将注意力机制应用在Seq2Seq 模型中的动机

**加入注意力**

![](https://i0.hdslb.com/bfs/note/cf53c5ea54051216e1987ac2a718210a2c0d064e.png)

- **编码器对每次词的输出（隐藏状态）作为 key 和 value**，序列中有多少个词元，就有多少个 key-value 对，它们是**等价**的，都是第 i 个词元的 RNN 的输出
- **解码器 RNN 对上一个词的预测输出（隐藏状态）是 query**
- **注意力的输出和下一个词的词嵌入合并**进入 RNN 解码器 
- 对 Seq2Seq 的改进之处在于:之前 Seq2Seq 的 RNN 解码器的输入是 RNN 编码器最后时刻的隐藏状态，加入注意力机制之后的模型相当于是**对所有的词进行了==加权==平均，根据翻译的词的不同使用不同时刻的 RNN 编码器输出的隐藏状态**

**总结**

1、Seq2Seq 中通过编码器**最后时刻的隐藏状态**在编码器和解码器中传递信息

2、注意力机制可以根据解码器==RNN的输出==来匹配到合适的编码器RNN的输出来更有效地传递信息

3、在预测词元时，如果不是所有输入词元都是相关的，加入**注意力机制**能够使 RNN 编码器-解码器**有选择地统计输入序列的不同部分**（通过将上下文变量视为加性注意力池化的输出来实现）

### 自注意力

![tutieshi_640x311_11s](https://article.biliimg.com/bfs/article/c4ef11b9b6992f44904beb5526a1587b7e5cb59e.gif)

人们发现RNN的顺序结构很不方便,难以并行运算效率太低,因为attention机制对全部输入进行了打分,RNN的顺序没啥用了,简化成可以称为**self-attention机制**了,就是去掉之前输入的箭头,利用这些权重加权表示,再放到一个所谓的前馈神经网络中,得到新的表示,就可以很好的==嵌入上下文信息==,这样的步骤重复几次效果会更好

![](https://i0.hdslb.com/bfs/note/615258f4671f4542937d318369c87c7489123de9.png)

- 给定序列是一个长为 n 的序列，每个 xi 是一个长为 d 的向量
- 自注意力将 xi 同时作为 key 、value 和 query ，以此来对序列抽取特征
- 基本上可以认为给定一个序列，会对序列中的每一个元素进行输出，也就是说，**每个查询都会关注所有的键-值对并生成一个注意力输出**
- 自注意力之所以叫做自注意力，是因为 key，value，query 都是来自于自身，xi 既作为 key ，又作为 value ，同时还作为 query （self-attention 中的==self 所强调的是key,value,query 的取法==）

**和 CNN，RNN 对比**

![](https://i0.hdslb.com/bfs/note/27e427f8b952c20ec07621aea4baabda890ebe70.png)

- CNN、RNN、自注意力都可以用来处理序列
- CNN 如何处理序列:给定一个序列，将其看作是一个一维的输入（之前在处理图片时，图片具有高和宽，而且每个像素都具有 chanel 数，也就是特征数），如果用 CNN 做序列的话，经过一个 1d 的卷积（只有宽没有高）之后，将每个元素的特征看作是`channel`数，这样就可以用来处理文本序列了
- **k**:窗口大小，每次看到的长度为 k
- **n**:长度
- **d**:dimension，每个x的维度（长度）
- **并行度**:每个输出(yi)可以自己并行做运算，因为 GPU 有大量的并行单元，所以**并行度越高，计算的速度就越快**
- **最长路径**:对于最长的那个序列，前面时刻的信息通过神经元传递到后面时刻,对应于计算机视觉中的感受野的概念（每一个神经元的输出对应的图片中的视野）
- **卷积神经网络和自注意力都拥有并行计算的优势，而且自注意力的最大路径长度最短。但是因为自注意力的计算复杂度是关于序列长度的二次方，所以在很长的序列中计算会非常慢**

**位置编码（position encoding）**

1、和 CNN / RNN 不同，**自注意力并没有记录位置信息**

2、为了使用序列的顺序信息，通过在输入表示中添加**位置编码**将位置信息注入到输入里

![](https://i0.hdslb.com/bfs/note/3d084bccfaebd777926bbfe6260da9c40a17e082.png)

* 位置编码不是将位置信息加入到模型中，一旦位置信息加入到模型中，会出现各种问题

- P 中的每个元素根据对应的 X 中元素位置的不同而不同

![](https://i0.hdslb.com/bfs/note/046c320531e7a13eb63a52460c47a92ba1d1204d.png)

- 对于 P 中的每一列，奇数列是一个 **cos 函数**，偶数列是一个 **sin 函数**，不同的列之间的周期是不一样的

**位置编码矩阵**

![](https://i0.hdslb.com/bfs/note/bd304e94e338052b1e0a5fe77f9b885e57db750b.png)

**总结**

1、自注意力池化层将 xi当作key ，value query来对序列抽取特征

2、**完全并行**、**最长序列为 1** 、但**对长序列计算复杂度高**

- 可以完全并行，和 CNN 是一样的，所以计算效率比较高
- 最长序列为1 ，**对于任何一个输出都能够看到整个序列信息**，所以这也是为什么当处理的文本比较大、序列比较长的时候，通常会用注意力和自注意力
- 但是问题是对长序列的计算复杂度比较高，这也是一大痛点

3、位置编码在输入中加入位置信息，使得自注意力能够记忆位置信息

- 类似于计算机的数字编码，对每个样本，给定一个长为d的编码
- 编码使用的是**sin函数或者是cos函数**，使得它对于序列中两个固定距离的位置编码，不管它们处于序列中的哪个位置，他们的编码信息都能够通过一个线性变换进行转换

![image-20230301160541037](https://article.biliimg.com/bfs/article/8c0d5de4b8e60f54b0ac6e468f740930d4dd6372.png)

## transformer

- Transformer模型是**完全基于注意力机制**，没有任何卷积层或循环神经网络
- Transformer最初应用在**文本数据上的序列到序列学习**，现在已经推广到各种现代的深度学习中，如**语言**、**视觉**、**语音**和**强化学习**领域

### Transformer 架构

![](https://i0.hdslb.com/bfs/note/6e974c24b7bd3101f55a334c89e87c1813c29411.png)

基于编码器-解码器的架构来处理序列对，Transformer 的编码器和解码器是==基于自注意力的模块叠加==而成的，源（source，输入）序列和目标（target，输出）序列的嵌入（embedding）表示通过==加上位置编码（positional encoding）==加入位置信息，再分别输入到编码器和解码器中

1、Transformer 的编码器是由多个相同的层叠加而成的,其中的`Add& norm`代表**残差连接**和**层级池化**

第一个子层是**多头自注意力汇聚**

![](https://i0.hdslb.com/bfs/note/38bf1650fd06498e28e6f6c41dd109e7a8c295c3.png)

、对**同一个 key 、value 、query** 抽取不同的信息

- 例如短距离关系和长距离关系

2、多头注意力使用 h 个独立的注意力池化

- 合并各个头（head）输出得到最终输出

![](https://i0.hdslb.com/bfs/note/49477fbbbff28a90726637cf92fcbf2e7e190d98.png)

- key 、value 、query 都是长为 1 的向量，通过全连接层映射到一个低一点的维度，然后进入到注意力模块中

![](https://i0.hdslb.com/bfs/note/f006dacd8af3893977df988bb834b4cc44f742f4.png)

### 带掩码的多头注意力（Masked Multi-head attention）

![](https://i0.hdslb.com/bfs/note/e17f20984c1f7de4edf9f95e96778a5516b7d7cf.png)

1、解码器对序列中一个元素输出时，不应该考虑该元素之后的元素

- **注意力中是没有时间信息的**，在输出中间第 i 个信息的时候，也能够看到后面的所有信息，这在编码的时候是可以的，但是在**解码**的时候是不行的，在解码的时候不应该考虑该元素本身或者该元素之后的元素

2、可以通过掩码来实现

- 也就是计算 xi 输出时，假装当前序列长度为 i

![image-20230301162814077](https://article.biliimg.com/bfs/article/978572102526fa3296064bd3d9932cae178be09d.png)

### 基于位置的前馈网络（Positionwise FFN）

- 基于位置的前馈网络**对序列中的所有位置的表示进行变换时**使用的是**同一个多层感知机（MLP）**，这就是称前馈网络是基于位置的原因 

![](https://i0.hdslb.com/bfs/note/338dc107cbe4233586b6b8ce3ae3f921e699b7bd.png)

B:有多少个句子  N:句子中有多少个字  d:就是字的维度

其实就是全连接层，将输入形状由（b，n，d）变成（bn，d），然后作用两个全连接层，最后输出形状由（bn，d）变回（b，n，d），等价于两层核窗口为 1 的一维卷积层

- b:batchsize
- n:序列长度
- d:dimension
- 在做卷积的时候是将 n 和 d 合成一维，变成 nd ；但是现在 n 是序列的长度，==会变化==，要使模型能够处理任意的特征，所以不能将 n 作为一个特征，因此对每个序列中的每个元素作用一个全连接（将每个序列中的 xi当作是一个样本）

### 残差连接和归一化（Add & norm）

![](https://i0.hdslb.com/bfs/note/2460db99911f4a33828d27cc028abf5056d0a0f0.png)

1、Add 就是一个 Residual_block

![](https://i0.hdslb.com/bfs/note/4b917a487b499a957cb5666d07deb384063e47a4.png)

1、加入归一化能够更好地训练比较深的网络，但是这里不能使用批量归一化，**批量归一化对每个特征/通道里元素进行归一化**

- 在做 NLP 的时候，如果选择将 d 作为特征的话，那么批量归一化的输入是 n*b ，b 是批量大小，n 是序列长度，**序列的长度是会变的**，所以每次做批量归一化的输入大小都不同，所以会导致不稳定，训练和预测的长度本来就不一样，预测的长度会慢慢变长，所以批量归一化不适合长度会变的 NLP 应用

比如一行是一项数据，那么batch norm就是对一列进行归一化，就是所有数据项的某一列进行特征归一化

有b句话，每句话有len个单词，每个词由d个特征表示，BN是对所有句子所有词的某一特征做归一化，LN是对某一句话的所有词所有特征做归一化

2、**层归一化对每个样本里的元素进行归一化**

![](https://i0.hdslb.com/bfs/note/f2d25c3ebb7fa8af957606f2c8dce69a1e9d2019.png)

### 信息传递

![](https://i0.hdslb.com/bfs/note/ec1f09dda566e73a5fc053b09d217658ef09845c.png)

假设编码器中的输出是 y1，... ，yn ，将其作为解码中第 i 个 Transformer 块中多头注意力的**key**和**value**

- 一共有三个多头注意力（包括一个**带掩码的多头注意力**），位于带掩码的多头注意力与其它两个不同，其他两个都是自注意力（key 、value 和 query 都相同），而它是普通的注意力（它的 key 和 value 来自编码器的输出， query 来自目标序列），应该是**训练时**，decoder的第一个mask多头K,V来自Q，第二个attentionde的K,V来自encoder的输出；预测时，K,V就来自decoder的上一时刻的输出作为K,V。

**预测**

![](https://i0.hdslb.com/bfs/note/2ec22a2ff26e0cc5d8e855899e79f8c12a4fcbd1.png)

预测第 t+1 个输出时，解码器中输入前 t 个预测值

- 在自注意力中，前 t 个预测值作为 key 和 value ，第 t 个预测值还作为 query

关于序列到序列模型，在训练阶段，输出序列的所有位置（时间步）的词元都是已知的；但是在预测阶段，输出序列的次元是逐个生成的

- 在任何解码器时间步中，只有生成的词元才能用于解码器的自注意力计算中

**总结**

1、和 seq2seq 有点类似，不同之处在于  **Transformer 是一个纯使用注意力的编码-解码器**

2、编码器和解码器都有 n 个 Transformer 块

3、每个块里使用**多头（自）注意力（multi-head attention）**，**基于位置的前馈网络（Positionwise FFN）**，**残差连接**和**层归一化**

- 编码器和解码器中各有一个自注意力，但是在编码器和解码器中传递信息的是一个正常的注意力
- 基于位置的前馈网络使用同一个多层感知机，作用是对所有序列位置的表示进行转换，实际上就是一个全连接，**等价于 1\*1 的卷积**
- Add & norm:Add 实际上就是 Residual block 可以帮助将网络做的更深，norm 使用的是 Layer Norm 使得训练起来更加容易；Transformer 中的残差连接和层规范化是训练非常深度模型的重要工具

4、在 Transformer 中，多头注意力用于表示输入序列和输出序列，但是解码器必须通过**掩码机制**来保留**自回归属性**

## BERT

### 自我监督学习

Self-supervised Learning就是自己想办法去做Supervised Learning,具体操作就是把训练资料的一部分作为Model的**输入**,另外一部分作为Model的**label**，希望**输出与label**越接近越好

![](https://img-blog.csdnimg.cn/ba969aa974dd4e2ab7b01aefbc8bad90.png)



具体的怎么一部分划分为输入,一部分划分为标签有两种方法

2. Masking Input
   BERT的架构与Transformer Encoder是一样的。如下图所示，BERT的输入是一段文字，随机地把这段文字当中的某些词语做**下标记**（两种方法，第一种是设置一个`special token MASK`，另外一种是用**随机的词语**替代这个词语），然后把标记后的词语做Linear Transform（乘上一个矩阵），最后做softmax得到输出（Distribution）跟标记前的词语`minimize cross entropy`。

![](https://img-blog.csdnimg.cn/a233a306dfe94571ab09ff56c039f7f7.png)

2. Next Sentence Prediction

​	`SEP`是间隔符,`CLS`单独输出区别两个句子是否连接在一起。

![](https://img-blog.csdnimg.cn/dc8c32f3c44f40ab8de205ce15570c7c.png)

### How to use BERT

经过`Self-supervised Learning`的过程称为`Pre-train`,`Fine-tune`指的是在一个特定的任务上**微调**预训练模型以生成更加准确的输出。BERT真正做的任务叫做`Downstream Tasks`

![](https://img-blog.csdnimg.cn/6b555286f8a1420e996e3a9408c57349.png)

1. Case 1

   1）举例`Sentiment analysis`，输入是一个句子，输出是一个类别（正面 or 负面）。如下图所示，这里使用的是做过`pre-train`的BERT，好处在于这样会比随机初始化表现更好。

   ![](https://img-blog.csdnimg.cn/69c0525fc2174e5bb2e746e9d14cf9e5.png)

   CLS分类器是要随机初始化的,是要标签,而BERT是pre-train是自我监督的,合起来这个过程也叫做`Semi-supervised learning`(半监督)

 


如果卷积层的扩张率（dilation rate）为 $2$，那么输出张量大小的计算公式为:

$output\_height = \lfloor\frac{input\_height + 2 * padding\_height - (dilation\_rate * (kernel\_height - 1) + 1)}{stride\_height} + 1\rfloor$

$output\_width = \lfloor\frac{input\_width + 2 * padding\_width - (dilation\_rate * (kernel\_width - 1) + 1)}{stride\_width} + 1\rfloor$

其中， $\lfloor x \rfloor$ 表示对 $x$ 进行向下取整操作。

根据题目中的条件，输入张量大小为 $7\times7$，卷积核大小为 $3\times3$，步长为 $1$，扩张率为 $2$，不使用填充。将这些值带入公式中，则可以得到输出张量的大小:

$output\_height = \lfloor\frac{7 + 2 * 0 - (2 * (3 - 1) + 1)}{1} + 1\rfloor = 3$

$output\_width = \lfloor\frac{7 + 2 * 0 - (2 * (3 - 1) + 1)}{1} + 1\rfloor = 3$

因此，输出张量的大小是 $3\times3$。

`nn.ELU` 是 pyTorch 深度学习框架中的一个函数或类，用于构建深度神经网络（DNN）的层结构。其中 `nn` 是 pyTorch 中的模块，提供了实现深度学习网络所需的各种功能，包括卷积神经网络、循环神经网络、损失函数等。

`nn.ELU` 是一个激活函数，全称为 Exponential Linear Unit（指数线性单元），其定义为:

$$
ELU(x) = 
\begin{cases}
x, & \text{if}\ x > 0 \\
\alpha (e^{x} - 1), & \text{otherwise}
\end{cases}
$$

其中 $x$ 表示输入，$\alpha$ 是一个可调参数，在 pyTorch 中默认值为 1.0。 

和其它常见的激活函数（如 sigmoid、ReLU）相比，ELU 的主要优点是在 $x < 0$ 时具有负饱和度，这使得它在训练深度神经网络时具有更好的表达能力和鲁棒性，可以缓解梯度消失问题。

`nn.PReLU` 是 pyTorch 深度学习框架中的一个函数或类，用于构建深度神经网络（DNN）的层结构。其中 `nn` 是 pyTorch 中的模块，提供了实现深度学习网络所需的各种功能，包括卷积神经网络、循环神经网络、损失函数等。

`nn.PReLU` 是一个激活函数，全称为 Parametric Rectified Linear Unit（参数修正线性单元），其定义为:

$$
PReLU(x) =
\begin{cases}
x, & \text{if}\ x > 0 \\
\alpha x, & \text{otherwise}
\end{cases}
$$

其中 $x$ 表示输入，$\alpha$ 是一个可学习的参数，它可以通过反向传播算法在训练过程中自动确定。

和其它常见的激活函数（如 sigmoid、ReLU）相比，PReLU 具有更好的表达能力和鲁棒性，可以缓解梯度消失问题，并能够适应不同的数据分布。由于 $\alpha$ 是一个可学习的参数，因此 PReLU 可以自适应地调整激活函数的形状，从而更好地适应数据集的特点。

假设输入特征图的大小为 $W_1 \times H_1 \times D_1$，卷积核大小为 $K_w \times K_h \times K_d$，padding 的大小为 $P_w, P_h, P_d$，stride 大小为 $S_w, S_h, S_d$，扩展率（dilation）为 $D_w, D_h, D_d$，则输出特征图的大小为:

$$(W_2, H_2, D_2) = \left(\lfloor \frac{W_1 + 2P_w - D_w \times (K_w - 1) - 1}{S_w} \rfloor + 1, \lfloor \frac{H_1 + 2P_h - D_h \times (K_h - 1) - 1}{S_h} \rfloor + 1, \lfloor \frac{D_1 + 2P_d - D_d \times (K_d - 1) - 1}{S_d} \rfloor + 1\right)$$

其中 $\lfloor \cdot \rfloor$ 表示向下取整。这个公式适用于普通卷积、转置卷积等常见的卷积层。

默认情况下，padding 和 dilation 值为 0，stride 值为 1。在 PyTorch 中，可以通过在创建卷积层对象时指定相应参数来修改这些默认值，比如:

```python
import torch.nn as nn

conv_layer = nn.Conv2d(in_channels=3, out_channels=16, kernel_size=3, stride=2, padding=1, dilation=2)
```

上面的代码创建了一个输入通道数为 3，输出通道数为 16，卷积核大小为 $3 \times 3$，stride 为 2，padding 为 1，dilation 为 2 的 2D 卷积层。



