---
layout: post
title:  "토이 프로젝트 - 2D 게임 실습 2"
date:   2023-07-27
excerpt: "토이 프로젝트로 복습"
project: true
tag:
- Unity
- ToyProject
comments: true
---


## 게임 방식

1. 화면에 랜덤 고양이 스프라이트가 랜덤으로 생성된다.
2. 고양이를 클릭하면 점수를 얻고 고양이가 사라진다.

## 고양이 움직이기

고양이가 랜덤으로 랜덤위치에 생성되어서 고양이처럼 움직이게 해야한다.

일단 고양이에 필요한 속성들을 생각해 봤다.

- 이동 속도
- 이동 방향
- 현재 상태

이 세가지 속성을 가지고 현재 상태에 따라서 애니메이션을 재생하려고 한다.


### 애니메이션 재생
```cs
enum CatState
{
    Idle,
    Walk,
    Run,
}

animCoolTime -= Time.deltaTime; 

        if (animCoolTime < 0)
            state = (CatState)Random.Range(0, 3);

        if (state != (CatState)anim.GetInteger("intState"))
        {
            AnimChange();
        }
```

애니메이션 변경 쿨타임이 지났을때만 애니메이션을 랜덤으로 돌려주고 현재 상태와 달라질때만 실제로 애니메이션을 변경하도록 만들었다.

```cs
if (state == CatState.Idle)
{
    anim.SetInteger("intState", 0);
    animCoolTime = 2.5f;
}
else if (state == CatState.Walk)
{
    anim.SetInteger("intState", 1);
    animCoolTime = 5f;
}
else if (state == CatState.Run)
{
    anim.SetInteger("intState", 2);
    animCoolTime = 2.5f;
}
else
{

}
```

애니메이션을 변경했다면 애니메이션 쿨타임을 설정해 일정시간동안 애니메이션이 변경되지 않도록 했다.

### 이동하기

이동을 하는건 간단한데, 이동방향을 어떻게 정해주지?

```cs
float dir = 0f;
float speed = 0.5f;

void CatMove()
    {
        dir = Random.Range(0, 1f);
        transform.position += new Vector3(speed * dir * Time.deltaTime, speed * (1 - dir) * Time.deltaTime, 0);
    }
```

일단 x,y 이동의 비율을 정할 변수 dir 을 생성하고

dir 계수를 따라서 x,y를 이동해주면 합이 1이되도록 이동을 할 것이다.

하지만 이것만으로는 양의 방향으로만 이동을한다. 그래서

```cs
void CatFlip()
    {
        if (Random.Range(0,101) <= 15)
        {
             transform.localScale = new Vector3(transform.localScale.x * -1, 1, 0);
             speed *=-1;
        }
    }  
```

이렇게 15%의 확률로? 고양이를 뒤집게 만들어봤다. 하지만 방향이 전환되는 속도가 너무 빨라졌다.
그래서 방향도 쿨타임이 있도록 애니메이션을 바꿀때 바뀌게했다

하지만 이렇게는 전체 대각성 이동의 가짓수 중에서 반밖에 커버하지 못한다는걸 깨달았다.
x와 y의 방향이 서로 다른 경우도 고려해야 하는것이다.

```cs
 void CatMove()
    {
        float dx = speed * dir *xFlip * Time.deltaTime;
        float dy = speed * (1 - dir) * yFlip * Time.deltaTime;
        transform.position += new Vector3(dx, dy, 0);
    }

void CatFlip()
    {
       if (Random.Range(0,101) <= 50)
        {
            int rInt = Random.Range(0, 3);
            if (rInt == 0)
            {
                transform.localScale = new Vector3(transform.localScale.x * -1, 1, 0);
                xFlip *= -1;
            }
            else if (rInt == 1)
            {
                transform.localScale = new Vector3(transform.localScale.x * -1, 1, 0);
                xFlip *= -1;
                yFlip *= -1;
            }
            else
            {
                yFlip *= -1;
            }
            
        }
    }
```

그래서 결국 x와 y의 방향을  x만 바뀌는경우 y만 바뀌는경우 둘다 바뀌는 경우로 나누도록 했다.

다음은 고양이가 화면밖으로 나가지 않게 방향을 바꾸는 작업을 해야겠다.