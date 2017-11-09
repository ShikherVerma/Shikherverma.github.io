---
layout:     post
title:      "Git Fetch, Pull의 차이점을 알아보자"
subtitle:   "Git의 유용한 명령어들을 정리해보자!"
date:       2017-10-29 19:09:00
author:     "MinJun"
header-img: "img/tags/Github-bg.jpg"
comments: true
tags: [Github]
---





## 알아두면 좋은 명령어

| Command | 내용 | 
| :----------: | :----------: |
| git config --global user.name [user name] |   작업자 이름 설정 |
| git config --global user.email [user email]|   작업자 이메일 설정 |
| git config --global --list | 설정값(이름 및 메일등) 확인|

git init                                                                git 저장소(repo) 만들기
 
﻿
git remote add [remote name] [remote addres]  별명으로 원격지주소를 저장
git remote rm [remote name]                             별명의 원격지를 삭제
git remote rename [remote name] [new name]   별명을 새로운 별명으로 변경
 
git fetch [remote name]                                     remoet의 모든 정보를 가져옴(모든 branch)
 
git pull                                                                저장소에서 변경 내용 가져오기
 
git push                                                                 commit들을 master 저장소에 저장﻿
git push [remote name] [localbranch name] local branch의 내용을 업데이트
git push [server] tag [TAG]                                  server에 tag 전송
git push [server] --tags                                     변경된 모든 tag 전송
git push [server] [L.B]:[R:B]                                server 에 local branch 를
                                                                               -Remote branch이름으로저장
 
git tag [TAG NAME]                                              저장소에 태그를 붙인다.
git tag                                                                  태그목록을 본다.
git branch [branch name]                                   저장소의 branch name으로 branch를 만든다.
git branch                                                           branch 목록을 본다.
git branch -a                                                      현재 생성된 모든 local branch와
                                                                             reomte branch 확인
 
git checkout [branch name]                              다른 브랜치로 전환
git checkout -b [branch name]                         branch 생성
git checkout [file or folder]                            git repo 기준 마지막 commit 상태로 돌림
git checkout [id] [file or folder]                     git repo 기준 id에 해당하는 commit 상태로 돌림
git checkout -f              아직 commit 되지 않은 working tree와 -index 수정정사항 모두 사라짐
 
git merge [branch name]                                 branch의 내용을 가져와 합침
git add [file or folder]                                   git에 file 또는 folder 추가
git add *                                                           git에 모든 file 또는 folder 추가
git rm [file or folder]                                     git 파일 또는 폴더 제거
git status                                                         현재 git 상태 보기
git commit -m [message]                                message를 repo에 저장
git diff                                                             local과 remote의 차이점을 보여줌
git remote                                                        remote서버 확인
