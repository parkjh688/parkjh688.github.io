---
layout : post
title : "Average Pooling 과 Max Pooling 의 차이점"
date : 2018-07-20 01:00:00 +0900
author : viking
category : 데이터 사이언스 인터뷰 질문집 답변
---

오늘 질문은 `Average Pooling 과 Max Pooling 의 차이점` 이다.

<br>

Pooling 이라는 것은 CNN 을 할 때 많이 봤는데, CNN 에는 `Convolutional layer` 와 `Pooling layer` 가 있다.

<br><br>

질문에 대한 답을 하기 전에 각각의 layer 가 무슨 일을 할까?

## Convolutional layer

Conv layer 가 하는 일은 filter(or Kernel)를 데이터 위를 지나가면서, 입력 데이터와 합성곱을 하여 Feature Map 을 만든다.

내가 CNN 을 처음 공부할 때 저 말이 잘 이해가 가지 않았다. 어떤 블로그를 하나 봤는데 바로 와닿았다. 그 블로그에 나온 글을 짧게 번역해보겠다. 원문은 [여기](https://adeshpande3.github.io/adeshpande3.github.io/A-Beginner's-Guide-To-Understanding-Convolutional-Neural-Networks/) 있다.

<br>
<br>

아래에 왼쪽 그림은 filter 를 나타낸 행렬이다. 0이 아닌 값들이 있는 부분들을 색칠해 보면 오른쪽에 있는 선과 같은 모양이다.

![filter.png](https://dl.dropbox.com/s/6ud8piirpr8xkxc/Filter.png?)

<br>
<br>


위에서 본 filter 는 쥐 이미지의 엉덩이 부분(노랑색 네모 부분을)과 비슷한 값을 가지고 있는 행렬이다.

![OriginalAndFilter.png](https://dl.dropbox.com/s/dxpj9kuf6b54n7l/OriginalAndFilter.png?)

<br>

아래의 쥐의 엉덩이 필터 모양(오른쪽 이미지)과 실제 사진의 쥐 엉덩이 부분(왼쪽 이미지)을 같은 위치에 있는 값끼리 곱해보면, 6600 이라는 큰 숫자가 나온다.

![FirstPixelMulitiplication.png](https://dl.dropbox.com/s/zyqawr2nknnmuxx/FirstPixelMulitiplication.png?)

<br>

이번에는 쥐 얼굴 부분(왼쪽 이미지)과 쥐 엉덩이 부분을 곱했을 때는 겹치는 부분이 없으므로 0이라는 숫자가 나온다.

![SecondMultiplication.png](https://dl.dropbox.com/s/ooiy8cqmqys2xu3/SecondMultiplication.png?)

위의 예를 통해서 내가 원하는 이미지의 shape 을 가진 filter 가 있으면 이미지를 구분해낼 수 있다는 것을 알 수 있다. 이것이 바로 Convolutional Layer 가 하는 일이다.

<br><br>

## Pooling layer

Pooling layer 가 하는 일은 Convolutional layer 를 거친 데이터로부터 뽑아내는 것인데, 파라미터의 개수 혹은 연산량을 줄이기 위해서 뽑아내는 것이다.

max Pooling, average pooling, L2-norm pooling 등이 있다.


<br><br>
CNN 포스팅을 나중에 한 번 하기로 하고, 여기서 마무리하겠다.

그럼 본론으로 돌아가서 `Average Pooling 과 Max Pooling 의 차이점` 은 무엇일까?


아래 이미지는 Quora 에서 가져왔다. 그 차이점에 대해서도 잘 쓰여 있어서 Quora 답변을 참고해서 설명을 하겠다.

<br>

![quora_pooling.png](https://dl.dropbox.com/s/5as3s2cocmuj0it/pooling.png?)

<br>

이미지를 보면 pooling size 가 2 X 2, stride 가 2 이다.
max pooling 은 stride 를 하면서 window 안에서 가장 큰 값을 뽑는 것이고, average pooling 은 평균 값을 뽑는 것이다.

<br>

max pooling 을 한 이미지와 average pooling 을 한 이미지를 비교해보면 max pooling 은 차이가 뚜렷해보이고, average pooling 은 전체적으로 회색이면서 비슷하다.


그렇기 때문에 이미지의 edge 와 같은 가장 중요한 feature 를 뽑을 때 주로 max pooling 이 사용된다.
