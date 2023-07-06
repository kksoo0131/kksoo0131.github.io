---
title: Blog 커스텀 도전
author: kksoo0131
date: 2023-07-06 14:16:00 +0900
categories: [Blog]
tags: [Blog]
render_with_liquid: false
---

jekyll-theme-chirpy 테마를 이용해서 블로그를 생성했는데 마음에 안드는 부분이 있어서 수정을 해보려고 한다.

`jekyll`는 이번에 처음 사용해봤고 `html`, `css` 같은 건 학부때 배우기는 했지만 다 까먹어서 `chat-gpt`를 사용해서 원하는 부분을 찾아서 조금씩 바꿔보려고 한다.


## Sidebar
지금은 사이드 바에 있는 카테고리를 누르면 카테고리 목록 페이지가 뜨는데 

![현재 사이드바 카테고리 사진](/assets/img/post/2023-07-06-make-github-blog-2-1.jpg)


사이드 바에 여러 카테고리가 나오고 클릭했을때 바로 해당 카테고리로 넘어가게 하려고한다.

sidebar에 대한 html코드는 _includes/sidebar.html에서 찾을수 있었다.

```html
<!-- the real tabs -->
    {% for tab in site.tabs %}
      <li class="nav-item{% if tab.url == page.url %}{{ " active" }}{% endif %}">
        <a href="{{ tab.url | relative_url }}" class="nav-link">
          <i class="fa-fw {{ tab.icon }}"></i>
          {% capture tab_name %}{{ tab.url | split: '/' }}{% endcapture %}

          <span>{{ site.data.locales[include.lang].tabs.[tab_name] | default: tab.title | upcase }}</span>
        </a>
      </li>
      <!-- .nav-item -->
    {% endfor %}
```

이 코드는 site.tabs에 정의된 정보를 기반으로 탭을 생성하고
현재 페이지인경우 active 클래스를 추가해 활성화 상태로 표시한다.

탭에 대한 정보는 _tabs폴더에 여기서 원래 있던 카테고리 탭은 삭제했다.

```html
  {% for category in site.categories %}
    <li class="nav-item{% if category.url contains category[0] %}{{ active }}{% endif %}">
    <a href="{{ '/categories/' | relative_url }}{{category[0] | downcase | url_encode}}" class="mx-2">{{ category_name }}
      <i class="fa-fw fas fa-foleder"></i>
      <span>{{ category[0]}}</span>
    </a>
  </li>
  {% endfor %}
```
그리고 사이드 바의 홈 버튼과 다른 탭들 사이에 카테고리의 목록이 나오게 새로 코드를 추가했다.
원래 카테고리 목록 페이지에서 카테고리 페이지로 넘어가는 부분을 옮겨왔다.

## +
CI의 Test과정에서 문제가 발생했다.
1. 폴더는 소문자로 만들어지는데 경로를 대문자로 적어서 오류가 발생 downcase 필터로 소문자로 변경
2. c++카테고리의 폴더가 c로 만들어지는 문제 카테고리명을 cpp로 변경
## 다음 수정해야 할 내용

    1. 하위 카테고리 구분
