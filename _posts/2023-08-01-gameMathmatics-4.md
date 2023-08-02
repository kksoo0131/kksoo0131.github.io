---
title: 게임 수학의 소개 - 회전의 수학 1
author: kksoo0131
date: 2023-08-01 18:30:00 +0900
categories: [gameMathmatics]
tags: [gameMathmatics]
render_with_liquid: false
---

## 회전 이란?

트랜스 폼 이란 크기, 회전, 이동 세가지 변환을 순서대로 진행하는 합성 변환이다.

게임에서 회전은 트랜스 폼을 통해서` 물체의 로컬 공간을 회전` 시킨다.


## 공간의 변환

벡터 공간의 모든 벡터는 `표준 기저 벡터`의 선형 조합으로 만들어진다.

공간의 변환은 이 `표준 기저 벡터를 변화`시켜서 `공간의 모든 벡터를 한번에 변환`해서 

새로운 공간을 만들어 낸다.


## 회전 변환의 특징

회전 변환으로 변환된 물체는 물체의 모습이 변하지 않는다. 

즉, 표준 기저 벡터가 가지고 있던 성질을 그대로 유지시키며 변환시킨다.

    예를 들어

    2차원 공간에서 두 표준기저벡터가 가지는 성질은

    1. 크기가 항상 1이다
    2. 서로 직교하고 있다.

    이러한 성질을 유지한다면 우리는 회전 변환을 할수 있다.


    2차원 평면에서 크기가 1인 벡터를 모두 그린다면 반지름이 1인 단위 원이 그려질 것이다.
    
    이때 이 단위 원 상에 위치한 벡터를 임의로 가져 온다면 일단 1번 조건을 만족 시킨다.

    여기서 직교를 이루는 다른 벡터를 쌍으로 가져온다면 2번 조건도 만족시켜서 표준기저벡터의 성질을 만족한다.

    이것이 2차원 공간에서 회전 변환의 원리이다.

## 삼각 함수

`원호` 위의 점을 가져오기 위해서 `삼각 함수`에 대해 알아야 한다.

원호 위의 점은 (x,y)로 표현할 수도 있지만 

기준 위치에서 얼마만큼의 𝛩각만큼 회전했는지 회전한 각과 반지름을 사용해서 (r,𝛩)로도 표현할 수 있다.

여기서 수선을 그리면 직각 삼각형이 그려지고 빗변, 높이, 밑변으로 구성된다.

이 빗변, 높이, 밑변으로 만드는 비를 `삼각비` 라고하고 높이/빗변, 밑변/빗변, 높이/밑변 등이 있다. 

그리고 여기서 파생된 것이 `삼각 함수`이다.

삼각 함수에서는 삼각비를 sin𝛩 (높이/빗변), cos𝛩 (밑변/빗변), tan𝛩(높이/밑변)로 나타낸다.

단위 원의 반지름은 1이기 때문에 단위 원 상에 위치한 `임의의 한 점의 좌표는 (cos𝛩, sin𝛩)`가 되고 이때 `sin𝛩 = 높이, cos𝛩 = 밑변`이 된다.

## 삼각 함수를 사용해 직교하는 두 기저벡터를 얻는 법

표준 기저 벡터에서 각 𝛩만큼 이동한 벡터 `(cos𝛩, sin𝛩)`에 직교하는 다른 벡터는 (cos(𝛩+90), sin(𝛩+90)) 즉, `(-sin𝛩, cos𝛩)`가 된다.

결국 회전은 두 표준 기저 벡터 (0,1)과 (1,0)을 (cos𝛩, sin𝛩)와 (-sin𝛩, cos𝛩)로 재구성한 한것이고,

행렬을 통해 쉽게 계산할 수 있다.


## 3차원 공간에서의 회전

3차원 공간에서는 임의의 2차원 평면을 설정하고 회전시켜야 한다.

1. 축 - 각 회전 (Axis-Angle): 임의의 회전축을 하나 설정하고 우리가 회전하고자 하는 점이 속한 평면을 만들고 회전 시킨다.
2. 오일러 각 회전 (Euler Angles): x,y,z 세개의 표준기저벡터를 중심축으로 잡고 지정된 순서에 따라서 하나씩 세번 회전 시킨다.


## 축 - 각 회전 (Axis-Angle Rotation)

축-각 회전의 대표적인 방식

1. 로드리게스 회전 공식 : 내적과 외적을 사용해 계산한다.

