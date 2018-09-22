---
layout : post
title : "Entropy, Cross-Entropy, KL Divergence"
date : 2018-09-19 01:00:00 +0900
author : viking
category: Deep Learning
---
<script type="text/javascript"
       src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default">
</script>

> 이 글은 머신러닝, 딥러닝에서 사용하는 KL Divergence, Cross-Entropy 를 이해하기 위해, 정보이론에서의 정보와 entropy 가 무엇인지에 대해 공부한 것에 대한 리뷰입니다.

<br></br>
<br></br>

## 정보(Information)

정보 이론(Information theory)에서 정보(information)는 **놀람의 정도** 로 일어날 확률이 높을 수록 정보량이 작다고 표현합니다. 아래의 수식으로 정보를 정의합니다.

<br></br>

$I(x) = -logP(x)$

<br></br>

* $P(x)$ : 사건 x 에 대한 확률, $0 \le P(x) \le 1$
* $I(x)$ : $-logP(x)$, $0 \lt P(x) \le 1$


![figure1](https://dl.dropbox.com/s/onugj0qsc9jf0ho/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202018-09-19%20%EC%98%A4%ED%9B%84%2012.39.11.png?)
*figure1 : $-log(x)$ 그래프*

<br></br>

$-log(x)$ 그래프는 figure1 처럼 생겼기 때문에, l(x) 는 $0 \lt P(x) \le 1$ 의 값을 얻는다. 따라서 $P(x)$ 의 확률이 클 수록 정보량이 작기 때문에 일어날 확률이 높을 수록 정보량이 작다는 것을 표현할 수 있습니다.

<br></br>
<br></br>

## 엔트로피 (Entropy)

위 식에서 이야기한 정보 수식에 $log$ 의 밑이 2 인 경우, 정보량의 단위를 섀년(shannon) 또는 비트(bit) 라고 하고 자연상수를 밑으로 할 경우 내트(nat)라고 부릅니다. 머신러닝에서는 자연상수를 밑으로 많이 사용합니다.

**섀넌 엔트로피(Shannon entropy)** 는 모든 사건 정보량의 기대값을 뜻합니다. 전체 사건의 확률분포의 불확실성의 양을 나타낼 때 씁니다. 서로 다른 종류의 레코드들이 섞여 있으면 엔트로피가 높고, 같은 종류의 레코드들이 섞여 있으면 엔트로피가 낮습니다. 무슨 뜻인지는 아래의 예를 보면 이해가 더 쉬울 것입니다.

<br></br>

$H[x] = E_{X{\sim}P}[I(x)] = -E_{X{\sim}P}[logP(x)]$
*figure2 : 섀넌 엔트로피(Shannon entropy)*

<br></br>

x 가 0 또는 1 이 나올 수 있다고 가정했을 때를 아래의 (1), (2), (3) 확률에 대한 엔트로피를 계산해 보겠습니다.

(1) $P(X=0) = 0.5, P(X=1) = 0.5$

$H[X] = -[p(X=0)logp(X=0) + p(X=1)logp(X=1)]
=  -[0.5log0.5 + 0.5log0.5] = -[0.5]$

(2) $P(X=0) = 0.7, P(X=1) = 0.3$

$H[X] = -[p(X=0)logp(X=0) + p(X=1)logp(X=1)]
=  -[0.7log0.7 + 0.3log0.3] = -[0.5]$

(3) $P(X=0) = 1 P(X=1) = 0$

$H[X] = -[p(X=0)logp(X=0) + p(X=1)logp(X=1)]
=  -[1log1 + 0log0] = -[0.5]$

<br></br>

확률이 균등하게 퍼져있는(서로 다른 레코드들이 섞여있는) (1) 번의 경우가 엔트로피가 가장 높고, 한 가지 확률에 쏠려있는 (3) 번의 경우가 엔트로피가 가장 낮습니다.

<br></br>
<br></br>

## KL Divergence

KL Divergence(Kullback-Leibler Divergence)는 두 확률분포의 차이를 계산하는데 사용합니다. VAE 도 loss function 의 일부로 KL Divergence 를 사용합니다. 보통은 <U>현재 가지고 있는 데이터분포 $P(x)$ 와 모델이 추정한 데이터분포 $Q(x)$ 의 차이를 계산해서 모델이 원래 데이터분포와 얼마나 비슷한 분포를 만들었나를 계산하기 위해 사용</U>합니다.

KL Divergence 는 세가지 성질이 있습니다.

* $D_{KL}(P||Q) = 0$ 이면, $P(x) = Q(x)$ 입니다. 즉, KL Divergence 값이 0 일 때 두 개의 확률 분포가 같다는 뜻입니다.
* $D_{KL}(P||Q) \neq D_{KL}(Q||P)$. 즉, KL Divergence 는 Non-Symetric 하다는 뜻입니다.
*  $D_{KL}(P||Q) \ge 0$

KL Divergence 의 식은 figure 3 과 같습니다.

<br></br>

$D_{KL}(P||Q) = E_{X{\sim}P}[log\frac{P(x)}{Q(x)}] = E_{X{\sim}P}[logP(x)-logQ(x)]$
*figure3 : KL Divergence*

<br></br>
<br></br>

## Cross-Entropy

Cross-Entropy 는 loss function 으로서 딥러닝에서 많이 사용됩니다. KL Divergence 와 Cross-Entropy 사이에는 figure4 와 같은 관계가 있습니다.

<br></br>

$H(P,Q) = H(P) + D_{KL}(P||Q)$
*figure4 : KL Divergence 와 Cross-Entropy 사이의 관계*

<br></br>

딥러닝 모델을 학습할 때, Cross-Entropy 가 최소화되도록 학습을 합니다. 그 때, $Q$ 에 Cross-Entropy 가 최소화 된다는 것은 figure4 에서 보이는 $ D_{KL}(P||Q)$ 를 최소화 하는 것과 같습니다. 왜냐하면 엔트로피인 $H(P)$ 는 학습과정에서 바뀌는 것이 아니기 때문입니다.

따라서 Cross-Entropy 는 아래의 figure5 와 같습니다.
$H(P,Q) = D_{KL}(P||Q) = -\sum_xP(X)logQ(X)$
