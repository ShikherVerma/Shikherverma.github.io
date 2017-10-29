---
layout:     post
title:      "Git 을 이용한 WorkFlow"
subtitle:   "Git을 이용해서 협업을 해보자!"
date:       2017-10-29 19:09:00
author:     "MinJun"
header-img: "img/tags/Github-bg.jpg"
comments: true
tags: [Github]
---

## Git을 이용한 협업 Workflow 

1. Centralized Workflow

2. Feature Branch Workflow

3. Gitflow Workflow

4. Forking Workflow

---

## 1. Centralized Workflow


![screen](/img/posts/gitworkflow.jpg)

Git으로 협업 환경을 전환하는 것은 굉장히 어려워 보이지만, 지금 소개하는 Centralized Workflow는 사실 기존의 Subversion(SVN)으로 협업할 때와 크게 다를 바 없다. <br>

SVN에 비하면 Git은 다음 장점이 있다. 첫째, 모든 팀 구성원이 로컬 저장소를 이용해서 개발한다는 점이다. 로컬 저장소는 중앙 저장소로 부터 완벽히 격리된 상태이므로, 다른 팀 구성원 및 중앙 저장소의 변경 내용을 신경 쓰지 않고 자신의 작업에만 집중할 수 있다. <br>

둘째, Git의 브랜치와 병합 기능의 이점을 들 수 있다. Git 브랜치를 이용하면 안전하게 코드를 변경하고 다른 브랜치에 통합할 수 있다.

> 중앙 저장소는 한개로 두고, 작업자들 각각 중앙 저장소의 내용을 받아서, 작업 -> push 순으로 작업을 합니다. 이때 각자 작업의 시점차이 때문에 충돌이 발생할수 있는데, 이때는 중앙 저장소에서 push 를 받아주지 않습니다. 그때는 현재 작업한것과, 중앙 저장소의 내용을 pull 한후, merge 하여 다시 push 해줍니다.
> 
> 형상관리의 발전 순서가, CVS->CSN 이라고 합니다.

---

## 2. Feature Branch Workflow

![screen](/img/posts/gitworkflow-1.jpg)

Feature Branch Workflow의 핵심 컨셉은 기능별 브랜치를 만들어서 작업한다는 사실이다. 기능 개발 브랜치는 격리된 작업 환경을 제공하기 때문에 다수의 팀 구성원이 메인 코드 베이스(master)를 중심으로 해서 안전하게 새로운 기능을 개발할 수 있다. 따라서 master 브랜치는 항상 버그 프리 상태로 유지할 수 있어, 지속적 통합(Continuout Integration)을 적용하기도 수월하다. 또, 풀 리퀘스트를 적용하기도 쉽다. <br>

> 기능별로 branch 를 만들어서, 통합하기전 `pull requests` 하여 안전하게 중앙 저장소에서 merge 하며 개발합니다.
> 
> 작동원리는 Gitflow Workflow도 팀 구성원간의 협업을 위한 창구로 중앙 저장소를 사용한다. 또 다른 워크플로우와 마찬가지로 로컬 브랜치에서 작업하고 중앙 저장소에 푸시한다. 단지 브랜치의 구조만 다를 뿐입니다.
> 
> 


---

## 3. Gitflow Workflow

![screen](/img/posts/gitworkflow-2.jpg)

> Gitflow Workflow는 코드 릴리스를 중심으로 좀 더 엄격한 브랜칭 모델을 제시한다. Feature Branch Workflow보다 복잡하긴하지만, 대형 프로젝트에도 적용할 수 있는 강건한 작업 절차다.
> 
> 작동 원리 는 Gitflow Workflow도 팀 구성원간의 협업을 위한 창구로 중앙 저장소를 사용한다. 또 다른 워크플로우와 마찬가지로 로컬 브랜치에서 작업하고 중앙 저장소에 푸시한다. 단지 브랜치의 구조만 다를 뿐이다.
> 
> Gitflow는 크게 `Develop`,`Release` 브랜치를 사용한다. Develop 은 항상 최신의 상태로 유지하고, 각각 개발완료 버전에 따라서 Release에서 v.1 - v.1.1 - v.1.2... 로 나 눠지면서 관리되어집니다.


---


## 4. Forking Workflow

 OpenSource 에서 가장 많이 사용하는 방식 입니다. 누군가 만들어 놓은 repo를 fork 떠가고 난후, 수정, 보완 사항을 pull requests 해서 반영할수 있는 방식입니다. 
 
 
---

## Git 유용한 명렁어 

커밋 취소하기 <br>

```
git reset HEAD^
```

feature_x 라는 branch 가 없으면 생성하고 feature_x brnach 로 이동합니다 <br>

```
git checkout -b feature_x
```

branch 삭제<br>

```
git branch -d feature_x
```

새로만든 branch 를 원격 저장소로 전송하기 전까지는 다른사람들이 접근할수 없습니다 <br>

```
git push origin <branch_name>
```

Git의 내장 GUI <br>

```
gitk
```

Git의 log 이력 확인

```
git log
```

log 이력과 변경 사항 표시

```
git log -p
```

git add, commit 시에 변경이력 확인 을 좀더 세부적으로 할수 있습니다

```
git add -p
git commit -v
```

branch의 지역과, 원격 브랜치 보기

```
git branch -r
```

---

## 여담

오늘 갑자기 좋은 개발자란 무엇일까? 하는 고민거리를 가지고 생각해보았습니다.  <br>

개인적인 생각으로 `좋은 개발자`란, 함께 일하고 싶은사람 이 아닐까 하고 생각합니다. 아, 저분 하고 한번 같이 작업 해보고 싶다. 라는 생각을 떠올리게 만드는 개발자가 `좋은 개발자` 라고 생각합니다.
 
어떤 사람들이 같이 일하고 싶다는 생각을 하게 만들까? 라는 고민을 해보면, 배울점이 있는분(포괄적인 의미에서), 열정적이고, 무언가를 시작하면 즐기는분...등등 이유는 많이 있는것 같습니다. 본질적으로 함께 무언가를 했을때 즐겁게 만들어 준다면, 진짜 좋은 개발자가 아닐까...하고 생각해보면,  

그런 의미에서 저는 한~~참 멀리 있는것 같다는 생각이 드네요...😭 그냥 카페에서 심심해서 끄적여 봅니다...

---

## Refrence 

#### [appkr.memo 님 blog](http://blog.appkr.kr/learn-n-think/comparing-workflows/) <br>

#### [Git 명령어 모음](https://medium.com/@joongwon/git-git-명령어-정리-c25b421ecdbd) <br> 
