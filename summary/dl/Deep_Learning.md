Deep Learning_study

> 有些知识在机器学习中已经学过了，这里只记录一些没有见到的知识

## Regularizing your neural network

### Dropout Regularizing your  neural network

> 众所周知，越小的神经网络越不容易发生过拟合，所以使用dropout方法，减少过程中的过拟合问题

- 设置keep概率，每一层可以不一样，一般多的设置小一点，大多情况为0.8

- 利用numpy的随机数设置一个节点的矩阵

- 通过和keep比较大小，使新矩阵相应的位置为true or false(python自动转换成0或1)

- 将新矩阵与原来的a相乘，留下的就是这次要训练的节点

- 将a除以keep-pro，为的是确保a的期望值不变

  ![image-20210814175136268](.\images\image-20210814175136268.png)

### Early Stopping

> 如题

### Normalizing inputs

![image-20210814175356563](.\images\image-20210814175356563.png)

- 好处是能让学习率放心的变大

![image-20210814175439016](.\images\image-20210814175439016.png)

### Vanishing/exploding gradients

> 当深度学习的神经网络中有许多层后，误差回随着层的传递会越来越接0或者变得很大

一个小的解决办法是手动初始化

参数![image-20210814180352679](.\images\image-20210814180352679.png)

> tip：注意这里是正态分布获得参数

其中 ReLu为2，tanh为1（具体数学原理还没看懂

## Optimization  Algorithms

### Exponentially weighted averages

原理

> 加权移动平均法，是对观察值分别给予不同的权数，按不同权数求得移动平均值，并以最后的移动平均值为基础，确定预测值的方法。采用加权移动平均法，是因为观察期的近期观察值对预测值有较大影响，它更能反映近期变化的趋势。
>
> 　　指数移动加权平均法，是指各数值的加权系数随时间呈指数式递减，越靠近当前时刻的数值加权系数就越大。
>
> 　　指数移动加权平均较传统的平均法来说，一是不需要保存过去所有的数值；二是计算量显著减小。

![image-20210815173318700](.\images\image-20210815173318700.png)

修正偏差：

![image-20210815174438500](.\images\image-20210815174438500.png)

### Gradient descent with momentum

通过指数加权平均，将近期的数据平均，减少了梯度下降的抖动

![image-20210815173631522](.\images\image-20210815173631522.png)

### RMSprop

通过除以均方根，达到修正地图下降的效果

![image-20210815173828626](.\images\image-20210815173828626.png)

### Adam optimization algorithm

![image-20210815173938057](.\images\image-20210815173938057.png)

建议的参数：

- alpha: needs to be tune
- beta1: 0.9
- beta2: 0.999
- sigma: 10^-8

### Learning rate decay

![image-20210815174140112](.\images\image-20210815174140112.png)

还有很多的衰减可以尝试

## Hyperparameter tuning

参数优先图：

![image-20210815230049084](.\images\image-20210815230049084.png)

### Using an appropriate scale to pick hyperparameters

- 正确的随机选取应该是根据参数的数量级来进行分配

![image-20210815231310906](.\images\image-20210815231310906.png)

![image-20210815231326786](.\images\image-20210815231326786.png)

### Fitting Batch Norm into a neural network

- Why the Batch Norm

![image-20210815231555436](.\images\image-20210815231555436.png)

- what the Batch Norm

![image-20210815231647160](.\images\image-20210815231647160.png)

- 在神经网络中，通过引入BN可以加快学习速率，得到一点正则化的作用，增强神经层之间的独立性

### Softmax regression

> 将最后的结果转换为概率的样子来表示输出

## Learning from multiple task

### Transfer learning

- 先确定要训练的数据类型，再找到相关的数据进行预训练，并在预训练好的参数上进行目标数据的训练。
- 当目标数据集大小不够，可以利用其他数据集的一些共同特点先完成底层数据的训练
- 这种训练方式是串行的，一般是前者的数据集远大于后者

![image-20210818220333805](.\images\image-20210818220333805.png)

### Multi-task

- 并行训练

![image-20210818220442823](.\images\image-20210818220442823.png)

## End-to-end deep learning

pros:

- 充分挖掘数据的信息
- 方便，减少了中间组件的设计

cons：

- 需要大量的数据
- 排除了一些有用的组件

![image-20210818220814571](.\images\image-20210818220814571.png)

