---
layout:     post
title:      "Swift. Autolayout 사용하기 "
subtitle:   "Autolayout 으로 기본적인 IOS UI를 다각도로 구성하고 분석해보자! "
date:       2017-11-05 22:21:00
author:     "MinJun"
header-img: "img/tags/Swift-bg.jpg"
comments: true
tags: [Swift]
---


## 목차

1. **Constraints** <br>
2. **Constraints Multiplier** <br>
3. **Hugging-Priority & Content Compression resistance Priority** <br>
4. **Image resolution** <br>
5. **align** <br>
6. **Rotate** <br>
7. **ScrollView AutoLayOut** <br>
8. **margin** <br>

---

## Constraints

#### - Constraints <br>

객체에 제약조건을 주어서, `device` 의 크기가 변화해도, 정해놓은 제약조건을 값, 비율에 따라서 layout이 유동적으로 변하게 만들어 줄수 있습니다.

#### - align(정렬) <br> 

정렬은 단일 객체가 선택 되면, X,Y 의 Center 값만 설정 할수 있고, 여러개의 객체를 선택 하면 둘간의 관계에 따라서 정렬을 선택 해줄수 있습니다.
기본적으로 아이폰의 device 에서 왼쪽에서 오른쪽으로 갈수록 x의 값이 커지게 되고, y값은 위에서 아래로 갈수록 값이 커지게 됩니다.

```swift
item1.atrribute = 비율 * item2.atrribute + 간격 
```

위의 공식을 통해서, 오토레이웃은 감으로 하는게 아니라, 공식으로 할수 있다는것을 생각하면 설계하도록 하자.

---

## Constraints 

| update Frame | Reset to Seggest Constraints & Update Constraints Constants |
| :---: | :---: |
| ![screen](/img/posts/constraints.jpg) | ![screen](/img/posts/constraints-1.jpg) |

#### - update Frame  <br>

오토 레이아웃을 설정하고, 내가 마우스로 크기나 위치를 변경하거나, 어떤 값을 주었을때 변경된 값으로 에러 메세지가 나오는데, 그 상태에서 `update Frame` 을 하게 되면, 변하기 이전의 frame 상태로 돌아갑니다. 단축키는 `option + command + =`

#### -Reset to Seggest Constraints <br>

system 적으로 AutoLayOut을 제안해줍니다..

#### - update constraints <br>

AutolayOut 을 적용 시키고, 마우스로 크기나 위치를 변경하고, 어떤 값을 주고난 이후, update Constraints 를 하게되면, 변해있는 상태의값을 Constraints 값으로 변경 한다는 의미입니다..

--- 

## Constraints

#### - Label <br>

레이블은 기본크기가 텍스트에 따라서 잡혀 있다. 텍스트 사이즈에 따라서 Label 의 오토레이아웃을 지정할수 있습니다. <br>

*tip > 오른쪽 마우스를 누르고 객체를 선택하면 controll 누르고 선택하는것과 같은 효과를 볼수 있습니다.*
*tip > 옵션키를 누르고, 오브젝트를 누르게되면 오브젝트에 적용 되어 있는 Constraints 값을 볼수 있습니다.*


---

## Constraints Multiplier

| * | Red to grayView | Red to blue |
| :---: | :---: | :---: |
| ![screen](/img/posts/constraints-2.jpg) | ![screen](/img/posts/constraints-3.jpg) | ![screen](/img/posts/constraints-4.jpg) |

RedView를 기준으로, grayView애 1:2 비율이 적용되고, BlueView에 1:3 비율이 적용되었습니다. 따라서 RedView는 상대적으로 grayView는 redView의 2배 width를 가지게 되고, BlueView는 redView의 3배의 width를 가지게 됩니다.

```swift
item1.atrribute = 비율 * item2.atrribute + 간격 
```

> 위의 공식을 생각하면서 적용 해봅니다. 

#### - Align(정렬)

![screen](/img/posts/constraints-5.jpg) <br>

- 하나만 선택했을때는 ,가로 정렬, 새로정렬을 할수 있습니다. <br>

- 비율 정렬시, `0`은 작성할수 없음. -> 0과 비슷한 값 0.001.. 값을 입력해주면, piexle 의 오차가 조금 있을수 있지만, 거의 사용하지 않습니다.. 

---

## Constraints Multiplier


| * | 구름 View의 Constraints |
| :---: | :---: |
| ![screen](/img/posts/constraints-6.jpg) | ![screen](/img/posts/constraints-7.jpg) |

위같이, View에 상단에 반쯤 걸쳐 있는 View를 만들수 있는 상황이 있는데, 저렇게 표현하기 위한 방법은 여러가지가 있습니다. 구름 이미지가 GrayView의 자식 View로 들어가서 `AutolayOut` 을 적용해주어서 표현해줄수 있지만, 이런 경우는 좋은 경우가 아닙니다. 

