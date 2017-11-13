---
layout:     post
title:      "Swift. Autolayout 을 이용한 동적 text 만들기  "
subtitle:   "Autolayout을 통해서 Label을 동적으로 사용해보자! "
date:       2017-11-13 17:49:00
author:     "MinJun"
header-img: "img/tags/Swift-bg.jpg"
comments: true
tags: [Swift]
---

## Dynamic_Text

|--view <br>
|----imageView <br>
|----image <br>

> 이미지속에 작성될 텍스트를, image의 크기에 따라서 동적으로 변경되게 적용 하려고 합니다..!
>
> View, Imageview에 autoLayout을 적용해서, 모든 디바이스에 원하는 형태로 대응할수 있게 만들어 줍니다. <br>

|--View <br>
|----ImageView를 넣어줍니다.  <br>
 
| * | Aspect Fit |  
| :---: | :---: | 
|  ![screen](/img/posts/Dynamic_Text.png) |  ![screen](/img/posts/Dynamic_Text-1.png)|

  
 - View의 Constraints 는, 좌,우 20, 센터 정렬을 해줍니다(top,bottom 은 지정하지않습니다) <br>
 - imageView의 Constraints 응 상,하,좌,우 0 으로 빽빽하게 적용 하고 image를 넣어줍니다. <br>

> 이렇게 적용하게 되면 image의 pixels 크기로 imageView가 사이즈를 자동으로 변경하게 됩니다. 이때 `Aspect Fit` 을 해주게되면, 보여지는 image는 device 사이즈의 가운데로 오지만, imageView의 CGSize 는 image의 원본 사이즈를 유지하게 됩니다.  
> 


| imagePixeld | Aspect Ratio | multiplier | imageView 사이즈 확인|
| :---: | :---: | :---: | :---: | 
|  ![screen](/img/posts/Dynamic_Text-2.png) |  ![screen](/img/posts/Dynamic_Text-3.png)|   ![screen](/img/posts/Dynamic_Text-4.png) |  ![screen](/img/posts/Dynamic_Text-5.png)| 
 
- 디바이스에 보여지는 크기로 만들어 주기 위해서는 이미지의 픽셀 크기를 비율로 imageView의 사이즈를 정의해주면, 디바이스에 보여지는 사이즈의 비율로, 이미지의 비율을 유지할수 있습니다.

> 이미지에 label을 넣어서, 그 레이블과 이미지의 사이즈, 디바이스 변화에 따라서 레이블의 font 값, 위치를 변경하기 위해서 정렬과 멀티 플라이어를 사용해줍니다..
> 

| imageView와 Label선택후, 정렬 지정 | bottom Multiplier 변경 | 확인 | 
| :---: | :---: | :---: | 
|  ![screen](/img/posts/Dynamic_Text-6.png) |  ![screen](/img/posts/Dynamic_Text-7.png)|   ![screen](/img/posts/Dynamic_Text-8.png) |  

> 디바이스 사이즈에 따라서 label의 위치는 고정되어 있습니다. 하지만 Label의 Font size는 초기에 설정한 크기가 그대로 고정 되어 있어서, 아직 완벽하게 동기화 된것은 아닙니다.
> 

| superView 와 Aspect Ratio 선택  | autoshrink -> Minimum Font Scale | 텍스트 Line 가 1이상인경우 |  목적 |
| :---: | :---: | :---: | :---: | 
|  ![screen](/img/posts/Dynamic_Text-9.png) |  ![screen](/img/posts/Dynamic_Text-10.png)|   ![screen](/img/posts/Dynamic_Text-11.png) |  ![screen](/img/posts/Dynamic_Text-12.png) |  


Label의 Font Size 는 Label의 넓이에 따라서 유동적으로 변하게 해줄수 있습니다. 그렇게 하기 위해서, Label의 Aspect Ratio 를 superView와 같게 설정해줍니다. 이렇게 설정하게 되면, label의 넓이가 디바이스 사이즈에 따라서 비율로 정의되게 됩니다. 비율로 넓이가 결정되면 font Size로 유동적으로 설정 해줄수 있습니다. 

Line이 1 이 아닌 경우에는, 라인을 '0' 으로 지정 하게 되면, Label의 넓이가 변할때, 라인을 넘겨주어서 처리할수 있으면, 라인을 넘겨주어서 처리하는것을 우선으로 실행합니다. 그것을 방지 하기 위해서, 사용하는 라인수를 정확하게 지정해주면, 라인 변경 없이 label의 font 크기가 변경되는것을 확인 할수 있습니다. 

---

