---
layout:     post
title:      "Github Blog에 댓글(disqus) 기능 추가하기"
subtitle:   "disqus 사용 용해서 댓글 기능 사용하기"
date:       2017-10-07 14:37:00
author:     "MinJun"
header-img: "img/tags/Github-bg.jpg"
comments: true
tags: [Github]
---

github blog 에 댓글 기능을 넣기 위해서는 `disqus`를 사용합니다. 댓글을 달때 facebook, google, disqus 계정 등을 이용해서 댓글을 달수 있습니다.

## 필요 한 것

 - Disqus 가입하기
 - Disqus 설정하기
 - Github 계정과 연동하기
 
---

## disqus 가입하기

[여기](https://disqus.com) 로 가서 disqus 에 회원가입 이 후, Verify email 을 인증 합니다.

---

## 1.

![screen](/img/posts/disqus.jpg)

회원 가입후, 오른쪽 상단의 사람모양 아이콘을 클릭후 `Settings`로 갑니다

---

## 2.

![screen](/img/posts/disqus1.jpg)

`Profile` 와 `Account` 로 가서 필요한것들을 작성해줍니다.

---

## 3.

![screen](/img/posts/disqus2.jpg)

우측 상단의 `Add Disqus To Site` 들어가서, Site 셋팅을 해줍니다.

![screen](/img/posts/disqus3.jpg)

`i want to install Disqus on my site` 로 들어갑니다.

---

## 4.

![screen](/img/posts/disqus4.jpg)

`install disqus` 부분으로 오게 되면, 화면을 아래로 내려서 `Universal Code` 를 선택합니다. 

---

## 5.

![screen](/img/posts/disqus5.jpg)

![screen](/img/posts/disqus6.jpg)



위의 코드를 복사 후에, github blog 가 생성되어있는 file로 갑니다.<br>
`_includes` 에 들어가서. `disqus_comments.html` 을 만들어서 위의 코드를 붙여 넣습니다.

> 주의: 모든 theme 들이 이런 방식으로 작동되는것은 아니지만 대부분 이런 형식으로 동작됩니다, 아니면 필살기로, 위의 코드를 post 마다 붙여넣어도 동작 하는것을 확인 하실수 있습니다..!

---

## 6.

![screen](/img/posts/disqus7.jpg)

`_config.yml` 에 들어가서 추가해줍니다. 



---

## 7. 결과 확인!

![screen](/img/posts/disqus8.jpg)

잘 작동이 되는지 확인해줍니다!

## Reference

[disqus 사용방법: hackya 님 블로그](https://hackya.com/kr/disqus-api-사용하는-방법/)








