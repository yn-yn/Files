


> Written with [StackEdit中文版](https://stackedit.cn/).

> cited from [基于卷积神经网络的壁面清洗机器人障碍物检测识别算法](https://kns.cnki.net/kcms/detail/detail.aspx?dbcode=CJFD&dbname=CJFDAUTO&filename=GSGY202204013&uniplatform=NZKPT&v=2I5ByEu1-nSYGcOJdnm65RJfiHvwK_dNVCZbwBh9N0Gfg18CM4NbLOp816OHQ9b9)

# 卷积神经网络（CNN）
卷积神经网络（CNN）是一类特殊的人工神经网络，主要包括：

 1. 输入层
 2. 卷积层
 3. 池化层
 4. 全连接层
 5. 输出层

首先通过卷积、池化以及非线性激活函数映射等操作，从原始数据输入层提取出高层语义信息。然后利用最后一层的运算将目标任务变换为目标函数，接着采用反向传播算法，通过最后一层向前反馈误差或损失，从而更新网络各层参数，多次反馈，重复操作直至找到最优解使模型收敛。

在本文模型中，卷积层及池化层均使用 ReLU 函数，ReLU 函数能够实现较高效率的梯度下降以及反向传播，主要以监督的方式训练网络，不需要依赖无监督的逐层预训练，从而避免了梯度爆炸和梯度消失这两个问题，ReLU 函数的数学表达为：
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/14/lL3u02b6Wn7y9yOk.png)

## 学习率 (Learning rate)
学习率 (Learning rate) 作为监督学习以及深度学习中重要的超参，其决定着目标函数能否收敛到局部最小值以及何时收敛到最小值。
文章常通过梯度下降法迭代求解，从而得到损失函数的最小值以及模型的参数值，假设第τ 次迭代于某个位置ｗτ，以下式更新权重：
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/14/iEVjiVGkYzClAUhW.png)
文章比较了四种不同学习率对训练过程的影响，如下图所示，网络在学习率为10^-3^ 时收敛速度最快也最稳定，因此本文选择10^-3^来训练。
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/14/SAb8GpRN23KkQq76.png)
*不同学习率对训练的影响*

# 卷积神经网络的训练
1. 构建数据集
2. 图像预处理
3. 训练算法
	- 前向传播：卷积层 & 池化层 `为了将输入成倍缩小，不需要训练参数`
	- 反向传播：需要对卷积层、池化层以及激活函数求导

# 实验结果
VGG-16 与 LeCun对比：
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/14/t6TD9EMZV0NCCNL1.png)
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/14/2GDjuM7qgv2HFU2X.png)

另外，文章将常规方法与卷积神经网络的性能进行了比较，使用同样的样本集分别在 BP 神经网络和支持向量机（support vector machine, SVM）上识别，结果如下表所列。通过基于 BP 神经网络的不变矩方法识别障碍物时，三种障碍物的识别效果较低，尤其对玻璃固定架的识别，仅有72.5% ；通过小波矩方法识别障碍物时，由于 SVM 是一种利用统计学习理论的机器学习方法，能较大程度地克服机器学习中的“过学习”问题，因此将小波矩与SVM结合识别障碍物能得到较高的识别效率，达到90% 以上。通过以上比较发现，简化的 VGG-16 络训练后得到三种障碍物的平均识别率达99.0% ，更适合于壁面清洗机器人对障碍物的识别。
![](https://raw.githubusercontent.com/yn-yn/image1/master/2022/09/14/jnuuXCDXH8WOKYUY.png)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMwODg0NTEwMF19
-->