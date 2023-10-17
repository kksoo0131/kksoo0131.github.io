---
layout: post
title:  "GitHub Page 만들기"
date:   2023-06-27
excerpt: "GitHub Page 만들기"
tag:
- GitHub
comments: true
---

## GitHub Page ?

GitHub에서 무료로 제공하는 정적 웹 호스팅 서비스 별도의 웹 서버를 구축하거나 관리할 필요없이 GitHub를 통해 웹 사이트를 배포할 수 있습니다.

GitHub Page에 Jekyll로 생성한 웹 사이트를 올려서 Blog를 만들수 있습니다.

### 1. GitHub Page 생성

- repository의 이름을 `USERNAME.github.io`로 생성한다.

- repository에 저장된 웹 사이트를 `https://USERNAME.github.io`를 통해 접속할 수 있게된다.

- GitHub Desktop을 이용해서 해당 repository의 clone을 자신의 컴퓨터에 만들어줍니다.

## Jekyll ?

Ruby 기반의 정적 웹 사이트 생성기로 마크다운 이나 HTML과 같은 마크업 언어로 작성된 웹 페이지와 템플릿을 사용하여 웹사이트를 생성하는 도구

### 2. Ruby 설치

- Jekyll는 Ruby 기반이기 때문에 Jekyll 설치하려면 Ruby를 먼저 설치 해주어야 합니다.

- [Ruby 공식 설치 사이트](https://rubyinstaller.org/downloads/) 에서 Ruby+Devkit를 다운로드 합니다.

- 설치파일을 실행 후, 별 다른 수정 없이 default 값으로 설치를 진행합니다.


### 3. Jeykll 설치

- 윈도우 검색창에 ruby를 겅색하여 `Start Command Prompt with Ruby`를 실행해 줍니다.


- 자신의 repository clone을 저장한 폴더의 경로로 이동합니다.

    ```
    cd C:\Users\Kks\Desktop\kksoo0131.github.io 
    ```

- 경로에 유니코드 문자가 포함된 경우 발생하는 문제를 방지하기 위해 UTF-8로 인코딩 합니다. (입력 후 `Active code page: 650001` 가 출력되면 정상입니다.)
    ```
    chcp 65001
    ```

- jekyll, bundle 설치
    ```
    gem install jekyll bundler
    ```

### 4. 원하는 테마 적용

- 구글링을 통해 원하는 테마를 다운로드 받습니다. ex ) [jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy)

- 다운로드 받은 파일의 압축을 풀어 컴퓨터에 repository clone폴더에 넣어줍니다.

- bundle install을 입력해 프로젝트의 Gemfile에 명시된 종속성을 확인해 해당되는 Ruby Gem패키지들을 설치해 줍니다.
    ```
    bundle install
    ```

- 준비가 완료됬으니 jeykll을 통해 정적 웹사이트를 빌드하고 로컬서버를 실행합니다.
    ```
    bundle exec jekyll serve
    ```

- [로컬 주소](https://127.0.0.1:4000/) 에 접속해 웹사이트가 제대로 생성되었는지 확인합니다.


### 5. Push

- 생성된 웹 사이트를 깃허브에 push합니다.

- 제 경우에는 jekyll로 빌드된 웹사이트가 _site폴더에 저장 되었는데 

- 이 폴더의 파일들을 루트 폴더(repository clone 폴더)로 옮겨서 push했을때 정상적으로 작동했습니다.


### 6. 작동 확인

- 깃허브에 들어가서 USERNAME.github.io Repository의 Settings - Page탭에서

- Page가 업데이트된 시간을 확인하고, 업데이트 됬으면

- `https://USERNAME.github.io`에 접속해서 확인합니다.

