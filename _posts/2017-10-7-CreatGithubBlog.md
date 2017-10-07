---
layout:     post
title:      "Github Blog 빠르게 만들기"
subtitle:   "Github Page 이용해서, 나만의 blog 만들기"
date:       2017-10-07 12:04:00
author:     "MinJun"
header-img: "img/tags/Github-bg.jpg"
comments: true
tags: [Github]
---


![main](/img/posts/github-pages.png)


아무것도 모르고 Github Blog 를 시작 하고 손댔다가, 약 3주 정도 시간 동안 고생하게 되었습니다. 그 경험을 바탕으로 가장 빠르게 Github Blog 를 셋팅하고, 이용 하는 방법을 알아보겠습니다.


---

## 필요 한 것

빠르게 시작 하려면, 무엇이 필요한지 먼저 알아야 합니다.

1. Blog Themes 설정
 
2. Github ID 

3. Github Page 

4. jekyll의 구조 에 대한 이해 

5. markDown 사용법

---

## Blog 테마 설정 

**[여기](https://github.com/jekyll/jekyll/wiki/themes)** 에 들어 가면 내가 적용 할 blog 의 Themes을 확인 할수 있습니다.(blog의 테마를 고른다고 생각하시면 됩니다!)


![screen](/img/posts/CreatGithubblog.jpg)

마음에 드는 Theme 를 선택후, demo 부분을 눌러서 확인 해봅니다(저는 여기에서 시간이 제일 많이 걸렸습니다..) 

> 주의사항: 저는 마음에드는 여러가지 Theme을 적용 해보았는데, 처음에는 멋지고 화려한 theme 보다, 심플한 theme 을 사용 하는 것이 정신건강에 좋다는걸 알았습니다.. 물론 멋진 theme 을 하고 싶지만, 멋진 theme 일수록 구조가 복잡해서 아무것도 모르고 이것저것 만지다보면 예상하지 못한 에러들이 튀어나오는데, 멘붕의 서막 시작입니다..(HTML, CSS, Javascript 에 대한 이해가 있으신분은 예외입니다)


---

## GitHub Blog Page 생성 하기 



![screen](/img/posts/CreatGithubblog1.jpg)

저는 예시로 `trophy` theme 을 선택 했습니다. 


---

# 1.

![screen](/img/posts/CreatGithubblog2.jpg)

이제 theme 을 선택했으니, Github blog Page 생성 하는 방법을 알아보겠습니다.

아까 theme 선택하는 부분에서, `source` 부분을 들어갑니다

---

# 2.

![screen](/img/posts/CreatGithubblog3.jpg)

이미지 상단의 우측 부분에 'fork' 를 클릭합니다

> fork 는 나의 나의 github 계정으로, 다른 사람이 만들어 놓은 source 를 가져오는 과정입니다. 

---

# 3.

![screen](/img/posts/CreatGithubblog4.jpg)

`fork`를 했으면, 내 github 계정으로 가서, 잘 fork 가 되었는지 확인합니다. 잘 `fork`가 되었다면 자신의 이름으로 `fork`가 된것을 확인할수 있습니다. 

이제 홈페이지 중단부의 Code Pull Requeste Projects .... 부분에서 Settings 부분을 들어갑니다.

---

# 4.
![screen](/img/posts/CreatGithubblog5.jpg)

Settings 부분을 들어가면, 아래같은 화면을 볼수 있는데, `Repository name` 을 github 계정 이름.github.io로 바꿉니다. 현재 자신의 github 계정의 이름이 궁금 하면, 아래 화면의 왼쪽 상단 부분 보시면 `나의github계정 이름/trophy-jekyll` 을 확인 하실수 있습니다.

> 위의 github계정이름.github.io가 실제 blog의 주소가 됩니다. 이게 마음이 들지 않다면 `CNAME` 을 이용해서 주소를 바꿀수 있습니다. 이 내용은 이 post에서는 다루지 않겠습니다!


---

# 5.
 
![screen](/img/posts/CreatGithubblog6.jpg)

여기까지 했으면 절반 완성했습니다. 이제는 자신의 홈페이지를 어떻게 커스텀 할것인지 정해야하는데
github blog Theme 은 거의 비슷한 구조를 가지고 있습니다.

 - README.md : 만들어진 github blog 마다 사용방법이 조금씩 다릅니다. 홈페이지에 대해서 친절하게 설명이 되어있는것도 있지만, 그렇지 않은것도 있습니다. 하지만 기본적으로 읽어주고 시작 합니다. 그러면 조금더 빠르게 theme 에 대한 이해를 할수 있습니다.

 - _config.yml : 환경설정 정보를 보관합니다. 기본적인 설정은 대부분은 이곳에서 합니다
 
 - _includes : 재사용하기 위한 파일은 담는 곳 입니다. 포스트나, 레이아웃을 손쉽게 편집할수 있는 녀석들을 담아 놓는 곳입니다.(모든 github blog theme 이 다 이런 구조는 아니지만, 대략적으로 이런 용도로 사용한다라고 생각하시고 읽어주시면 좋을것 같습니다!)

 - _layouts : github page의 포스트별로 적용되는 code 들이 있는곳입니다. post의 모습을 편집하고 싶으시다면, 해당 디렉토리에 들어가서 살펴 보시면 될것 같습니다

 - _index.html : 가장 기본적인 부분입니다. 이부분에서 homepage를 그려준다(?) 라고 생각하시면 됩니다.(엄밀하게 이 부분에서 그려주는것은 아니지만, 처음에는 그렇게 이해하시면 조금 편합니다)

 - _posts: github blog 에서 글을 포스팅 할때 이 디렉토리에 markdown 파일을 생성 해서 작성을 합니다. 주의해야할 사항은, markdown 으로 작성한 글이 blog에 올라가려면 어떤 형식을 따라줘야 합니다 예를 들어 서 파일이름은 `YEAR-MONTH-DAY-title.MARKDOWN` 형식을 사용하고, 내부에서 글을 작성할때 작성해야하는 것들이 있는데, 이러한 설명들은 대부분 README.md 에 설명이 되어있습니다.


> 자 이제 여기 까지 읽었다면, 선택한 theme의 README.md 를 읽고 -> _config.yml 에 들어가서 기본 설정을 하고 `https://github이름.github.io`로 들어가서 확인해봅시다!
> 
> 주의 사항으로는, 이녀석들을 설정하고 `git add > git commit > git push` 과정까지 해주어야 합니다. 위의 과정을 마치고 약 5분 정도 이후에 들어갔을때, 홈페이지가 보인다면 성공, 그렇지 않다면, 어딘가 잘못 작성이 되었을 것입니다..!(혹은 git push 이후에 홈페이지가 바로 생성이 되지 않는 경우도 있습니다. github 과 github page간의 시간차(?) 가 생길수도 있는데 심한 경우 post를 하나 작성해놓고, 하루가 지나고 확인하면 post가 되는 경우도 있습니다...! 

---

## MarkDown 사용방법

[MarkDown 문법 설명이 잘되어 있습니다.](https://gist.github.com/ihoneymon/652be052a0727ad59601)

[MarkDown 웹 에디터 입니다](https://stackedit.io/editor)

[제가 사용하고 있는 MarkDown 에디터 MacDown 다운로드 링크입니다](http://d.pr/f/YeYkOQ?itm_campaign=directdownload&itm_content=views1000)

---


# Reference

[github page 만들기: 태환님 블로그](http://thdev.net/653)

[jekyll Documents](https://jekyllrb.com/docs/home/)

[git, github 사용방법](http://rogerdudler.github.io/git-guide/index.ko.html)
























