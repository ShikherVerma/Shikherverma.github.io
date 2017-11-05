---
layout:     post
title:      "Xcode. CocoaPods 사용법 "
subtitle:   "라이브러리를 의존성 관리도구인 CocoaPods를 이용해서 라이브러리를 관리하자!"
date:       2017-10-29 19:10:00
author:     "MinJun"
header-img: "img/tags/Swift-bg.jpg"
comments: true
tags: [Swift]
---

1. CocoaPods 란?
2. CocoaPods 설치
3. pod 찾아보기 


---

## CocoaPods 란?

IOS 등 애플의 개발 플랫폼을 이용하다 보면 외부 라이브러리를 사용해야 하는 경우가 생기는데, 이때 외부 라이브러리 들을 쉽게 관리해줄수 있는 의존성 관리도구의 일종입니다.

---

## CocoaPods 설치 하기

- 설치 순서

1. Xcode 로 Project 생성 
2. 생성된 project 폴더로 이동하여(터미널로)
3. podfile 을 생성해준다.
4. pod 설치 

---

## CocoaPods 시작하기 


* Xcode 로 Project 생성 <br>
* 생성된 project 폴더로 이동하여(터미널로)

``` 
sudo gem install cocoapods

*제거할때 
sudo gem uninstall cocoapods
```

* podfile 을 생성해준다.

```
pod init 
```


| podfile | 사용할 podfile |
| :----: | :----: |
| ![screen](/img/posts/cocoapod.jpg) | ![screen](/img/posts/cocoapod-1.jpg) |

> 위의 명령어를 터미널에서 작성하면, `podfile`이 생성 됩니다.  `vi podfile` 을 통해서 pod file 을 들어가면, cocoapods의 라이브러리 들을 추가할수 있습니다. 


[https://cocoapods.org](https://cocoapods.org) 가서 사용하고싶은 라이브러리를 검색후, 위의 이미지처럼 `end` 위에 podfile 을 복붙해서 사용하고 싶은 라이브러리를 추가한후.

* pod 설치 

```
pod install
```
---

## 여담 

라이브러리 마다 사용방법이 모두 달라서, 설치후 에러가 발생할수 있습니다. 그때를 구글신이나... 해당 라이브러리의 github 으로 가서 이슈를 확인해보시면 같은 증상을 겪으신분들이 해결 하신 내용을 찾아볼수 있습니다.


---

## Reference 

[야곰님 블로그](http://blog.yagom.net/534)<br>
[네이버 D2](http://d2.naver.com/helloworld/444849)<br>
[CocoaPods](https://github.com/CocoaPods/CocoaPods/wiki) <br>
[https://cocoapods.org](https://cocoapods.org)