---
layout : post
title : "PyTorch 시작하기 - Tensor, AutoGrad"
date : 2018-07-16 01:00:00 +0900
author : viking
category: PyTorch
tag: ['Deep Learning', '"PyTorch']
---
<script type="text/javascript"
       src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default">
</script>

PyTorch 0.4.0 버전의 Tutorial Document 를 참고해서 쓰는 글입니다.

## PyTorch 설치

https://pytorch.org/ 에 가면 OS의 버전(Linux, macOS, Window), Package 의 종류(pip, conda, source), python 의 버전, CUDA 의 버전 혹은 유무로 알맞는 `pytorch 설치 commend` 를 얻을 수 있다.


나는 macOS에 conda 를 이용해서 설치했기 때문에 아래와 같았다.

`conda install pytorch torchvision -c pytorch`


pyTorch 를 설치했다면 Tensor 부터 시작해보자.
Tensor 는 딥러닝 프레임워크들에 있어서 약간 수학의 집합 같은 애다. 항상 Tensor 보고 matmul 이런거 보다가 아휴.. 내일해야지 하고 랩톱을 덮는다ㅋㅋㅋㅋㅋㅋㅋㅋ


## Tensor

PyTorch 에서 연산을 수행하기 위한 가장 기본적인 객체가 바로 Tensor 이다. 변수, 객체 이렇게 생각하면 된다.

Tensor 는 아래의 코드처럼 선언하면 된다. 간단한 설명을 하자면 `x` 는 특정 값을 준 Tensor 이고, `y` 는 사이즈(or shape)만 선언 해줘서 안에 랜덤 값이 알아서 들어간다.


```
import torch

x = torch.Tensor([[1, 2], [2, 1]])
>>> x
tensor([[ 1.,  2.],
        [ 2.,  1.]])

y = torch.rand(2, 2)
tensor([[ 0.3228,  0.8035],
        [ 0.3851,  0.2210]])
```

Tensor 에는 다양한 데이터 타입이 있다.데이터 타입은 [여기](https://pytorch.org/docs/stable/tensor_attributes.html) 를 참고하면된다.

Tensor 를 선언할 때 특정 데이터 타입을 명시하지 않으면, 디폴트 타입인 torch.FloatTensor로 선언된다.

<br><br>

## AutoGrad : Automatic Differentiation

자동으로 미분(Backpropagation)을 해주는 기능이다.
```
import torch

>>> x = torch.ones(2, 2, requires_grad=True)
>>> y = x + 2
>>> a = torch.ones(5, 5)
>>>
>>> print(x.grad_fn)
None
>>> print(y.grad_fn)
<AddBackward0 object at 0x106ef9da0>
>>>
>>> print(a.grad_fn)
None
>>> x.requires_grad
True
>>> y.requires_grad
True
>>> a.requires_grad
False
```
<br><br>

gradient가 필요한 Tensor는 requires_grad=True 로 설정 해야한다. 위의 예제에서 x, a 는 grad_fn 이 None 이고 y 는 None 이 아닌 값을 가지고 있는데, y 는 `y = x + 3` 연산을 통해 만들어진 Tensor 이기 때문이다.

<br><br>

## Gradient

그렇다면 gradient 를 구해보자.

```
# 위의 예제에 이어서

>>> z = y * y * 3
>>> out = z.mean()
>>>
>>>print(z, out)
>>> print(z, out)
tensor([[ 27.,  27.],
        [ 27.,  27.]]) tensor(27.)
>>>
>>> out.backward()
>>>
>>> print(x.grad)
tensor([[ 4.5000,  4.5000],
        [ 4.5000,  4.5000]])
```

<br>

실제로 x의 gradient 가 `tensor([[ 4.5000,  4.5000], [ 4.5000,  4.5000]])` 맞는지 계산을 해보자.

x 가 out 이라는 Tensor 에 준 영향을 Gradient 라고 할 수 있는데, 이걸 식으로 표현하면 $\frac{d(out)}{dx}$ 와 같다.

우리가 아는 것은 아래와 같다.

- $out = \frac{1}{4}\sum_iz_i$
- $z_i = 3(x_i + 2)^2$
- $z_i|_{x_i=1} = 27$


순서대로 설명을 하면
1) out 은 z.mean() 즉, z 의 평균을 구한 것과 같다
2) y = x+2 이고 z = y * y * 3 이기 때문에 y를 x+2 로 치환하면 z = (x+2)(x+2)3 으로 $z_i = 3(x_i + 2)^2$ 이다.
3) x 가 1일 때, z 는 27 이다.

위의 out, z 식을 이용하면

$\frac{d(out)}{dx_i} = \frac{3}{2}(x_i + 2)$ 가 되므로, $\frac{d(out)}{dx_i}|_{x_j=1} = \frac{9}{2}$ = 4.5 을 도출할 수 있다.


제대로 미분이 되고 있는게 맞다.

참고자료
- https://pytorch.org/tutorials/
- https://kh-kim.gitbook.io/natural-language-processing-with-pytorch/hello-pytorch/pytorch-tutorial
