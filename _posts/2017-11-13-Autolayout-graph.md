---
layout:     post
title:      "Swift. Autolayout 을 이용해서 그래프 만들기"
subtitle:   "Autolayout을 통해서 Animate 한 graph 만들기!"
date:       2017-11-13 18:00:00
author:     "MinJun"
header-img: "img/tags/Swift-bg.jpg"
comments: true
tags: [Swift]
---

## Graph_AutoLayout_Code(멋지게 그래프를 표현하는 방법과 애니메이션)

![scrrn](/img/posts/graph.MOV.gif) <br>

--View <br>
----view 6개 <br>

View 속에, 그래프로 사용될 View 6개를 만들어 놓고, autoLayout 을 통해서, 그래프의 변경 되는 값을 조정해줍니다.<br>
기본 superview의 autolayout 을 통해서, 위치와 사이즈를 정해줍니다. 그 속에서, 각 그래프의 bottom 을 superView에 고정 해놓고 각 View들의 height 값을 비율로 정의해서 그래프를 정의하는 방법을 사용합니다.


> 원리는, View의 bottom 을 고정시켜놓고, 그래프의 height값을 input값으로 받아서 변경시켜주면, 유동적은 그래프를 사용할수 있습니다.
> 


```swift
import UIKit
class ViewController: UIViewController {
	 // autoLayout 으로 지정한 height 값을 IBOutlet을 통해서 가져옵니다. 
    @IBOutlet weak var graph1Height: NSLayoutConstraint!
    @IBOutlet weak var graph2Height: NSLayoutConstraint!
    @IBOutlet weak var graph3Height: NSLayoutConstraint!
    @IBOutlet weak var graph4Height: NSLayoutConstraint!
    @IBOutlet weak var graph5Height: NSLayoutConstraint!
    @IBOutlet weak var graph6Height: NSLayoutConstraint!
    
    override func viewDidLoad() {
        super.viewDidLoad()   
    }
    
    @IBAction func btn1Action(_ sender: UIButton) {
        UIView.animate(withDuration: 3, animations: {
            self.graph1Height = self.graph1Height.changeMultiplier(changeMultiplier: 0.1)
            self.graph2Height = self.graph2Height.changeMultiplier(changeMultiplier: 0.2)
            self.graph3Height = self.graph3Height.changeMultiplier(changeMultiplier: 0.3)
            self.graph4Height = self.graph4Height.changeMultiplier(changeMultiplier: 0.4)
            self.graph5Height = self.graph5Height.changeMultiplier(changeMultiplier: 0.5)
            self.graph6Height = self.graph6Height.changeMultiplier(changeMultiplier: 0.6)        
            self.view.layoutIfNeeded()
        })
    }
    
    @IBAction func btn2Action(_ sender: UIButton) {
        self.graph1Height = self.graph1Height.changeMultiplier(changeMultiplier: 0.6)
        self.graph2Height = self.graph2Height.changeMultiplier(changeMultiplier: 0.5)
        self.graph3Height = self.graph3Height.changeMultiplier(changeMultiplier: 0.4)
        self.graph4Height = self.graph4Height.changeMultiplier(changeMultiplier: 0.3)
        self.graph5Height = self.graph5Height.changeMultiplier(changeMultiplier: 0.2)
        self.graph6Height = self.graph6Height.changeMultiplier(changeMultiplier: 0.1)        
        UIView.animate(withDuration: 3, animations: {
            self.view.layoutIfNeeded()
        })
    }
}

// NSLayout의 multiplier 를 코드로 바로 적용하려고 하면, 적용이 되지 않습니다. 그래서 원래의 Constraints 값을 비활성화후, 새롭게 Constraints 값을 생성후, 새로 적용된 값을 Constraints에 적용 시켜서 사용합니다..!
extension NSLayoutConstraint {
    func changeMultiplier(changeMultiplier: CGFloat) -> NSLayoutConstraint {   
        // 원래 자신의 Constraint 값을 deactivate 하고,
        // 새롭게 Newconstraint 값을 적용한것을, activate 함..
        NSLayoutConstraint.deactivate([self])
        let newConstraint = NSLayoutConstraint(item: self.firstItem,
                                              attribute: firstAttribute,
                                              relatedBy: self.relation,
                                              toItem: self.secondItem,
                                              attribute: self.secondAttribute,
                                              multiplier: changeMultiplier,
                                              constant: self.constant)
        // View가 가지고 있는 기본 셋팅값을 사용하겠다는 코드
        newConstraint.priority = self.priority
        newConstraint.shouldBeArchived = self.shouldBeArchived
        newConstraint.identifier = self.identifier
        NSLayoutConstraint.activate([newConstraint])
        return newConstraint
    }
    //NsLayout을 사용하는데, 내가 기본셋팅을 바꾸어서, 원하는 값만 사용하려고함
}
```

> Animate 적용시 `self.view.layoutIfNeeded()` 적용 시켜 주어야, 변경된 Layer 값이 실시간으로 적용이 됩니다..!
> 
> 주의 사항으로는 Constraints 값을 두번 정의하게되면, 오류가 나게 됩니다. 이유는 Constraints 값이 두번 적용이 되고, Priority 값이 기본적으로 1000 이기때문에, 어떤 값을 적용 해야하는데 알수 없어서 나는 오류입니다. 


---

## Reference 

[IOS Autolayout 강의](https://www.inflearn.com/course/autolayout-ui_ios/)