## Convolutional Neural Networks

### Kernal

> 对于核的理解就是一个过滤器，每次进行卷积时，会进行一次特征提取，比如第一层是一些线条的检测，第二层则成为了一些局部特征的识别

![image-20210820201634798](.\images\image-20210820201634798.png)	

### Padding

类似于html中的那个padding，给数据加一层边框

- Valid：意思为0，不进行padding出来
- Same：意思为卷积后的大小和输入的大小相同，公式为`(f-1)/2`(步长为1的时候，其中f为核的大小)

### Strided convolutions

核滑动的步长

![image-20210820202143058](.\images\image-20210820202143058.png)

### Convolutions over volumes

核的第三维和输入的信道数是一样的，最后又变成了一个二位的面，但核的数量又使输出变成三维的

![image-20210820202442501](.\images\image-20210820202442501.png)

### Pooling layers

池化层提取了一个范围内的特征，增强了神经网络的鲁棒性

常见有

- MaxPooling
- AveragePooling

### Case Studies

#### LetNet-5

![image-20210820221511536](.\images\image-20210820221511536.png)

#### AlexNet

![image-20210820221526841](.\images\image-20210820221526841.png)

#### VGG-16

![image-20210820221609309](.\images\image-20210820221609309.png)

#### ResNet

![image-20210820221644619](.\images\image-20210820221644619.png)

![image-20210820221702123](.\images\image-20210820221702123.png)

### Inception network motivation

![image-20210820221838081](.\images\image-20210820221838081.png)

![image-20210820221814609](.\images\image-20210820221814609.png)

## Recurrent Neural Networks

### Notation

感觉重要的理解什么是sequence，个人的理解是乱序无意，特征需要上下文才能提取出来的数据

### Recurrent Neural Network Model

![image-20210823162856961](.\images\image-20210823162856961.png)

### backpropagation and loss function

![image-20210823163047044](.\images\image-20210823163047044.png)

### Different types of RNNs

![image-20210823163133265](.\images\image-20210823163133265.png)

### Language model

> 建议带着疑惑看到后面的束搜索

- 每个单元输出的是啥？
  - 在特定输入下的不同单词的概率
- 评判的标准是啥
  - 一整个句子的概率
- 越长岂不是越亏？
  - 会有部分归一化来拯救

### Gated Recurrent Unit (GRU) and LSTM

理解：类似有一个记忆门，弥补了句子中，因为插入而使上下文的关系变得不紧密。通过记忆门，跳过中间的插入，重新连接上下文

![image-20210823164654984](.\images\image-20210823164654984.png)

![image-20210823164716185](.\images\image-20210823164716185.png)

LSTM理解类似，不过改变了一些东东

![image-20210823164857882](.\images\image-20210823164857882.png)

![image-20210823164915237](.\images\image-20210823164915237.png)

### Bidirectional RNN

注意：必须是完整地输入才能使用，且双向的权重不一样

![image-20210823165025257](.\images\image-20210823165025257.png)

### Deep RNNs

![image-20210823165151095](.\images\image-20210823165151095.png)

## NLP and Word Embeddings

（欸嘿嘿 这里的数据可以网上拿

最开始的单词表示都使用one-hot形式，维数大，且不能表示单词之间的练习

### Word2Vec

实现了再回来写

#### Skip-grams

#### Negative sampling

#### GloVe word vectors

## Sequence to sequence models

![image-20210823213634339](.\images\image-20210823213634339.png)

![image-20210823213705781](.\images\image-20210823213705781.png)

### Beam search

在每一个RNN单元后，选择最有可能的c_num个短句



![image-20210823214231909](.\images\image-20210823214231909.png)

![image-20210823214347603](C:\Users\86153\AppData\Roaming\Typora\typora-user-images\image-20210823214347603.png)

### Bleu score

为了检测哪种翻译结果更好而产生的指标

![image-20210823214534321](.\images\image-20210823214534321.png)

![image-20210823214551194](.\images\image-20210823214551194.png)

### Attention model  intuition

Ng的解释是，在翻译一个比较长的句子时，网络会因为句子过长而导致效果下降，加入注意力机制能使网络更注意局部的特点

![image-20210823215935120](.\images\image-20210823215935120.png)

![image-20210823215948124](.\images\image-20210823215948124.png)

