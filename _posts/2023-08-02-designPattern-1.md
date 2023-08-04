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

> `정적 변수`는 `클래스 레벨에 선언`되므로, 클래스가 로드 될 때 메모리에 할당 되고 프로그램이 종료될 때 메모리에서 해제됩니다.
>
> 즉, `프로그램 실행 중에는 해당 변수가 계속 유지`된다.

2. 인스턴스를 외부에서 추가로 생성할 수 없다. 

생성자를 private로 선언하여 외부에서 인스턴스를 생성할 수 없다.

3. 인스턴스에 접근하기 위한 정적 메서드를 제공해 어디서든 접근할 수 있다.

```cs
public class Singleton
{
    private static Singleton _singleton = new Singleton(); // 1 
    private Singleton() {} // 2
    public static GetInstance() {return _singleton}; // 3
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
유니티에서 게임의 전반적인 기능을 관리할 Manager 오브젝트

MonoBehaviour를 상속받은 싱글톤 클래스로 만들어서 관리한다.

<br/>
<br/>

## 싱글톤 패턴의 장점

`장점`
1. 인스턴스를 하나만 생성하므로 메모리 사용 최적화


2. 어디서든 동일한 인스턴스에 접근할 수 있어서 데이터 공유 용이


3. 여러 모듈이 하나의 인스턴스를 공유해 데이터의 일관성 유지

`단점`
1. 전역 상태를 유지하므로 의존성이 높아질 수 있고, 코드가 복잡해질 수 있다.

2. 남발하면 결합도가 높아져서 유지보수와 테스트가 어려워질 수 있다.

3. 디버깅이 어려울수 있고, 의존성이 숨겨져 코드를 이해하기 어려울 수 있다.

<br/>
<br/>

## 싱글톤 패턴 과 static 클래스의 공통점과 차이점

`공통점`

1. 어디서든 동일한 정적멤버에 접근하여 데이터를 공유하게 된다.

<br/>

`차이점`

1. 인스턴스 생성 방법:

        싱글톤 패턴 : 클래스 내부에서 해당 클래스이 인스턴스를 정적멤버로 생성한다.

        static 클래스 : 인스턴스를 생성하지 않고, 클래스 자체가 메모리에 상주 모든 멤버와 메서드가 정적으로 선언된다.

2. 일반 멤버와 메서드의 유무:
        
        싱글톤 패턴 : 하나의 인스턴스를 공유하여 접근하기 떄문에, 일반 멤버와 메서드를 가질 수 있다.

        static 클래스 : 인스턴스를 생성하지 않기 때문에 일반 멤버와 메서드는 가질 수 없다.

3. 의존성 주입 : 
    
        싱글톤 패턴 : 다른 클래스들이 해당 싱글톤 인스턴스를 사용하기 위해서는 `의존성 주입`을 해야한다.

        의존성 주입은 인터페이스를 활용하여 클래스들 간의 결합도를 낮추고 유연성과 테스트 용이성을 증가 시킨다;
        

        static 클래스 : 의존성 주입을 사용하지 않고 직접 호출해야한다.

> 의존성 주입 이란? : 다른 클래스의 기능을 사용할 때 , 내부에 인스턴스를 직접 생성하지 않고 외부에서 해당 인스턴스를 주입받아 사용
        
        클래스1 에서 클래스2의 기능을 쓸때 클래스1의 내부에 클래스2의 인스턴스를 생성하는게 아니라

        클래스1의 외부에 생성되어있는 클래스2의 인스턴스에 접근해서 기능을 사용한다.

        이를 통해 두 클래스의 결합도가 낮아진다.

4. `상속` : 
        
        싱글톤 패턴 : 어떤 클래스를 상속받아서 싱글톤 클래스를 만들 수 있다, 즉 , 유니티에서라면 MonoBehaviour를 상속받아서 오브젝트로 싱글톤 인스턴스를 생성할 수 있다

        static 클래스 : 다른 클래스를 상속받을수 없다.

5. 사용목적:

        싱글톤 패턴 : 여러 곳에서 동일한 인스턴스를 공유해야 할 때 사용한다. 예를 들어, 게임의 매니저 클래스

        static 클래스 : 인스턴스를 생성하지 않고도 접근할 수 있어야 하는 유틸리티 함수나 상수값

<br/>
<br/>

## 유니티 클라이언트에서 싱글톤 패턴을 사용하는 이유

1. 데이터 및 상태 공유 : 여러 스크립트나 오브젝트 간에 데이터를 공유해야 하기 떄문에

2. 리소스 접근 : 여러 스크립트에서 접근해야 하는 리소스를 하나의 인스턴스로 관리

3. 씬 전환 관리 : 씬을 전환하면서 데이터를 보존할 떄

4. 게임 매너지 역할 : 게임 내의 여러 기능들을 통합적으로 관리

5. 효율적인 리소스 사용 : 하나의 인스턴스를 공유해 메모리 관리 용이





