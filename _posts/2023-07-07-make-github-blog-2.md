---
layout: post
title:  "Blog 코드블럭 커스텀"
date:   2023-07-07
excerpt: "Blog 코드블럭 커스텀"
tag:
- Blog
- jekyll
comments: true
---

_sass/color 폴더를 확인해보니 dark-syntax.scss 파일이 다크모드 스타일을 정의하는것 같았고

확인해보니 코드블럭안의 색상과 인라인코드 색상을 지정할수 있었다.


```scss

@mixin dark-syntax {
    /*코드 블록*/
  --language-border-color: #4ea1d3; // 테두리 
  --highlight-bg-color: #454552;             // 배경
  --highlight-lineno-color: #d8e9ef;                   // 코드라인 번호
  --code-header-text-color: #d8e9ef;                  // 헤더
  --code-header-muted-color: #4ea1d3;        // 헤더 보조 (왼쪽의 동그라미 세 개)
  --code-header-icon-color: #4ea1d3;          // 헤더 아이콘
  --clipboard-checked-color: #e85a71;                // 복사 후 버튼
  --filepath-text-color: #e85a71;                      // 파일 경로
  pre { color: #d8e9ef}                               // 텍스트

  /*인라인 코드 (``)*/
  --highlighter-rouge-color: #23c4a9;              // 글자
  --inline-code-bg: #222221;                           // 배경

  // 이후로 .highlight 클래스의 각각 속성에 대한 색상 지정

}
```

## 수정 전
![before](/assets/img/post/2023-07-06-make-github-blog-3-1.png)

## 수정 후
![after](/assets/img/post/2023-07-06-make-github-blog-3-2.png)

## 수정해야 할 내용

    1. 하위 카테고리 구분
    
    2. 카테고리 탭 수정
