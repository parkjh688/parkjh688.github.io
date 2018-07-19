---
layout : post
title : "PyTorch 시작하기 - Neural Networks"
date : 2018-07-17 01:00:00 +0900
author : viking
category: PyTorch
---

Neural Networks 는 `torch.nn` 패키지를 이용해서 만들 수 있다.
torch.nn 패키지는 autograd 를 사용해서 모델을 정의하고 미분한다. `nn.Module` 은 레이어와 output 을 return 하는 forward(input) 메소드를 가지고 있다.

<br>아래 이미지는 알파벳을 분류하는 Convnet 신경망(Neural Net)이다.

<br>

![cnnImage](https://pytorch.org/tutorials/_images/mnist.png)

이 신경망은 단순한 feed-forward 네트워크이다. 데이터를 입력받고 여러 레이어에 차례대로 데이터를 feed 합니다. 그리고 결과 값을 줍니다.

일반적인 신경망의 순서는 다음과 같다.

- weight 를 갖는 신경망 정의
- 반복해서 데이터 입력
- 네트워크를 통해 입력 데이터 처리
- loss 계산(결과값 output 이 얼마나 맞고 틀렸는지)
- 네트워크 파라미터에 대한 역전파
- 네트워크 파라미터 업데이트

## PyTorch 에서의 네트워크 정의

<br>

```
import torch
import torch.nn as nn
import torch.nn.functional as F

class Net(nn.Module):

	def __init__(self):
		super(Net, self).__init__()
		# 입력 이미지 1개, 출력 6개, 커널크기 5 x 5
		# kernal
		self.conv1 = nn.Conv2d(1, 6, 5)
		self.conv2 = nn.Conv2d(6, 16, 5)
		# y = Wx + b
		self.fc1 = nn.Linear(16 * 5 * 5, 120)
		self.fc2 = nn.Linear(120, 84)
		self.fc3 = nn.Linear(84, 10)

	def forward(self, x):
        	# Max pooling : (2, 2) window
        	x = F.max_pool2d(F.relu(self.conv1(x)), (2, 2))
        	# window 사이즈가 정사각형이면, 하나의 숫자로 지정할 수 있음.
       		x = F.max_pool2d(F.relu(self.conv2(x)), 2)
        	x = x.view(-1, self.num_flat_features(x))
        	x = F.relu(self.fc1(x))
        	x = F.relu(self.fc2(x))
        	x = self.fc3(x)
        	return x

	def num_flat_features(self, x):
        	size = x.size()[1:]  # 배치 차원을 제외한 모든 차원
        	num_features = 1

        	for s in size:
            		num_features *= s
        	return num_features


net = Net()
print(net)
```

<br>
결과값은 다음과 같다.
<br>

```
Net(
  (conv1): Conv2d(1, 6, kernel_size=(5, 5), stride=(1, 1))
  (conv2): Conv2d(6, 16, kernel_size=(5, 5), stride=(1, 1))
  (fc1): Linear(in_features=400, out_features=120, bias=True)
  (fc2): Linear(in_features=120, out_features=84, bias=True)
  (fc3): Linear(in_features=84, out_features=10, bias=True)
)
```

<br>
네트워크를 정의할 때는 forward 함수만 정의하면된다. backward 함수는 autograd 를 통해 자동으로 정의되기 때문이다.

<br><br>

weight(or parameter) 는 net.parameters() 함수로 return 얻을 수 있다.

<br>

```
params = list(net.parameters())
print(len(params))
print(params[0].size())  # conv1's weight

# 결과
10
torch.Size([6, 1, 5, 5])
```

이 네트워크는 input size 가 32 X 32 인 네트워크 이기 때문에, 입력되는 데이터는 모두 사이즈가 32 X 32 이어야 한다. 임의의 32 X 32 사이즈 데이터를 네트워크에 넣어보자.

```
input = torch.randn(1, 1, 32, 32)
out = net(input)
print(out)

# 결과
tensor([[ 0.0890,  0.0227,  0.0010, -0.1081,  0.1768,  0.0867,  0.0313,
          0.1498,  0.0732,  0.1056]])
```

<br><br>

## Loss function

Loss function 은 (output, target) 형태로 데이터가 입력된다.

여러개의 다양한 Loss function 이 nn package 안에 정의되어 있다. 간단한 Loss function 인 Mean Squared Error 인 `nn.MSELoss`를 해보자.

```
output = net(input)
target = torch.arange(1, 11)  # a dummy target, for example
target = target.view(1, -1)  # make it the same shape as output
criterion = nn.MSELoss()

loss = criterion(output, target)
print(loss)

# 결과
tensor(38.7042)
```

<br>

`.grad_fn` 을 사용해서 반대 방향으로 loss 를 따라가면 다음과 같은 그래프를 볼 수 있다.

```
input -> conv2d -> relu -> maxpool2d -> conv2d -> relu -> maxpool2d
      -> view -> linear -> relu -> linear -> relu -> linear
      -> MSELoss
      -> loss
```

loss.backward() 를 실행할 때, 전체 그래프는 loss 에 대해 미분되며, 그래프 내의 requires_grad=True 인 모든 Tensor 는 Gradient가 누적된 .grad Tensor를 갖게 된다.

```
print(loss.grad_fn)  # MSELoss
print(loss.grad_fn.next_functions[0][0])  # Linear
print(loss.grad_fn.next_functions[0][0].next_functions[0][0])  # ReLU

# 결과
<MseLossBackward object at 0x7f12da7f6710>
<AddmmBackward object at 0x7f12da7f6128>
<ExpandBackward object at 0x7f12da7f6128>
```
