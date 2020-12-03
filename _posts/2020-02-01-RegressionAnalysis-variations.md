---
layout: post
title: "Linear Regression Model - variations(선형 회귀 및 변동의 분석)"
comments: true
categories: Statistics
---

간단한 단순 선형회귀모형 변동의 분석에 대한 설명이다.

단순 선형회귀분석이란, 

#### <u><b> 종속변수의 변동(variation)을 하나의 독립변수의 변동으로 설명하는 것이다. </b></u>

아빠의 키 값을 가지고 아들의 키 값을 예측하는 선형회귀모델을 만든다고 했을 때,  
아빠의 키 값(독립변수)이 다른 값을 가지면, 아들의 키 값(종속변수)도 다른 값을 가지는 것을 확인할 수 있다.   
즉, 아빠의 키 값의 변동으로 아들의 키 값의 변동을 설명하는 것이다.

변동의 정의는 __'변수가 평균 값으로부터 다른 정도'__ 이다.

따라서, 종속변수가 평균 값으로부터 다른 정도(변동)를 잘 설명할 수 있는 독립변수로 구성된 모형이 좋은 회귀모델이다.

<center><img src="/img/SST.png" width="500" height="400"></center>

1. SST(Total Sum of Squares): 실제치와 평균의 차이를 보여주는 총 변동
2. SSR(Sum of Squares due to Regression): 추정치와 평균의 차이를 X로 설명하는 Y의 변동; 회귀추정치(회귀모형)로 설명 가능한 변동
3. SSE(Sum of Squared Errors): 실제치와 추정치의 차이; 회귀추정치(회귀모형)로 설명할 수 없는 변동(SSR에서 설명이 불가한 차이)

한가지 덧붙이면, R^2은 종속변수의 변동을 독립변수의 변동으로 얼마나 설명 가능한지를 측정하는 지표이다.   

R^2 = SSR/SST   

즉, 총 변동에서 모형으로 설명할 수 있는 변동을 가리키며, 당연히 높을수록 좋다.

* 혼동주의: 여기서 SSR은 Sum of Squares due to Regression 혹은 Regression Sum of Squares로, Sum of Squared Residuals(SSR)과는 다른 개념이다.