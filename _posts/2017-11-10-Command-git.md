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

## 알아두면 좋은 명령어

* <br>

| Command | 내용 | 
| :----------: | :----------: |
| git config --global user.name [user name] |   작업자 이름 설정 |
| git config --global user.email [user email]|   작업자 이메일 설정 |
| git config --global --list | 설정값(이름 및 메일등) 확인 |
| git init |  git 저장소(repo) 만들기 | <br>

* <br>
 
﻿| Command | 내용 | 
| :----------: | :----------: |
| git remote add [remote name] [remote addres] | 별명으로 원격지주소를 저장 |
| git remote rm [remote name]   |   별명의 원격지를 삭제 |
| git remote rename [remote name] [new name]  | 별명을 새로운 별명으로 변경 | 
 
* <br>
 
| Command | 내용 | 
| :----------: | :----------: |
|git fetch [remote name]    | remoet의 모든 정보를 가져옴(모든 branch) |
| git pull |  저장소에서 변경 내용 가져오기 |

* <br>

| Command | 내용 | 
| :----------: | :----------: | 
| git push | commit들을 master 저장소에 저장﻿ |
| git push [remote name] [localbranch name] |local branch의 내용을 업데이트|
| git push [server] tag [TAG]    |                              server에 tag 전송|
| git push [server] --tags      |                               변경된 모든 tag 전송|
| git push [server] [L.B]:[R:B] |                               server 에 local branch 를-Remote branch이름으로저장 |
 
* <br>                                                                              

| Command | 내용 | 
| :----------: | :----------: |  
| git tag [TAG NAME] |                                              저장소에 태그를 붙인다. |
| git tag             |                                                     태그목록을 본다.|
| git branch [branch name] |                                   저장소의 branch name으로 branch를 만든다. |
| git branch                |                                           branch 목록을 본다.|
| git branch -a              |                                        현재 생성된 모든 local branch와 reomte branch 확인|
                                                                             
* <br>
 
|git checkout [branch name]                     |         다른 브랜치로 전환 |
|git checkout -b [branch name]  |                       branch 생성 |
|git checkout [file or folder]  |                           git repo 기준 마지막 commit 상태로 돌림 |
|git checkout [id] [file or folder]      |               git repo 기준 id에 해당하는 commit 상태로 돌림 |
|git checkout -f      |        아직 commit 되지 않은 working tree와 -index 수정정사항 모두 사라짐 |
 
* <br>
 
|git merge [branch name]                        |         branch의 내용을 가져와 합침|
|git add [file or folder]                        |           git에 file 또는 folder 추가|
|git add *                                        |                   git에 모든 file 또는 folder 추가|
|git rm [file or folder]                           |          git 파일 또는 폴더 제거|
|git status                                         |                현재 git 상태 보기|
|git commit -m [message]   |                             message를 repo에 저장 |
|git diff                   |                                          local과 remote의 차이점을 보여줌 |
|git remote                  |                                      remote서버 확인 |

---

## 스테이시(`stash`)에 안전하게 보관하기

어떤 브랜치에서 파일을 수정하거나 추가한 후 커밋하지 않은 상태에서 다른 브랜치로 체크아웃할 경우 아래와 같이 오류메시지를 보게 된다.

```git
$ git checkout master
error: Your local changes to the following files would be overwritten by checkout:
        00_topsection/css/meritz.css
Please commit your changes or stash them before you switch branches.
Aborting
```

커밋을 하고 체크아웃하면 되겠지만 작업 도중이고 아직 완료가 되지 않은 상태에서 갑작스런 핫픽스 요청이 들어왔을 때 안전하게 저장하기 위해서 stash를 사용할 수 있다. 현재 진행중이던 내용들을 언제든지 git stash로 저장해두고 다른 브랜치로 이동하여 작업한 뒤에 다시 돌아와 복구하여 작업을 계속할 수 있다.
또 저장한 내용을 다른 브랜치로 옮기는 것도 충분히 가능하다. (아마 잘못된 브랜치에서 작업중이었음을 뒤늦게 알았을 경우)
안전하게 보관한 뒤 복구하기


```
$ git status
On branch feature-meritz
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   css/meritz.css
```
        
위와 같이 아직 작업중인 meritz.css 파일이 있다고 하자. 아직 인덱스에 추가하지도 커밋하지도 않은 상태에서 다른 브랜치로 체크아웃을 해야 한다면 다음과 같이 안전하게 저장해 두자

#### `git stash` : 스테이시로 안전하게 보관

```
$ git stash
Saved working directory and index state WIP on feature-meritz: 629732d update css
HEAD is now at 629732d update css
```

작업 디렉토리와 인덱스 상태가 저장되었다고 나온다. 그리고 HEAD는 작업 커밋 상에 있음을 알려준다. git status 명령을 실행하면 meritz.css의 변경사항이 없어진 걸 알 수 있다. 이제 다른 브랜치로 체크아웃 할 수 있다.+

#### `git stash list` : 스테이시 목록 조회

```
$ git stash list
stash@{0}: WIP on feature-meritz: 629732d update css
```

`git stash` 하위 명령으로 list를 실행하게 되면 현재 stash area에 저장되어 있는 변경사항들을 모두 조회 가능하다. 목록 앞에 보이는 stash@{0}는 stash ID로 각각의 저장 내용을 구별짓는 번호이다. stash는 스택 방식으로 동작한다. 가장 최근에 저장한 것이 가장 먼저 나오게 된다.

#### `git stash pop` : 저장내용을 복구

```
$ git stash pop
On branch feature-meritz
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   css/meritz.css
```

        
`git stash pop` 명령을 통해 저장내용을 현재 브랜치에 적용할 수 있는데 이때 git status 결과를 함께 보여준다. 그리고, 스테이스 목록에서도 제거된다.

#### `git stash apply` : stash에 저장된 내용을 다른 브랜치에 적용

```
$ git stash apply
On branch hotfix
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   css/meritz.css
```

        
git stash apply 명령은 git stash pop와 비슷하면서도 다르다. 일단 현재 브랜치에서 저장된 내용을 적용하는 것은 동일하다. 하지만 적용된 저장된 내용을 stash 목록에서 drop하지는 않는다. 그러므로 이를 이용하면 여러 브랜치에 저장된 내용을 적용하는 것이 가능하다.

#### `git stash drop` : stash에 저장된 내용 삭제

```
$ git stash drop stash@{0}
Dropped stash@{0} (629732d629732d629732d629732d)
```

git stash drop 명령은 특정 stash를 삭제해 준다. stash ID를 명시하지 않으면 pop과 같이 나중에 저장된 내용이 먼저 삭제된다.

---

## 자주 사용하는 명령어

| command | 내용 |
| :----: | :----: |
| git log | commit 이력 확인|
| git diff | 원격과 로컬의 다른점을 보여줍니다 |
| git check -b <branch 이름> | <branch 이름>의 branch 가 없으면, brnach 생성후, 해당 branch 로 이동합니다  |
| git add -p | stage로 date를 보낼때 무엇이 변했는지 확인할수 있습니다.|
| git commit -v | commit 단계에서, 무엇이 변했는지 확인할수 있습니다.|

---

## Reference 

[git 생활코딩](https://mylko72.gitbooks.io/git/content/)<br>
[git 입문](https://backlog.com/git-tutorial/kr/stepup/stepup3_1.html) <br>



