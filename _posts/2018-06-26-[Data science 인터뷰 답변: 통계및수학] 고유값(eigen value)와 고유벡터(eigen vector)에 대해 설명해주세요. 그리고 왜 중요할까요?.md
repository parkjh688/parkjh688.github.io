---
layout : post
title : "what is eigen value and eigen vector?"
date : 2018-06-26 01:00:00 +0900
author : viking
category : 데이터 사이언스 인터뷰 질문집 답변
---
<script type="text/javascript"
       src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default">
</script>
블로거이자 Machine Learning Engineer 이신 변성윤님이 블로그에 올리신 '데이터 사이언스 인터뷰 질문집'에 올린 질문들에 대해 아는 것은 아는대로, 모르는 것은 차근차근 공부하면서 올려보기로 했다.
<br>
질문을 열심히 모으셨을 것 같아서 질문에 대한 답변을 포스팅해도 되는지 물어보고 흔쾌히 허락을 해주셔서 진행하기로 했다.
<br>
포스팅되는 {질문 : 답변} ~~*뭔가 질문과 답변이 {key:value} 같아보여서..*~~ 이외에도 질문 셋들만 보고 싶다면 아래의 링크를 클릭하면 볼 수 있다.

<a href="https://zzsza.github.io/data/2018/02/17/datascience-interivew-questions/">변성윤님 블로그: 데이터 사이언스 인터뷰 질문집</a>

<br>
일주일에 최소 3개의 답변을 업로드 하는 것을 목표로 한다!
<br><br>
#### 통계 및 수학
<strong><span style="color:#006666">통계 및 수학</span></strong> contents 에서 1번 질문인

<strong>_고유값(eigen value)와 고유벡터(eigen vector)에 대해 설명해주세요. 그리고 왜 중요할까요?_</strong>

에 대한 답변 시작!

<br>
고유값(eigen value)와 고유벡터(eigen vector)이 무엇일까?
<br><br>

>  A를 𝑛 × 𝑛 정방행렬이라고 할 때, A𝐱 = λ𝐱 를 만족하는 𝑅^𝑛^ 의 영벡터가 아닌 벡터 𝐱 가 존재할 때, 스칼라 λ를 A의 고유값 (eigen value)이라 하고 벡터 𝐱 는 λ에 관련된 고유벡터 (eigen vector)라 한다.

<br>
이것이 고유값과 고유벡터의 정의다. 아는건데도 뭐라고 하는지 모르겠다.
외국어 번역하듯이 처음부터 풀어나가보자.
<br><br>

`A 라는 𝑛 × 𝑛 정방행렬이라고 할 때` 라는 문장이 있다. 그렇다면 정방행렬은 무엇일까?
정방행렬이란 행과 열의 개수가 같은 행렬을 말한다. 아래의 행렬과 같이 생겼을 것이다. <br><br>


$$A = (a_{ij})_{n\times n} =\begin{pmatrix}
a_{11} & a_{12} & \ldots & a_{1n} \\
a_{21} & a_{22} & \ldots & a_{2n} \\
\ldots & \ldots & \ldots & \ldots \\
a_{n1} & a_{n2} & \ldots & a_{nn} \\ \end{pmatrix}$$

<br>

다음은  `A𝐱 = λ𝐱 를 만족하는 𝑅^𝑛^ 의 영벡터가 아닌 벡터 𝐱 가 존재할 때 인데` 이다.
A는 행렬, 𝐱는 벡터, λ는 스칼라 값인 상관 계수(예:5, 37, pi와 같은 숫자) 일 때, A𝐱 = λ𝐱 를 만족하는 어떤 벡터 𝐱 가 존재하면서 벡터 𝐱 가 영벡터는 아니란 뜻이다.

위키피디아가 말하기를 영벡터는 영벡터는 모든 성분이 0인 벡터 (0, 0, …, 0)를 말한다.

또 그 다음은 `스칼라 λ를 A의 고유값 (eigen value)이라 하고 벡터 𝐱 는 λ에 관련된 고유벡터 (eigen vector)라 한다` 인데, 문장자체로 이해를 하면 될 것 같다.
<br>

즉, 고유값(eigen value)과 고유벡터(eigen vector)는 A𝐱 = λ𝐱 로 부터 나오기 때문에, 어떤 특정한 행렬 A 에 의해 정의되는 값으로 행렬 A 에 따라서 고유값, 고유벡터가 존재할 수도 있고 존재하지 않을 수도 있다.
<br>

$$\begin{pmatrix}
a_{11} & a_{12} & \ldots & a_{1n} \\
a_{21} & a_{22} & \ldots & a_{2n} \\
\ldots & \ldots & \ldots & \ldots \\
a_{n1} & a_{n2} & \ldots & a_{nn} \\ \end{pmatrix}
\begin{pmatrix}
    x_1\\
    x_2\\
    \ldots\\
    x_n
  \end{pmatrix}=
  λ\begin{pmatrix}
      x_1\\
      x_2\\
      \ldots\\
      x_n
    \end{pmatrix}$$

<br>
위의 고유벡터를 표현한 식을 보면, 고유벡터에 행렬 A를 곱해도, 방향(벡터 𝐱)은 그대로이고 크기(스칼라 값인 λ)만 변한다. 따라서 고유벡터는 입력 벡터 A에 대해 크기가 늘어나거나 줄어드는데 기준이 되는 축이라고 할 수 있다.

<br><br>
고유값과 고유벡터에 대해서는 무엇인지 이해했다. 그런데 이걸 어디에 쓰는걸까? 질문처럼 왜 중요한걸까?

여기서부터는 나도 잘 모르기 때문에, deeplearning4j 의 설명을 보면서 정리했다. 여기서 재미있는걸 가르쳐줬는데, ~~*나만 재미있나..*~~ 고유 벡터는 `아이겐 벡터`라고 읽는다고 한다. eigen(아이겐)은 독일어이고 `very own(자기 자신의, 고유의)` 라는 뜻이라고 한다. 고유가 무슨 뜻인진 느낌은 알긴 알겠는데 느낌이 바로 안온다.. 한국어 너무 어렵다.
<br>

고유 벡터를 이해하면 공분산(covariance), 주 성분 분석(principal component analysis) 및 정보 엔트로피(information entropy)도 자연스럽게 이해할 수 있다고 한다.
