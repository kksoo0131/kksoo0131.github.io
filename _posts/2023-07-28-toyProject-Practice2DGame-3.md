---
title: 토이 프로젝트 - 2D 게임 실습 3
author: kksoo0131
date: 2023-07-28 17:00:00 +0900
categories: [toyProject, Unity]
tags: [toyProject, Unity]
render_with_liquid: false
---

## 오늘 해야할 내용

어제는 고양이 오브젝트가 모든 방향으로 이동할 수 있도록 만들어 줬다.

오늘은 고양이 오브젝트가 화면 밖으로 나가지 않게 화면 끝에 도달하면

방향을 반대로 전환하도록 만들어 줄 것이다. 

그리고 고양이를 GamaManager에서 계속해서 생성하도록 만들어야 한다.


## 고양이를 화면안에 가두기!

고양이를 화면안에 가두기 위해서는

고양이의 스크립트 `cat.cs`에서 고양이가 화면의 영역 밖으로 나갔는지 판단하고

영역 밖으로 나간다면 방향을 반대로 바꾸어주어야 한다.

```cs
 void CameraCheck()
    {
        Vector3 viewPort = Camera.main.WorldToViewportPoint(transform.position);

        if (viewPort.x < 0 || viewPort.x > 1)
        {
            xFlip *= -1;
        }

        if (viewPort.y <0 || viewPort.y > 1)
        {
            yFlip *= -1;
        }
    }
```
고양이의 월드좌표를 카메라의 뷰포트 좌표로 변환해서 화면 밖으로 나갔는지를 

x축과 y축을 따로 판별하고 방향을 반대로 바꾸어준다.

그렇다면 이 메서드는 언제 실행 되어야 할까?

```cs
 void Update()
    {
        animCoolTime -= Time.deltaTime; 

        if (animCoolTime < 0)
            state = (CatState)Random.Range(0, 3);

        if (state != (CatState)anim.GetInteger("intState"))
        {
            AnimChange();
        }

        CatMove();


    }
```
현재 내 코드에서는 AnimChange()에서 방향 전환하는 부분을 포함하고 있었다.

그런데 화면밖을 벗어나는걸 판별하는부분은 CatMove()가 실행 될때 마다 같이 실행 되어야한다.

`그리고 방향을 바꾸자마자 다시 바뀌는 일어 없도록 방향 전환과 쿨타임을 공유해야된다.`

그래서 방향을 전환하는 부분을 AnimChange() 에서 빼서 따로 만들어 주어야 겠다.

```cs

void CatMove()
    {
        float dx = speed * dir *xFlip * Time.deltaTime;
        float dy = speed * (1 - dir) * yFlip * Time.deltaTime;
        transform.position += new Vector3(dx, dy, 0);

        CameraCheck();

        if (dirCoolTime < 0)
        {
            CatFlip();
            dir = Random.Range(0f, 1f);
        }
    }
```
dirCoolTime 변수를 선언해서 방향을 바꿀때 마다 쿨타임을 생성하도록 만들었다.

완벽하지는 않지만 어느정도는 원하는 기능을 구현한 것 같아서

이제는 `고양이를 랜덤으로 계속해서 생성하는 기능`을 추가하려고 한다.

이 기능은 `GameManager 오브젝트`를 생성해서 `GameManager.cs`에 메서드를 만들어 줄 것이다.

```cs
    void Start()
    {
        cat1 = Resources.Load<GameObject>("Prefabs/cat1");
        InvokeRepeating("MakeCat", 0, 10f * Time.deltaTime);
    }

    void MakeCat()
    {
        Instantiate(cat1);
    }

```

그리고 이제 고양이를 클릭하면 고양이가 사라지는 기능을 추가 한다.

```cs

void OnMouseDown()
    {
        GameManager.Instance.AddScore();
        Destroy(gameObject);
    }
```
OnMouseDown() 메소드를 오버라이딩해서 Collider를 마우스로 클릭 했을때 어떻게 할지 정해줄 수 있다.

## 방향성

대충 기본적인 기능은 구현 했으니 정확히 어떤 게임으로 만들지 정해야 한다.

일단 첫 번째로 생각난 아이디어다.

    1. 고양이가 일정시간 마다 계속해서 소환된다.
    2. 시간이 지나면 점점 체력이 높은 고양이가 소환되거나 고양이수가 더늘어난다 (난이도가 올라간다.)
    3. 맵에 소환된 고양이의 수가 한계를 넘어서면 패배하고 몇초를 버텼는지 기록한다.

그리고 이건 두 번째로 생각난 아이디어 이다.

    1. 매 라운드 마다 고양이가 라운드 수 만큼 생성된다.
    2. 고양이를 모두 잡으면 바로 다음 라운드로 넘어간다.
    3. x 라운드까지 도달하는데 걸리는 시간을 기록한다.


## 2번안 

일단 먼저 2번안을 선택해서 만들어 보자.

```cs
void Update()
    {
        playTime += Time.deltaTime;
        timeTxt.text = playTime.ToString("F2");

        if (roundTxt.text != round.ToString())
        {
            roundTxt.text = round.ToString();
        }

        if (catNum == 0 && round <= 15)
        {
            catNum = ++round;

            for (int i=0; i<catNum; i++)
            {
                Instantiate(cat1);
            }
        }
        else if (round > 15)
        {
            EndGame();
        }
        
    }

    void EndGame()
    {
        Time.timeScale = 0f;
    }

    public void AddScore()
    {
        catNum--;
    }
```
게임 매니져에서 `playTime` `round` `catNum` 변수들을 사용해서 플레이 시간, 현재 라운드 , 남은 고양이수를 관리한다.

남은 고양이 수가 0이 되서 round가 종료되면 다음라운드로 진행하게되고 다시 고양이가 생성된다.

그리고 최종라운드인 15라운드를 마치면 게임이 종료된다.
