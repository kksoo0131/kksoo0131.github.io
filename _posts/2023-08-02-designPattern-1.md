---
title: 디자인 패턴 - 싱글톤
author: kksoo0131
date: 2023-08-02 20:00:00 +0900
categories: [DesignPattern]
tags: [DesignPattern, Singleton]
render_with_liquid: false
---

## Singleton Pattern

싱글톤 패턴이란?

클래스의 `인스턴스`를 `하나만 존재`하도록 하고, 아래 조건을 만족한다.

1. 클래스 내부에서 정적 변수로 인스턴스를 생성한다

```cs
public class Singleton
{
    private static Singleton _singleton = new Singleton();
}
```
> `정적 변수`는 `클래스 레벨에 선언`되므로, 클래스가 로드 될 때 메모리에 할당 되고 프로그램이 종료될 때 메모리에서 해제됩니다.
>
> 즉, `프로그램 실행 중에는 해당 변수가 계속 유지`된다.

<br/>
<br/>

2. 인스턴스를 외부에서 추가로 생성할 수 없다. 

```cs
public class Singleton
{
    private Singleton() {}
}
```
생성자를 private로 선언하여 외부에서 인스턴스를 생성할 수 없다.

<br/>
<br/>

3. 인스턴스에 접근하기 위한 정적 메서드를 제공해 어디서든 접근할 수 있다.

```cs
public class Singleton
{
    public static GetInstance() {return _singleton};
}
```
<br/>
<br/>


## 싱글톤 유니티 ver.

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Managers : MonoBehaviour
{
    private static Managers instance;
    public static Managers GetInstance() { Init(); return instance; }
   
    static void Init()
    {
        if (instance == null)
        {
            GameObject go = GameObject.Find("@Managers");
            if (go == null)
            {
                go = new GameObject { name = "@Managers" };
                go.AddComponent<Managers>();
            }
            instance = go.GetComponent<Managers>();
        }
   
    }
    private void Awake()
    {
        Init();
    }
}


```
## 싱글톤 패턴의 장점

`장점`
1. 인스턴스를 하나만 생성하므로 메모리 사용 최적화


2. 어디서든 동일한 인스턴스에 접근할 수 있어서 데이터 공유 용이


3. 여러 모듈이 하나의 인스턴스를 공유해 데이터의 일관성 유지

`단점`
1. 전역 상태를 유지하므로 의존성이 높아질 수 있고, 코드가 복잡해질 수 있다.

2. 남발하면 결합도가 높아져서 유지보수와 테스트가 어려워질 수 있다.

3. 디버깅이 어려울수 있고, 의존성이 숨겨져 코드를 이해하기 어려울 수 있다.


## 유니티 클라이언트에서 싱글톤 패턴을 사용하는 이유

1. 데이터 및 상태 공유 : 여러 스크립트나 오브젝트 간에 데이터를 공유해야 하기 떄문에

2. 리소스 접근 : 여러 스크립트에서 접근해야 하는 리소스를 하나의 인스턴스로 관리

3. 씬 전환 관리 : 씬을 전환하면서 데이터를 보존할 떄

4. 게임 매너지 역할 : 게임 내의 여러 기능들을 통합적으로 관리

5. 효율적인 리소스 사용 : 하나의 인스턴스를 공유해 메모리 관리 용이



