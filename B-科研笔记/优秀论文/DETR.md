object query:限定要出多少框

1. 用卷积神经网络抽取特征
2. 用transformer-Encoder去学全局特征，帮助后面做检测
3. 用transformer-Decoder生成很多的预测框
4. 将预测框和gt做bipartite matching loss 匹配，在匹配里面找到两个做合适的 


我有一个npy文件"out/npy/image_2.npy",这个npy值0代表背景,不同的实例代表不同非0值,将这些不同非0值,绘画成不同的颜色进行展示,写出这样的一段代码

```python
import numpy as np
import matplotlib.pyplot as plt

# Load the npy file
npy_file = np.load("out/npy/image_2.npy")

# Get unique non-zero values in the npy file
unique_values = np.unique(npy_file[np.nonzero(npy_file)])

# Generate random colors for each unique value
colors = np.random.rand(len(unique_values), 3)

# Create a color map dictionary for each unique value
color_map = {value: color for value, color in zip(unique_values, colors)}

# Create a new image with colored instances
colored_image = np.zeros((npy_file.shape[0], npy_file.shape[1], 3))
for i in range(npy_file.shape[0]):
    for j in range(npy_file.shape[1]):
        if npy_file[i, j] != 0:
            colored_image[i, j] = color_map[npy_file[i, j]]

# Display the colored image
plt.imshow(colored_image)
plt.axis('off')
plt.show()
```



|  result_AJI           |  result_Dice         |
|:---------------------:|:--------------------:|
|   0.3964528534051754  |  0.7026832623892499  |
|   0.4687724160771722  |  0.7669577377827127  |
|   0.4099091350842457  |  0.7230529469425188  |
|  0.4640834619372225   |  0.7518354444450819  |
|  0.42350500004616204  |  0.6905903087194695  |
|  0.49164007437688345  |   0.788299839824413  |
|  0.46776690441185287  |  0.7384793369265271  |
|   0.46502023753554433 |   0.7757512477063973 |
|   0.46666978063575587 |   0.7705631143138478 |
|    0.4621684932623437 |   0.7714448712613994 |  




