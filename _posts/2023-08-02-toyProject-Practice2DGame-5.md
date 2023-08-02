---
title: 토이 프로젝트 - 2D 게임 실습 5
author: kksoo0131
date: 2023-08-02 16:00:00 +0900
categories: [toyProject, Unity]
tags: [toyProject, Unity]
render_with_liquid: false
---

## 게임 배경 정하기

고양이와 어울릴 만한 배경을 찾다가 [Pixel Art Top Down - Basic](https://assetstore.unity.com/packages/p/pixel-art-top-down-basic-187605)이라는 에셋을 발견했고

여기에 나오는 풀밭을 배경으로 쓰면 어울릴 것 같아서 사용하기로 했다.

배경을 만들때는 `2D Object` > `Tilemap` > `Rectangular` 를 생성해서 Tile Palette를 사용해 배경을 만들어 주었다.

## 배경 음악 추가하기

게임의 BGM으로 [Casual Game BGM #5](https://assetstore.unity.com/packages/audio/music/casual-game-bgm-5-135943) 에셋을 사용했다.

```cs
audioSource = gameObject.GetComponent<AudioSource>();
audioSource.clip = Resources.Load<AudioClip>("CasualGameBGM05/BGM_04");
audioSource.loop = true;
audioSource.Play();

// 게임 종료 시 스탑
audioSource.Stop();
```

GameManager에 audioSource 컴포넌트를 추가하고 해당 컴포넌트의 clip에 AudioClip을 불러와 반복 재생 시켰다.

## 빌드 해보기

그냥 윈도우 환경으로 아무런 세팅없이 빌드를 하니까 전체화면으로 게임이 실행되었다.

그래서 720 x 1280 을 창모드로 실행하도록 GameManager의 Awake() 부분에 코드를 추가했다.

```cs
Screen.SetResolution(720, 1280, FullScreenMode.Windowed);
```

## 실행

![게임 플레이 영상](/assets/videos/catchCat.mp4)
