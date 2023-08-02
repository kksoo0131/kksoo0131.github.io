---
title: 토이 프로젝트 - 2D 게임 실습 4
author: kksoo0131
date: 2023-07-31 17:00:00 +0900
categories: [toyProject, Unity]
tags: [toyProject, Unity]
render_with_liquid: false
---

## 고양이 종류 추가

고양이 종류를 3가지로 늘리고 고양이를 소환할 때 
GameManager에서 랜덤으로 3가지중 하나의 고양이를 골라서 소환하도록 했다

```cs
 if (catNum == 0 && round <= 15)
        {
            catNum = ++round;

            for (int i=0; i<catNum; i++)
            {
                switch(Random.Range(0, 3))
                {
                    case 0:
                        Instantiate(cat1);
                        break;
                    case 1:
                        Instantiate(cat2);
                        break;
                    case 2:
                        Instantiate(cat3);
                        break;
                    default:
                        break;
                }
                
            }
        }
```
## retryBtn

60초가 주어지고 타이머가 0이되면 게임이 종료되어 

retryBtn과 점수판이 나온다.

```cs
 void EndGame()
    {

        Time.timeScale = 0f;

        if (PlayerPrefs.HasKey("bestScore"))
        {
            if (PlayerPrefs.GetInt("bestScore") < catchCat)
            {
                PlayerPrefs.SetInt("bestScore", catchCat);
            }
        }
        else
        {
            PlayerPrefs.SetInt("bestScore", catchCat);
        }

        bestScore.text = PlayerPrefs.GetInt("bestScore").ToString();
        nowScore.text = catchCat.ToString();

        retryBtn.SetActive(true);
    }
```
PlayerPrefs를 사용해서 최고 점수를 저장하고 점수판에 출력한다.


## to-do-list

1. 어떤 기능을 더 넣을지 고민

2. 고양이랑 맞는 배경 에셋을 유니티 에셋스토어 찾기

3. 고양이 소리, 배경 음악등 게임 사운드 찾기



