---
layout:     post
title:      "Git Fetch, Pull의 차이점을 알아보자"
subtitle:   "Git의 유용한 명령어들을 정리해보자!"
date:       2017-11-10 00:30:00
author:     "MinJun"
header-img: "img/tags/Github-bg.jpg"
comments: true
tags: [Github]
---

## fetch 

`Fetch` 는 원격 저장소의 데이터를 로컬에 가져오기만 합니다.(이게 무슨이야기 인가 하면) `pull` 을 실행하면, 원격 저장소의 내용을 가져와 자동으로 병합 작업을 실행하게 됩니다.(pull을 하게되면, 자동으로 merge 도 함께 실행하게 됩니다.) 그러나 단순히 원격 저장소의 내용을 확인만 하고 로컬 데이터와 병합은 하고 싶지 않은 경우에는 fetch 명령어를 사용할 수 있습니다.

`fetch` 를 실행하면, 원격 저장소의 최신 이력을 확인할 수 있습니다. 이 때 가져온 최신 커밋 이력은 이름 없는 브랜치로 로컬에 가져오게 됩니다. 이 브랜치는 `FETCH_HEAD`의 이름으로 체크아웃 할 수도 있습니다.

예를 들어, 로컬 저장소와 원격 저장소에 B에서 진행된 커밋이 있는 상태에서 fetch 를 수행하면 아래 그림과 같이 이력이 남겨집니다.

![screen](/img/posts/gitlog.jpg)

이 상태에서 원격 저장소의 내용을 로컬 저장소의 'master'에 통합하고 싶은 경우에는, 'FETCH_HEAD' 브랜치를 merge 하거나 다시 pull 을 실행하면 됩니다.

![screen](/img/posts/gitlog-1.jpg)

---

## Pull 

`Pull` 은 원격저장소에 저장되어있는 소스들을 가져와, 자동으로 merge 하게 됩니다. 이때 현재 Local 의 date와 충돌이 없으면 자동으로 merge가 되지만, 그렇지 않은 경우 충돌이 발생하고, 그 충돌을 해결 해주는 merge 과정이 필요합니다.

![screen](/img/posts/gitlog-2.jpg)

---


## Reference 


[git 입문](https://backlog.com/git-tutorial/kr/stepup/stepup3_1.html) <br>



