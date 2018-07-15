---
layout : post
title : "Cost Function & Activation Function"
date : 2018-06-26 01:00:00 +0900
author : viking
category : 데이터 사이언스 인터뷰 질문집 답변
---
<script type="text/javascript"
       src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default">
</script>

<a href="https://zzsza.github.io/data/2018/02/17/datascience-interivew-questions/">'데이터 사이언스 인터뷰 질문집'</a> 의 <strong><span style="color:#eabe1e">딥러닝</span></strong> contents 의 아래에 보이는 질문 몇 개를 답변 해보겠다. 질문집에 나와있는 질문 순서와 동일하지는 않다.

* Cost Function과 Activation Function은 무엇인가요?
* 알고있는 Activation Function에 대해 알려주세요. (Sigmoid, ReLU, LeakyReLU, Tanh 등)

<br>

제일 먼저 `cost funtion` 이 무엇일까? cost funtion 에서의 cost 가 의미하는 것을 먼저 생각하면 쉬울 것 같다. **cost** 는 Data set 과 Hypothesis 사이의 오차를 계산하는 함수이다.


![cost_function](https://dl.dropbox.com/s/8obdaj5eefhd1oz/costfunction.png?dl)

위의 이미지의 파랑색 직선이 내가 정한 Hypothesis 라고 생각하고, X 표시들이 data set 이라고 생각했을 때 일치하지 않는다. 그래서 각 데이터들과 파랑색 직선 사이에 오차가 생긴다.

그 오차를 `cost` 라고 한다. 그리고 `cost function` 은 cost 를 구해주는 function(함수)이다.
