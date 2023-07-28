---
title: 게임 수학의 소개 - 공간에 대한 수학
author: kksoo0131
date: 2023-07-26 17:50:00 +0900
categories: [gameMathmatics]
tags: [gameMathmatics]
render_with_liquid: false
---

# `공간에 대한 수학`

게임세계는 현실세계를 모방한 가상세계이다.

하지만 현실세계는 불확실하지만 컴퓨터로 만든 가상세계는 `수로 만들어진 명확한 시스템`이고 이를 표현하는 일반적인 방법은 `벡터 공간`으로 표현하는것 이다.

> 벡터 공간 : 수의 체계를 기반으로 해서 쌓아올린 다차원의 데이터


## 벡터와 스칼라

벡터 공간은 벡터와 스칼라로 구성된다.

- 벡터 : 크기와 방향을 가진 물리량, 벡터 공간의 원소

- 스칼라 : 크기만 가진 물리량, 체 집합의 원소

## 체의 개념
`체 집합`: 어떤 특정한 규칙을 따르는 집합 이다. 기본적으로 세가지 성질을 만족 해야한다. 

 1. 닫힘 : 체 집합에 속하는 임의의 두 원소를 더하거나 곱하면 그 결과 또한 체 집합에 속하는 원소이다
 2. 결합 법칙
 3. 항등원
   
> 여기서 덧셈과 곱셈은 `일반적인 수학적 덧셈과 곰셈과는 다른 형태`일수 있다.
> 
> 체 집합의 에시 : 실수 체 , 유리수 체, 정수 체 , 유한 체 등
> 
### 체 집합의 예시 2

`체`의 예시 :
 
 1. 자동차의 방향을 나타내는 변수가 있다고 가정,
 
 2. 이 변수는 N,E,S,W와 같은 네 가지 상태를 가질 수 있고, 이때 집합 {N,E,S,W}가 유한체가 된다.
 
 3. `덧셈과 곱셈 모두 해당 방향으로 방향을 변경한다는 규칙을 가진다.`
 
 4. 덧셈 연산 -> 현재 방향이 N이고 N을 더하면 방향이 그대로 유지되어 N이다
 
 5. 곱셈 연산 -> 현재 방향이 N이고 S를 곱하면 방향이 S로 바뀐다

 6. 항등원 -> 현재 방향과 같은 값을 더하거나 곱하면 현재방향이 나온다.

 7. 결합법칙 -> N * (S *E) = (N * S) * E = E

 이렇게 자동차의 방향을 나타내는 유한 체 집합이 정의하였고 집합 {N,E,S,W}는 체집합이 되었다.

## 선형 변환과 행렬

1초에 많은 프레임을 찍어내기 위해서는 공간의 변환이 빠르고 단순 명료하게 이루어져야 한다.

그렇게 때문에 공간의 변환은 `선형 변환`을 통해서 이루어진다.

이를 컴퓨터가 수행하도록 하는 도구가 `행렬`이고 이를 통해 가상 공간이 빠르게 변화할 수 있다.


## 평면의 방정식

평면의 방정식을 사용해 여러개의 평면으로 이루어진` 공간안의 영역을 구축해서 활용`할 수 있다.

ex ) 카메라가 보는 영역을 절두체(Frustum)으로 구축하고 카메라에 보이는 물체만 렌더링하는 매커니즘





## Reference
[게임 수학의 이해 - 이득우](https://www.inflearn.com/course/%EA%B2%8C%EC%9E%84-%EC%88%98%ED%95%99-%EC%9D%B4%ED%95%B4/dashboard)