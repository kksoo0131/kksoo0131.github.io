---
layout: post
title:  "서버란 무엇인가?"
date:   2024-01-15
excerpt: "게임 서버"
tag:
- 서버
- 게임 서버

comments: true
---

1. 서버란 무엇인가?

2. HTTP - 웹 서버

3. SOCKET - 게임 서버

4. HTTP, SOCKET의 차이점

![nbcbanner](/assets/img/TILbanner.png)


<br/>
<br/>


# 서버란 무엇인가?

다른 컴퓨터에서 연결이 가능하도록 대기 상태로 상시로 실행중인 프로그램 혹은 해당 프로그램이 실행되는 컴퓨팅 장비를 뜻한다.

중요한 점은 `다른 컴퓨터의 연결 요청을 대기하고 있는 프로그램` 이라는 것.

서버는 크게 HTTP, SOCKET 방식으로 나눌수 있다.

<br/>
<br/>

# HTTP - 웹 서버

HTTP 서버는 HTTP 프로토콜을 기반으로 하는 서버로 주로 웹 서버에서 사용된다.

클라이언트의 요청이 있을때만 연결되고 이후로는 연결이 해제된다는 특징을 가진다.

- 정보의 요청/갱신 빈도가 적을 때
- 실시간 상호작용이 필요하지 않을 때
- 클라이언트에서만 서버에 접근할때 (서버에서는 클라이언트에 접근 X, 단방향)
- 예시 1
  - HTTP방식의 웹 서버를 사용하는 웹 사이트의 게시판에서 최신글을 보기위해서는 
  
    F5를 눌러 페이지를 새로고침해 서버에 갱신을 요청해야한다. (실시간 X, 단방향)

    

- 예시 2
  - HTTP 방식의 서버로 게임의 랭킹 시스템을 구현할 때

    게임이 끝날때 내 클라이언트의 결과를 서버에 업데이트를 요청하고 랭킹을 갱신한다.
    

    대신 다음 게임이 끝날 때까지 새로운 랭킹 정보를 받아올수 없다. (실시간 X, 단방향)

    최신 상태의 랭킹을 확인하기 위해서는 일정 시간마다 서버에 갱신을 요청하거나 
    
    실시간 통신이 가능한 SOCKET 방식을 사용해야한다.

- 언어 별로 주로 사용하는 프레임 워크가 있다.
  - ASP.NET (C#)
  - Spring(Java)
  - NodeJS(Javascript)
  - Django,Flask(Python)
  - PHP
  - ...


<br/>
<br/>

# SOCKET - 게임 서버

SOCKET 서버는 다양한 프로토콜을 사용할 수 있으며 주로, TCP or UDP 소켓을 사용하는 방식으로 주로 게임 서버에서 사용된다.

클라이언트의 요청을 받으면 클라이언트와 서버의 소켓을 연결하고 이후 해제 전까지 실시간 통신이 가능하다.

- 요청/갱신의 빈도가 많을 때
- 실시간 상호작용이 필요할 때
- 서버에서도 클라이언트에 접근해야할 때 (양방향)
- 게임에 따라 요구사항이 너무나 다르기 떄문에 최적화된 프레임워크가 존재하기 애매하다.

- 게임 서버를 제작할 떄의 고려 사항
  - 최대 동시 접속자
  - 게임의 장르
  - 채널 한도
  - 스레드의 역할 구분
  - 스레드의 개수
  - 스레드의 모델
  - 네트워크 모델
  - 반응성
  - 데이터베이스

<br/>
<br/>

# HTTP, SOCKET의 차이점

결국 둘의 중요한 차이점은 `서버와 클라이언트의 연결을 유지하는가?`이다.

이로 인해 두 서버의 특징이 결정된다.

<br/>
<br/>

![nbcthumbnail](/assets/img/thumbnail-image.png)