이유는, 부모뷰의 범위를 넘어가게 되면, 일단 버튼은 눌리지도 않고, `Clip to bounds`를 부모뷰에서 선택하게 되면, 부모뷰를 넘어간 영역이 잘리게 됩니다.. 그래서 좋은 autoLayOut이 아닌 케이스가 됩니다. 동등한 계층 구조에서, align 을 사용해서 정렬을 해주면 올바르게 적용 할수 있습니다..

> 동등한 계층 구조에서 정렬을 하는게 좋다. 부모뷰의 범위를 넘어가면 click 도 되지 않는다..! 고로 AutolayOut을 설계할때, 기능적인 부분도 고려 해주어야 합니다.!

---

## Hugging-Priority

#### - Label <br>
	- 레이블 사이즈를 키워즈는 명령어 `edit->size to fit`, `command + =`, 텍스트의 크기 만큼 사이즈가 지정됩니다.
	- 레이블에 위치지정을 하게되면, 텍스트의 길이가 늘어남에 다라서 자동으로 레이블의 크기가 증가하게 됩니다.
	- 레이블의 Constraints 를 한쪽을 열어놓지 않고, 모두 적용하게 되면, Label의 사이즈는 고정되어 있게됩니다.
	- 기본적으로 텍스트의 크기에 따라서 Label의 사이즈를 유동적으로 만드려고 한다면, 셋팅을 해주어야 합니다.

#### - Content Hugging Priority

**Priority**는 우선순위 값 입니다. Label의 경우에 2개의 Label을 정의 해놓습니다.

| * | * | * |
| :---: | :---: | :---: |
| ![screen](/img/posts/constraints-8.jpg) | ![screen](/img/posts/constraints-9.jpg) | ![screen](/img/posts/constraints-10.jpg) |

왼쪽의 Label이 `Content Hugging Priority` 값의 `Horizontal` 부분이 더 큰값입니다. 우선순위 값이 높으면, 기본적인 Label의 사이즈가 변할때, 왼쪽에 있는 Label의 자신의 사이즈를 먼저 마추고, 그다음에 오른쪽에 있는 Label의 사이즈를 고려하게 됩니다. 그래서 왼쪽의 값이 1이 일때, 1234 일때 변하는 크기에 따라서 오른쪽 Label 위치가 변하게 됩니다. 

반대로 오른쪽의 레이블의 크기를 늘리게 되면, 이때 왼쪽 레이블의 크기는 고정이 되어서 변화되지 않고, 왼쪽 레이블의 크기만 변하게 되는데, 일정 범위를 넘어가게 되면 device 의 범위를 벗어나서 오류가 발생하게 됩니다. 

> 자신의 사이즈를 유지 하느냐 안하겠느냐에 대한 우선 순위를 설정한다고 생각하면 조금 편합니다..

#### - Content Compression resistance Priority

`Content Hugging Priority` 의 반대되는 개념입니다.<br>
위의 `Content Hugging Priority` 값이 높은 쪽은 자신의 초기 크기를 유지하게됩니다. 그러다가 두개의 Label의 크기가 device의 범위를 넘어가게될때 두개의 label 오류인상태로 늘어나지도, 줄어들지도 않게 되는데  `Content Compression resistance Priority` 값을 적용하게 되면, `Content Compression resistance Priority` 값이 높은 쪽의 Label이 `Content Compression resistance Priority` 값이 낮은쪽의 Label의 크기가 변화하게 되면, 높은 값을 가지고 있는 쪽은 찌그러지지(?) 않게 됩니다. 찌그러지는 값에 대한 정항 정도라고 생각할수 있습니다. 

> 모든 객체들의 Constraints 에는 Priority 의 값이 있습니다. 이 값을 잘 이용한다면, 조금 더 원하는 형태의 AutolayOut을 설계 할수 있습니다.
>
> 조금 햇갈린다면, Rect가 같거나 커질때는 `Hugging Priority`, Rect가 같거나 작아질때는 `Compression resitance`

---

## Image resolution

Image 는 기본적인 사이즈를 가지고 있습니다. ImageView에 imageView를 추가하고, 본래 이미지의 raw Size 로 돌려리면 `Command + =` 키룰 눌러주면 됩니다. 
이미지의 @2x, @3x 가 없으면, 이미지의 원래 픽셀 사이즈로 보여지게 됩니다, @2x 는 기본 아이폰 시리즈에서 사용이되는 사이즈이고, @3x는 아이폰 + 사이즈에서 사용이 되는 이미지입니다. 시스템적으로 자동으로 +버전, 기본 아이폰 버전 사용시 설정할수 있습니다. 