단점 : 

1. 직관적이지 않고 행렬로 변환하기 어렵기 때문에 렌더링 파이프라인의 중간에 위치한 파이프라인 흐름이 끊겨 효율이 떨어질 수 있다.

## 오일러 각 회전 (Euler Angles Rotation)

하나의 회전을 3번의 회전으로 쪼개서 회전한다.

회전에 필요한 회전축은 표준기저벡터를 사용하고 얼만큼 돌아갈지 각에 대한 정보만 저장

> 직관적이고 데이터가 적게 사용되어 대부분의 3차원 소프트웨어에서 사용된다.

단점 : 

1. 손쉽게 행렬로 만들수 있지만, 한번의 회전을 3번으로 끊어서 표현하기 때문에 임의의 축에 대해

부드럽게 움직이는 회전을 계산하기 비효율적 이고 까다롭다.

2. 한축의 회전이 증발하는 짐벌락 현상이 발생할 수 있다.


## 3차원 회전을 안정적으로 구현하는 방법?

축-각 회전과 오일러 각 회전에 있는 단점들을 해결하기 위해서

다차원의 수체계를 사용하는 `Quaternion(사원수)`를 사욯한다.

## 수 란? 

수학에서는 집합의 관점에서 수를 관리한다.

하지만 수를 단순한 집합 관점에서만 보기에는 한계가 있다

수는 사칙연산이 가능하기 때문에 단순한 다른 집합들과는 `차별점`을 가지고 있다.

사칙연산이란 수와 수를 결합해 새로운 수를 만들어내는 `시스템`이고

그렇기 때문에 `수`는 `시스템(구조)를 가진 집합`이다.

그래서 아래의 `연산에 대한 공리(Axiom)을 만족하는 집합을 수 라고 한다.`

    1. 교환 법칙
    2. 결합 법칙
    3. 분배 법칙
    4. 항등원
    5. 역원

여러 요소로 구성된 수의 대표로는 복소수가 있다.

`복소수`는 두개의 원소로 구성된 수로 (a + bi) 실수와 허수로 구성되어 있지만 위의 연산에 대한 공리를 만족하는

집합이기 때문에 `수`라고 부를수 있다.

## 복소수의 곱셈연산

임의의 복소수 x+ yi에 크기가 1인 복소수 ( cos𝛩, sin𝛩i) 를 곱하는 것은

회전 변환의 식과 완전히 동일하다. 

따라서 `크기가 1인 복소수의 곱`은 `평면에서의 회전 변환과 동일`하다.

## 사원수

네 개의 원소로 구성된 `수`로 복소수에서 개념이 확장된 수로 실수부와 허수부로 구분된다.

> 사원수 :  a + bi + cj + dk (a는 실수부 나머지는 허수부)

이를 3차원 공간에서 사용할때는 0 + xi + yj + zk로 대응시킨다.

사원수의 a(실수부)를 `스칼라`, 사원수의 bi+cj+dk (허수부)를 `벡터`로 간주한다.

사원수의 곱셈 연산을 스칼라와, 벡터로 나누어서 계산하게 되면

    q1 * q2

    = (w1w2 - (v1 * v2), w1v2 + v2v1 + v1 x v2)

    즉, 벡터의 내적과 외적으로 요약할 수 있따.


사원수도 복소수와 마찬가지로 `크기가 1인 사원수와의 곱`은 `4차원 공간에서의 회전`을 의미한다.

3차원 공간에서 임의의 축 n에 대해 0할때 크기가 1인 사원수와의 곱으로 나타낼 수 있다.

이 곱셈을 요약하면 v+ w(r x v) + n x (r x v)로 벡터의 외적식으로 떨어지게되고

이것은 `게임엔진`에서 `3차원 공간의 벡터 기본 회전공식`으로 사용된다.

> 사원수를 사용해 빠르게 벡터를 회전시킬 수 있고, 행렬로 변환이 용이하며
> 
> 오일러 방식의 단점을 해결할 수있지만 축 - 각 방식이고 절반의 각으로 파생되기 때문에 직관적으로 사용하기는 어렵다.
>
> 그래서 내부적으로 사용되며 게임 제작에서는 여전히 오일러 각을 유용하게 사용한다.


## Reference
[게임 수학의 이해 - 이득우](https://www.inflearn.com/course/%EA%B2%8C%EC%9E%84-%EC%88%98%ED%95%99-%EC%9D%B4%ED%95%B4/dashboard)