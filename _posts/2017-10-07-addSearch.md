---
layout:     post
title:      "Github Blog 검색 가능하게 하는 방법"
subtitle:   "google, naver, daum 검색 노출 시키는 방법!"
date:       2017-10-07 22:52:00
author:     "MinJun"
header-img: "img/tags/Github-bg.jpg"
comments: true
tags: [Github]
---

github blog 도 만들고, 댓글 기능도 구현 했다면, 이제는 google, naver, daum 검색에 노출되어 봅시다! 저도 이번에 각 검색 엔진에 노출 되는 방법을 검색 하면서 알게 되었는데, 각 검색 엔진 마다 그 검색엔진에 어떤 등록 절차를 거치면, robot이 등록 절차를 거친 홈페이지에 접근해서, 포스트 되는 정보들을 긁어(?) 가면, 검색엔진에 노출이 된다고 합니다.(자세하게 들어가면 복잡하지만, 대강의 이해만 하고 건너갑니다.)

---

## 준비해야 하는것 

1. sitemap.xml 만들기 
2. RSS -> feed.xml 만들기 
4. robots.txt 만들기 
3. 검색엔진에 등록하기

---

## sitemap.xml 만들기

[여기 코드를 복사해서 사용합니다.](https://github.com/devminjun/devminjun.github.io/blob/master/sitemap.xml)

> 위의 코드를 복사할때  위의  --- 두줄도 같이 복사해서 사용하셔야 합니다.

1. 위의 코드를 복사해서, 자신의 github blog `root 디렉토리`에  `sitemap.xml` 을 만들어서 복사 붙여놓기를 합니다.

2. 그럼 sitemap.xml 파일 만들기 완료.

> 혹 xml 파일을 못만들겠다면, 터미널에서 xml 파일을 만들 위치로 들어가서 `touch sitemap.xml` 작성하시면 됩니다.

---

## feed.xml 파일 만들기 

[여기의 코드를 복사해서 사용합니다.](https://github.com/devminjun/devminjun.github.io/blob/master/feed.xml)

1. 위의 코드를 복사해서, 자신의 github blog `root 디렉토리`에  `feed.xml` 을 만들어서 복사 붙여놓기를 합니다.

2. 그럼 feed.xml 파일 만들기 완료.

---

## robots.txt 만들기

각 사이트마다, robots.txt를 만들어 놓습니다. 크롤러들이 어느 정도 까지 긁어 갈수 있게 허용 해주는 대문(?) 같은 역활을 합니다. 

일단 우리의 목적은 검색 엔진에 노출 되는거니까, 세부적인 설정은 이후에 하도록 합니다.

```txt
User-agent: *
Allow: 
```

1. 위의 코드를 복사해서, 자신의 github blog `root 디렉토리`에 `robots.txt`을 만들어서 복사 붙여 놓기를 합니다

> 크롤러들이 긁어 갈수 있는 정도를 설정 하는 방법은 google 하면 보다 자세하게 나와 있습니다. 저는 일단 모두 허용 하고 시작했습니다.


---



## google 검색 엔진에 등록 하는 방법

![screen](/img/posts/addSearch.jpg)

1. [구글 웹마스터 도구](https://www.google.com/webmasters/tools/home?hl=ko)에 접속하고, `속성` 추가를 누른후, blogURL을 등록합니다. ex) https:자신의github블로그 주소

2. 구글에서 제공하는 html 다운로드

3. 자신의 블로그  `root 디렉토리`에  다운받은 html을 드래그엔 드랍 해서 다져다 놓습니다.

> blog 디렉토리에 그냥 집어 넣으면됩니다!

4. commit 이후에 인증 확인을 눌러서, 인증 확인 된다면 완료!

5. 이제 [구글 웹마스터 도구](https://www.google.com/webmasters/tools/home?hl=ko) 에 가시면, 자신의 github blog 가 속성 추가 된것을 확인할수 있습니다. 들어가봅니다.

6. `크롤링 -> sitemaps` 메뉴에가서 `sitemaps.xml` 을 제출합니다.

7. 완료

---

## Daum 등록 

![screen](/img/posts/addSearch1.jpg)

1. [Daum 검색등록](https://register.search.daum.net/index.daum) 에 접속해서 로그인 이후 `블로그 등록` 카테고리를 선택후, 자신의 블로그 URL을 입력합니다.

---

## Naver 등록

1. [네이버 웹마스터 도구](http://webmastertool.naver.com/board/main.naver) 를 들어가서 로그인, 이후 사이트를 등록합니다.

2. 사이트 등록후 `요청` 탭에 `RSS 제출`, `사이트맵 제출` 을 들어가서 각각 제출을 해줍니다. 

> 사이트맵과 RSS 는 아까만든 sitemap.xml, 과 feed.xml 을 제출 하시면 됩니다.


---

## 여담 

여기까지만 하면, naver에 제일 빠르게 노출이 됩니다. 저는 추석연휴에 등록해서 그런지, google 과 daum이 아직 등록이 되지 않았습니다. 내 blog의 post가 검색엔진에 노출이 되니까 신기하면서 오묘한 희열(?) 이 느껴집니다..ㅎㅎ

---

## Reference

[초보 몽키님 블로그](https://wayhome25.github.io/etc/2017/02/20/google-search-sitemap-jekyll/) <br>
[Dveamer님 블로그](http://dveamer.github.io/homepage/SubmitSitemap.html)<br>
[naver 웹 마스터 도구 검색 가이드](http://webmastertool.naver.com/guide/basic_markup.naver#chapter1.3)<br>
[naver 신디케이션 이란?](https://blog.usefulparadigm.com/네이버-신디케이션-제대로-쓰기-4edbff52ace1)<br>
