---

## align 

| * | Size inspector | atrribute inspector |
| :--: | :---: | :--: |
| ![screen](/img/posts/constraints-11.jpg) | ![screen](/img/posts/constraints-12.jpg) | ![screen](/img/posts/constraints-13.jpg) |

버튼 타이틀의 정렬을 주어야 하는경우, image를 버튼왼쪽, 오른쪽에 놓고, title의 인셋을 조정할수 있습니다. `Size inspector` 부분에서, button의 `inset` 설정할수 있는데, `atrribute inspector`에 정렬과 논리적으로 맞아야 적용이 됩니다. 예를 들어서, `atrribute inspector`에서는, 왼쪽 정렬을 해놓고, `inset` 은 오른쪽 값을 주게 되면, 인셋값을 주나 마나인 경우가 됩니다..!

`Content Insets`은 `title insets` 와 `image insets` 둘다 적용 시킬수 있습니다. 기본적으로 둘다 적용 시키는 `Content Insets`를 많이 사용합니다.

---

## Rotate

| Portrait | LandScape |
| :--: | :---: |
| ![screen](/img/posts/constraints-14.jpg) | ![screen](/img/posts/constraints-15.jpg) |

| Vary for Traits | 
| :--: | 
| ![screen](/img/posts/constraints-16.jpg) | 
| ![screen](/img/posts/constraints-17.jpg) |

화면의 상태 Portrait(기본), LandScape(가로모드) 에 따라서 적용되는 AutoLayOut을 설정할수 있습니다.

1. `Vary for Traits` 을 누릅니다. `height`, `Witdh` 에 따라서 적용되는 모드를 찾아볼수 있습니다.

2. `height` 를 선택하면, LandScape 에 적용되는 모델들을 확인할수 있습니다.

3. 변경될 AutoLayOut 을 지정합니다. Constratins 값들을 변경한 후 `Done Varying` 을 누르면, 해당 모델에 LandScape 모드, 혹은 Portrait 모드에서 AutoLayout이 적용 되는것을 확인할수 있습니다. 



---

## ScrollView AutoLayOut 적용 

ScrollView의 AutoLayOut을 적용하는 경우는, ContentsView의 크기를 내부의 Contents의 개수, 혹은 크기에 따라서 유동적으로 적용하고 싶을때 사용할수 있습니다.

*Storyboard 로 ScrollView를 정의하는 경우에 ScrollView -> View를 올리게 되면, ContentsView로 인식을 하게됩니다. 그래서 크기값을 지정해주지 않으면, LayOut 오류라는 메세지를 출력합니다. *

| ScrollView | Constraints | bottom Constraints |
| :--: | :---: | :--: |
| ![screen](/img/posts/constraints-18.jpg) | ![screen](/img/posts/constraints-19.jpg) | ![screen](/img/posts/constraints-20.jpg) |

스크롤뷰를 정의하고 내부에 TextFiled 를 올려주었습니다. 이때 ContentsView의 bottom 부분의 Priority 값을 700 정도로 낮추어 줍니다. 그렇게 되면, ContentsView의 bottom 부분이 내부의 Contents 의 사이즈에 따라서 유동적으로 변하게 됩니다. 이런식으로 ContentsView의 Scroll을 유동적으로 정의해줄수 있게 됩니다.

---

## margin

AutoLayOut 에서 `margin` 은 조금 애매한 기능입니다. <br>

1. 가장 상위뷰를 기준으로 `margin` 을 셋팅하면 왼쪽과, 오른쪽에만 margin 이 셋팅이 됩니다.
2. 그 아래의 계층에 있는 View는 왼쪽, 오른쪽, 위, 아래 모두 8 -> 그 다음계층에서 margin 을 적용하게 되면 16.. 씩 적용이 됩니다(+ 모델의 경우도 동일하게 적용이 됩니다.)
3. `landscape` 일때도, 8, 16... 씩 margin 이 적용 됩니다. 
4. 아이템과 아이템 사이는 마진이 적용이 되지 않습니다.(모든 아이템에 마진을 적용해도, 시스템상에서 마진 적용을 무시합니다)

![screen](/img/posts/constraints-21.jpg)

---

## 여담 

AutoLayout을 다시 공부하니까, storyboard 가 생각보다 강력하다는것을 알게 되었습니다. code로만 작업할때는 추상적으로 생각했던 내용들을 Storyboard로 명확하게 작업하게 되니까, 시간도 절약되게 되고, 눈으로 보니까 조금더 명확하게 작업할수 있는것 같습니다.

---

## Reference 

[Autolayout guide](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/)<br>
[IOS Autolayout 강의](https://www.inflearn.com/course/autolayout-ui_ios/)




