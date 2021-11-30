# Pytorch_study

## 1 介绍

## 2 前置知识

### 2.1 Numpy Torch对比

```python
import torch
import numpy as np

# 相互转化
# np_data = np.arange(6).reshape((2,3))

# torch_data = torch.from_numpy(np_data)

# tensor2array = torch_data.numpy()

# print(
# 	'\nnumpy',np_data,
# 	'\ntorch',torch_data,
# 	tensor2array
# 	)


# abs
# data = [-1,-2,1,3]
# tensor = torch.FloatTensor(data)
# print(
# 	'\nsin',
# 	'\nnumpu',np.mean(data),
# 	'ntorch',torch.mean(tensor)
# 	)

data = [[1,2],[3,4]]
tensor = torch.FloatTensor(data)

print(
	np.matmul(data,data),
	torch.mm(tensor,tensor)
	)

```

### 2.2 Variable

> 神经网络中的参数都是Variable变量的类型 用于储存tensor的数据信息

```python
import torch
from torch.autograd import Variable

tensor = torch.FloatTensor([[1,2],[3,4]])
Variable = Variable(tensor,requires_grad=True)
# requires_grad用来表示是否需要计算每个节点的梯度方便计算反向传播


t_out = torch.mean(tensor*tensor)
v_out = torch.mean(variable*variable)
v_out.backward()
print(variable.grad) #这里的.grad是variable的经历过计算后的总式子的偏导
print(variable.data.numpy()) #转换成numpy需要.data
```

现在Variable和Tensor已经整合`tensor.requires_grad=True`之后便一样了

### 2.3 激励函数

```python
import torch
import torch.nn.functional as F
import matplotlib.pyplot as plt 
#解决mkl的冲突
import os
os.environ["KMP_DUPLICATE_LIB_OK"]  =  "TRUE"
#dummy data
x = torch.linspace(-5,5,200)  # x data (tensor),shape=(200,1)  平均分化
x_np = x.numpy()



#激励函数
y_relu = torch.relu(x).numpy()
y_sigmoid = torch.sigmoid(x).numpy()
y_tanh = torch.tanh(x).numpy()
y_softplus = F.softplus(x).numpy()

#画图
#初始化一个picture num为序号   size为宽高
plt.figure(2,figsize=(8,6))
#将网格分为几行几列占第几个(从左到右s形) 221=2，2，1
plt.subplot(221)
#x轴 y轴  线的颜色 标签
plt.plot(x_np,y_relu,c = "red",label="relu")
# 线的y的取值范围
plt.ylim((-1,5))
# 将标记放在最好的位置
plt.legend(loc='best')

plt.subplot(222)
plt.plot(x_np,y_sigmoid,c = "red",label="sigmoid")
plt.ylim((-0.2,1.2))
plt.legend(loc='best')

plt.subplot(223)
plt.plot(x_np,y_tanh,c = "red",label="tanh")
plt.ylim((-1.2,1.2))
plt.legend(loc='best')

plt.subplot(224)
plt.plot(x_np,y_softplus,c = "red",label="softplus")
plt.ylim((-0.2,6))
plt.legend(loc='best')

plt.show()
```

![image-20210807211250454](.\images\image-20210807211250454.png)

## 3 传统方法训练

### 3.1 Regression

> pytorch中的各种参数层（Linear、Conv2d、BatchNorm等）在`__init__`方法中定义后，不需要手动初始化就可以直接使用，这是因为Pytorch对这些层都会进行默认初始化

```python
import torch
import matplotlib.pyplot as plt 
import os
os.environ["KMP_DUPLICATE_LIB_OK"]  =  "TRUE"
# x data (tensor), shape=(100,1)
x = torch.unsqueeze(torch.linspace(-1,1,100),dim=1)
# noisy y data (tensor), shape(100,1)
# rand() - [0,1] shape(100,1)
y = x.pow(2) + 0.2 * torch.rand(x.size())
# plt.scatter(x.numpy(),y.numpy())
# plt.show()

class Net(torch.nn.Module):
	def __init__(self,n_feature,n_hidden,n_output):
		super(Net,self).__init__()
		# torch.nn.Linear(in_features, out_features, bias=True, device=None, dtype=None)
		# 对传入数据进行线性转换
		self.hidden = torch.nn.Linear(n_feature,n_hidden)
		self.predict = torch.nn.Linear(n_hidden,n_output)



	def forward(self,x):
		x = torch.relu(self.hidden(x))
		x = self.predict(x)
		return x

net = Net(1,10,1)
print(net)

#设置一个实时打印
#plt.ion() Enable the interact model
#The interactive mode is mainly useful if you build plots from the command line and want to see the effect of each command while you are building the figure.
plt.ion()
plt.show()

#优化器 优化参数  lr为学习效率
optimizer = torch.optim.SGD(net.parameters(),lr = 0.2)

# 一个损失函数 均方差
loss_func = torch.nn.MSELoss()

for t in range(200):
	prediction =  net(x)

	loss = loss_func(prediction,y)
	#Sets the gradients of all optimized torch.Tensor s to zero.
	#弹幕上说 pytorch计算梯度不是覆盖，而是与上一次的求和，这是为了方便RNN的计算,所以手动清零
	optimizer.zero_grad()
	#Computes the gradient of current tensor w.r.t. graph leaves.
	loss.backward()
	# Performs a single optimization step.
	optimizer.step()

	if t % 5 == 0:
		# plot and show learning process
        
        
        #Clear the current axes.人话就是清除子图（图中拟合的线）
		plt.cla()
		plt.scatter(x.numpy(),y.numpy())
		plt.plot(x.detach().numpy(),prediction.detach().numpy(),'r-',lw=5)
        
        # 带有梯度的tensor不能直接放入plt.text 需要中间有datach()转换一下 
		plt.text(0.5,0,'Loss=%.4f' % loss.detach().numpy(),fontdict={'size':10,'color':'red'})
		plt.pause(0.1)

##plt.ioff() disable the interact model
plt.ioff()
plt.show()
```

