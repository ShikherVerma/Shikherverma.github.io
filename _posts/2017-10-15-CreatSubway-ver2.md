---
layout:     post
title:      "지하철 노선도 만들기(초보편) part 2"
subtitle:   "Alert, ScrollView 이용해서 만들기"
date:       2017-10-15 16:26:00
author:     "MinJun"
header-img: "img/tags/Swift-bg.jpg"
comments: true
tags: [Swift]
---

# Alert, ScrollView 를 이용해서 지하철 노선도 만들기!


[Part 1 의 내용은 이곳 입니다.](https://devminjun.github.io/blog/CreatSubway) <br>
[Part 1 프로젝트 파일의 위치는 이곳 입니다.](https://github.com/devminjun/IOS-Develop5/tree/master/Project/CreatSubWay-ver1)<br>
[Part 2 프로젝트 파일의 위치는 이곳 입니다.](https://github.com/devminjun/IOS-Develop5/tree/master/Project/CreatSubWay-ver2)

---

## Part 2 에서 바뀐점

1. Data 부분 분리(각 역의 정보)
2. 반복문을 사용하여 Button 인스턴스 생성
3. 출발역 -> 도착역 다익스트라 알고리즘 사용 
4. 더블탭 실행시, 해당 터치 부분 확대, 축소 구현 

---

## 1. Data 부분을 분리 하여 정리(MVC 의 M)


- 기존(Part 1)

```swift

import UIKit

class ViewController: UIViewController, UIScrollViewDelegate {
    
   /*==========================
          역마다 버튼 만들기
    ==========================*/
    var yeungNamHosp: UIButton!
    var univOfEduc: UIButton!
    var myeongDeok: UIButton!
    
    // 환승역
    var banWorlDang: UIButton!
    
    var jungangno: UIButton!
    var daeguStation: UIButton!
    var chilseongMarket: UIButton!
    
    // 2호선
    var neaDang: UIButton!
    var bangogae: UIButton!
    var sinNam: UIButton!
    
    var kyungDeaHosp: UIButton!
    var daeguBank: UIButton!
    var beomeo: UIButton!
    
    var btn: UIButton!
    
    // 버튼 클릭 횟수
    var clickBtn = 0
    
    // 출발, 도착역 설정
    var startStation = ""
    var arrivalStation = ""
    
    // 스타트 역의 tag 값을 가져온다
    var getStartStionTag = 0


    override func viewDidLoad() {
        super.viewDidLoad()
        /*==========================
               각 역 인스턴스 생성
         ==========================*/
        yeungNamHosp = UIButton()
        univOfEduc = UIButton()
        myeongDeok = UIButton()

        // 환승역
        banWorlDang = UIButton()

        jungangno = UIButton()
        daeguStation = UIButton()
        chilseongMarket = UIButton()

        // 2호선
        neaDang = UIButton()
        bangogae = UIButton()
        sinNam = UIButton()

        kyungDeaHosp = UIButton()
        daeguBank = UIButton()
        beomeo = UIButton()
        
        
        /*=======================
                 각 버튼 위치
         ========================*/
        
        yeungNamHosp = UIButton(frame: CGRect(x: 465, y: 430, width: 20, height: 20))
        univOfEduc = UIButton(frame: CGRect(x: 486, y: 410, width: 20, height: 20))
        myeongDeok = UIButton(frame: CGRect(x: 515, y: 380, width: 20, height: 20))
        
                // 환승역
        banWorlDang = UIButton(frame: CGRect(x: 575, y: 320, width: 20, height: 20))
        
        jungangno = UIButton(frame: CGRect(x: 610, y: 290, width: 20, height: 20))
        daeguStation = UIButton(frame: CGRect(x: 640, y: 265, width: 20, height: 20))
        chilseongMarket = UIButton(frame: CGRect(x: 690, y: 265, width: 20, height: 20))
        
               // 2호선
        neaDang = UIButton(frame: CGRect(x: 430, y: 320, width: 20, height: 20))
        bangogae = UIButton(frame: CGRect(x: 475, y: 320, width: 20, height: 20))
        sinNam = UIButton(frame: CGRect(x: 545, y: 320, width: 20, height: 20))
        
        kyungDeaHosp = UIButton(frame: CGRect(x: 640, y: 320, width: 20, height: 20))
        daeguBank = UIButton(frame: CGRect(x: 680, y: 320, width: 20, height: 20))
        beomeo = UIButton(frame: CGRect(x: 730, y: 320, width: 20, height: 20))
        


        /*==========================
               버튼 색상, 곡률 지정
         ==========================*/

        yeungNamHosp.backgroundColor = UIColor.clear
        yeungNamHosp.layer.cornerRadius = 10

        univOfEduc.backgroundColor = UIColor.clear
        univOfEduc.layer.cornerRadius = 10

        myeongDeok.backgroundColor = UIColor.clear
        myeongDeok.layer.cornerRadius = 10

        // 환승역
        banWorlDang.backgroundColor = UIColor.clear
        banWorlDang.layer.cornerRadius = 10

        
        jungangno.backgroundColor = UIColor.clear
        jungangno.layer.cornerRadius = 10

        daeguStation.backgroundColor = UIColor.clear
        daeguStation.layer.cornerRadius = 10

        chilseongMarket.backgroundColor = UIColor.clear
        chilseongMarket.layer.cornerRadius = 10

        // 2호선
        neaDang.backgroundColor = UIColor.clear
        neaDang.layer.cornerRadius = 10

        bangogae.backgroundColor = UIColor.clear
        bangogae.layer.cornerRadius = 10

        sinNam.backgroundColor = UIColor.clear
        sinNam.layer.cornerRadius = 10

        kyungDeaHosp.backgroundColor = UIColor.clear
        kyungDeaHosp.layer.cornerRadius = 10

        daeguBank.backgroundColor = UIColor.clear
        daeguBank.layer.cornerRadius = 10

        beomeo.backgroundColor = UIColor.clear
        beomeo.layer.cornerRadius = 10



        /*=======================
             버튼 imageView 에 추가.
         ========================*/

        imageView.addSubview(yeungNamHosp)
        imageView.addSubview(univOfEduc)
        imageView.addSubview(myeongDeok)
        
        imageView.addSubview(banWorlDang)
        
        imageView.addSubview(jungangno)
        imageView.addSubview(daeguStation)
        imageView.addSubview(chilseongMarket)
        
        imageView.addSubview(neaDang)
        imageView.addSubview(bangogae)
        imageView.addSubview(sinNam)
        
        imageView.addSubview(kyungDeaHosp)
        imageView.addSubview(daeguBank)
        imageView.addSubview(beomeo)
        
        
        /*=======================
             버튼 기능 연결
         ========================*/
        
        yeungNamHosp.addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
        univOfEduc.addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
        myeongDeok.addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
        
        // 환승역
        banWorlDang.addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
        
        jungangno.addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
        daeguStation.addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
        chilseongMarket.addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
        
        neaDang.addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
        bangogae.addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
        sinNam.addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
        
        kyungDeaHosp.addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
        daeguBank.addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
        beomeo.addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
        
        /*=======================
              역 이름 한글 지정
         ========================*/
        
        yeungNamHosp.titleLabel?.text = "영대병원"
        univOfEduc.titleLabel?.text = "교대"
        myeongDeok.titleLabel?.text = "명덕"
        
        banWorlDang.titleLabel?.text = "반월당"
        
        jungangno.titleLabel?.text = "중앙로"
        daeguStation.titleLabel?.text = "대구역"
        chilseongMarket.titleLabel?.text = "칠성시장"
        
        neaDang.titleLabel?.text = "내당"
        bangogae.titleLabel?.text = "반고개"
        sinNam.titleLabel?.text = "신남"
        
        kyungDeaHosp.titleLabel?.text = "경대병원"
        daeguBank.titleLabel?.text = "대구은행"
        beomeo.titleLabel?.text = "범어"
        
        /*=======================
                 tag 값 설정
         ========================*/
        
        yeungNamHosp.tag = 3
        univOfEduc.tag = 2
        myeongDeok.tag = 1
        
        // 환승역
        banWorlDang.tag = 0
        
        jungangno.tag = 1
        daeguStation.tag = 2
        chilseongMarket.tag = 3
        
        // 2호선
        neaDang.tag = 3
        bangogae.tag = 2
        sinNam.tag = 1
        
        kyungDeaHosp.tag = 1
        daeguBank.tag = 2
        beomeo.tag = 3
    
        
    }


```

- 변경후


| 기존 | 변경후 |
| :------------ | -----------: | 
| ![screen](/img/posts/beforeSubway.jpg) | ![screen](/img/posts/beforeSubway-1.jpg) |


- data.swift 부분 


```swift

import UIKit

struct Station{
    var name: String
    var x: Int
    var y: Int
    var stationTimes: [(sIndex: Int,time: Int)]
    let width: Int = 20
    let height: Int = 20
    func getCGRect() -> CGRect {
        return CGRect(x: x, y: y, width: width, height: height)
    }
    
    static let sn = [
        "안지랑","현충로","영대병원", "교대", "명덕", "반월당",
        "중앙로", "대구역", "칠성시장", "신천", "동대구역",
        "감삼", "두류", "내당", "반고개", "신남",
        "경대병원", "대구은행", "범어","수성구청","만촌"]      //12
    
    static let stations: [Station] = [
        //0
        Station(name: "안지랑", x: 400, y: 470, stationTimes: [
            (sn.index(of: "현충로")!,1)
            ]),
        //1
        Station(name: "현충로", x: 440, y: 455, stationTimes: [
            (sn.index(of: "안지랑")!,1),
            (sn.index(of: "영대병원")!,2)
            ]),
        //2
        Station(name: "영대병원", x: 465, y: 430, stationTimes: [
            (sn.index(of: "현충로")!,2),
            (sn.index(of: "교대")!,3)]),
        //3
        Station(name: "교대", x: 486, y: 410, stationTimes: [
            (sn.index(of: "영대병원")!,3),
            (sn.index(of: "명덕")!,4)]
        ),
        //4
        Station(name: "명덕", x: 515, y: 380, stationTimes: [
            (sn.index(of: "교대")!,4),
            (sn.index(of: "반월당")!,3)]),
        
        //5
        Station(name: "반월당", x: 575, y: 320, stationTimes: [
            (sn.index(of: "명덕")!,3),
            (sn.index(of: "중앙로")!,3),
            (sn.index(of: "신남")!,3),
            (sn.index(of: "경대병원")!,3)]),
        
        //6
        Station(name: "중앙로", x: 610, y: 290, stationTimes: [
            (sn.index(of: "반월당")!,3),
            (sn.index(of: "대구역")!,4)]),
        
        //7
        Station(name: "대구역", x: 640, y: 265, stationTimes: [
            (sn.index(of: "중앙로")!,4),
            (sn.index(of: "칠성시장")!,3)]),
        
        //8
        Station(name: "칠성시장", x: 690, y: 265, stationTimes: [
            (sn.index(of: "대구역")!,3),
            (sn.index(of: "신천")!,4)]),
        
        //9
        Station(name: "신천", x: 738, y: 265, stationTimes: [
            (sn.index(of: "칠성시장")!,4),
            (sn.index(of: "동대구역")!,3)]),
        
        //10
        Station(name: "동대구역", x: 785, y: 265, stationTimes: [
            (sn.index(of: "신천")!,3)]),
        
        //11
        Station(name: "감삼", x: 785, y: 265, stationTimes: [
            (sn.index(of: "두류")!,3)]),
        
        //12
        Station(name: "두류", x: 738, y: 265, stationTimes: [
            (sn.index(of: "감삼")!,3),
            (sn.index(of: "내당")!,4)]),
        
        //13
        Station(name: "내당", x: 430, y: 320, stationTimes: [
            (sn.index(of: "두류")!,4),
            (sn.index(of: "반고개")!,3)]),
        
        //14
        Station(name: "반고개", x: 475, y: 320, stationTimes: [
            (sn.index(of: "신남")!,4),
            (sn.index(of: "내당")!,3)]),
        
        //15
        Station(name: "신남", x: 545, y: 320, stationTimes: [
            (sn.index(of: "반월당")!,3),
            (sn.index(of: "반고개")!,4)]),
        
        //16
        Station(name: "경대병원", x: 640, y: 320, stationTimes: [
            (sn.index(of: "반월당")!,3),
            (sn.index(of: "대구은행")!,4)]),
        
        //17
        Station(name: "대구은행", x: 680, y: 320, stationTimes: [
            (sn.index(of: "경대병원")!,4),
            (sn.index(of: "범어")!,3)]),
        
        //18
        Station(name: "범어", x: 730, y: 320, stationTimes: [
            (sn.index(of: "대구은행")!,3),
            (sn.index(of: "수성구청")!,4)]),
        
        //19
        Station(name: "수성구청", x: 780, y: 320, stationTimes: [
            (sn.index(of: "범어")!,4),
            (sn.index(of: "만촌")!,3)]),
        
        Station(name: "만촌", x: 830, y: 340, stationTimes: [
            (sn.index(of: "수성구청")!,3)])
    ]
}


```

> 구조체를 사용해서, 추가, 삭제 에 대해서 조금더 자유롭게 할수 있게 만들었습니다. 각 역 버튼 생성시, 구조체의 인덱스 번호로 tag 가 생성되고, tag 값을 통해서 각 역에 접근 할수 있습니다. 

---

## 2. 반복문을 사용하여 Button 인스턴스 생성

- Viewdidload 부분 

```swift


import UIKit

class ViewController: UIViewController, UIScrollViewDelegate {
    var scrollView: UIScrollView!
    var imageView: UIImageView!
    
    // 지하철 역버튼 
    var stationBtns: [UIButton]!
    
    var btn: UIButton!
    
    // 버튼 클릭 횟수
    var clickBtn = 0
    
	
   // 출발, 도착역에 대한 Tag값 
    var startStationTag = -1
    var arrivalStationTag = -1
    


    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 스크롤뷰를 만드는 다양한 방법이 있는데, 이분은 이렇게 함.
        let frameSize = view.bounds.size
        scrollView = UIScrollView(frame: CGRect(origin: CGPoint.zero, size: frameSize))
        
        let image = UIImage(named: "deagu.jpeg")
        imageView = UIImageView(image: image)
        scrollView.contentSize = imageView.bounds.size
        
        
        //view 에 뿌려주기
        
        scrollView.addSubview(imageView)
        view.addSubview(scrollView)

        
      
        
        /*==========================
                  btn 만들기
                 각 버튼 위치
         		버튼 색상, 곡률 지정
       		  버튼 imageView 에 추가.
      			   버튼 기능 연결
     			    역 이름 한글 지정
   			      tag 값 설정
         ========================*/
        
        //stationBtns = Array(repeatElement(UIButton(), count: Station.stations.count))
        stationBtns = []
        for i in 0..<Station.stations.count {
            stationBtns.append(UIButton())
            stationBtns[i].frame = Station.stations[i].getCGRect()
            stationBtns[i].backgroundColor = UIColor.black
            stationBtns[i].layer.cornerRadius = 10
            imageView.addSubview(stationBtns[i])
            stationBtns[i].addTarget(self, action: #selector(btnAction(_:)) , for: .touchUpInside)
            stationBtns[i].titleLabel?.text = Station.stations[i].name
            stationBtns[i].tag = i
            
            print(stationBtns[i].titleLabel?.text, stationBtns[i].tag)
        }

               
        

```

> 위의 코드를 사용해서, 버튼 생성, 버튼 색상 정의, 코너값 지정, add SubView, addTarget, titleLable, tag 값 설정을 해주었습니다.

---

## 3. 출발역 -> 도착역 다익스트라 알고리즘 사용 

출발역 -> 도착역에 대한 시간을 연산할때, 기존의 방법으로 하게되면, 각 라인별로 환승역에 추가되면, 환승역 추가되는 위치에 따라서 경우의 수가 기하급수 적으로 많아집니다. 모든 경우의 수를 계산해서 경우의수 별로 출발점과 도착점까지의 시간을 계산하여 반환할수 있지만, 한번 경우의 수를 짜놓으면 변경하기가 힘들다는 단점이 있었습니다. 조금 유연하게 바꾸어줄수 있는 방법 찾다 보니까 `다익스트라` 라는게 있어서 적용 해보았습니다. 


```swift

** 버튼의 기능부 입니다 **
** 기존에 if else -> switch 구문으로 변경 했습니다 **

@objc func btnAction(_ sender: UIButton) {
        
        let station = Station.stations[sender.tag]
        
        switch clickBtn {
        case 0:
            
            /*=======================
             출발역 알럿
             ========================*/
            let popAlert: UIAlertController = UIAlertController(title: "출발 역 은", message: "\(station.name) 입니다", preferredStyle: .alert)
            
            /*=======================
             OK 누르면 출발역 설정
             ========================*/
            let okAlertAction: UIAlertAction = UIAlertAction(title: "OK", style: .default, handler: { (alert) in
                
                // 전역 변수에 출발역 설정
                self.startStationTag = sender.tag
                self.clickBtn += 1
                print("출발역은 \(Station.stations[self.startStationTag].name)")
            })
            
            /*=======================
             cancle 버튼 누르면 출발역 재설정
             ========================*/
            let cancelAlertAction: UIAlertAction = UIAlertAction(title: "Cancel", style: .default, handler: { _ in })
            
            /*=======================
             alert 액션 설정
             ========================*/
            popAlert.addAction(okAlertAction)
            popAlert.addAction(cancelAlertAction)
            
            /*=======================
             alert 을 뷰에 뿌려줌
             ========================*/
            
            self.present(popAlert, animated: true, completion: nil)
            
            
            /*=======================
             도착역 설정 구간, 여기가 핵심(출발역 -> 도착역 알고리즘 적용)
             ========================*/
            
        case 1:
            
            print("도착역을 설정 해주세요 ")
            
            /*=======================
             도착역 설정 알럿
             ========================*/
            let popAlert: UIAlertController = UIAlertController(title: "도착 역 은", message: "\(station.name) 입니다", preferredStyle: .alert)
            
            
            /*=======================
             도착역 OK 버튼 누르면 예상 시간 반환
             ========================*/
            let okAlertAction: UIAlertAction = UIAlertAction(title: "OK", style: .default, handler: { (alert) in
                
                // 도착역 전역변수에 지정
                self.arrivalStationTag = sender.tag
                print("출발역은 \(Station.stations[self.startStationTag].name), 도착역은 \(Station.stations[self.arrivalStationTag].name) " )
                
                
                // 다익스트라
                // 각 역별로 출, 도착 시간을 계산할 표를 만듭니다.
                var calcTable: [(sIndex: Int, timeSum: Int)] = []
                for i in 0..<Station.stations.count {
                    calcTable.append((i,Int.max))
                }
                
                
                
                //startStationTag
                //arrivalStationTag
                
                //var currentTag
                
                // 다음역을 계산하기 위해서, 다음역 tag값을 지정하기 위한 변수를 선언 합니다.
                var nextStationTag: Int? = self.startStationTag
                
                // 환승역 구간같이 여러 갈래가 있는 역에서 반복 연산을 하지 않기 위해서, 이미 접근한 역의 tag 값을 넣습니다. 
                var dicAlreadyCheckStations: [Int] = []
                
            		// 시작점의 시간은 0 으로 둡니다(이유는, 현재 이 코드의 한계점 때문입니다. 한계점은 뒤에서 설명 하겠습니다)
                calcTable[nextStationTag!].timeSum = 0
                
                
                
                while nextStationTag != nil {
                    //다음역
                    let station = Station.stations[nextStationTag!]
                    let currentStationTime = calcTable[nextStationTag!].timeSum
                    //station을 방문했다고 체크
                    dicAlreadyCheckStations.append(nextStationTag!)
                    
                    // 현재역 -> 다음역 까지 걸리는 시간을 위의 calcTable 에 연산후 작성합니다.
                    for stationTime in station.stationTimes {
                        let ssIndex = stationTime.sIndex
                        let calcTimeSum = (currentStationTime + stationTime.time)
                        if( calcTable[ssIndex].timeSum > calcTimeSum ) {
                            calcTable[ssIndex].timeSum = calcTimeSum
                        }
                    }
                    
                    // 모든역을 방문 하고, 다음역이 더이상 없다면 while 구문을 종료합니다. 
                    nextStationTag = nil
                    
                    var minTime = Int.max // 시작역과 가장 가까운 역의 걸리는 시간 저장
                    for calcTableItem in calcTable { //모든 역 비교
                        print(dicAlreadyCheckStations)
                        
                        //방문한적이 없으면서 시작역과 가장 가까우면(시간)
                        if !dicAlreadyCheckStations.contains(calcTableItem.sIndex) && 
                            minTime > calcTableItem.timeSum { 
                            
                            // 연산할 다음역의 sIndex 지정, 
                            nextStationTag = calcTableItem.sIndex 
                            
                            //시작역과 가장 가까우면(시간)
                            minTime = calcTableItem.timeSum 
                        }
                    }
                  
                 
                }
                
                let popAlert: UIAlertController = UIAlertController(title: "총 \(calcTable[self.arrivalStationTag].timeSum) 분 소요 됩니다", message:"" , preferredStyle: .alert)

                let okAlertAction: UIAlertAction = UIAlertAction(title: "OK", style: .default, handler: { (alert) in

                    // 모두 리셋
                    self.startStationTag = -1
                    self.arrivalStationTag = -1
                    self.clickBtn = 0

                })
                
                
                /*=======================
                 결과 알럿 출력
                 ========================*/
                popAlert.addAction(okAlertAction)
                self.present(popAlert, animated: true, completion: nil)
                
            })
            ///////////////////////위까지 okAlert 범위
            
            // 도착역 재설정
            let cancelAlertAction: UIAlertAction = UIAlertAction(title: "Cancel", style: .default, handler: { _ in })
            
            // 출발역  재설정
            let resetStartStation: UIAlertAction = UIAlertAction(title: "ResetStatstation", style: .default, handler: { (alert) in
                self.clickBtn = 0
                self.startStationTag = -1
            })
            
            
            // 3. 알럿액션을 알럿 컨트롤러에 연결
            popAlert.addAction(okAlertAction)
            popAlert.addAction(cancelAlertAction)
            popAlert.addAction(resetStartStation)
            
            // 4. 알럿 뿌려주기
            
            self.present(popAlert, animated: true, completion: nil)
            
        default: break
        }
        
        
    }



```

> 도착역을 지정 했을때 연산하는 과정 이외의 부분은 Part 1 이랑 동일 합니다.
> 
> 아까 위에서 이 코드의 한계인데, 위의 코드에서는 출발역 -> 도착역을 지정 했을때, 출발역 -> 도착역 까지의 부분만 연산하지 않고, 출발역 -> 도착역 과, 현재 있는 모든역 까지의 시간을 계산합니다. 그리고 출발역 -> 도착역 까지 걸린 시간을 알럿에 띄워 주어서 연산을 합니다. 
> 

---

## 4. 더블탭 실행시, 해당 터치 부분 확대, 축소 구현 


```swift

    @objc func tapToZoom(_ gestureRecognizer: UIGestureRecognizer) {
  
        // 더블탭 간단 하게 구현
        if scrollView.zoomScale == CGFloat(1) {
            
            
            // 해당 화면의 X,Y 좌표를 이용해서 확대후, 확대 이전의 좌표값 과 연산을 통해서 그 위치로 이동..
            let locationX = gestureRecognizer.location(in: scrollView).x*2.95-90
            let locationY = gestureRecognizer.location(in: scrollView).y*2.95-300

            scrollView.setZoomScale(3, animated: true)
            
            scrollView.setContentOffset(CGPoint(x: locationX, y: locationY), animated: true)
        }else {
            
            let locationX = gestureRecognizer.location(in: scrollView).x/2.95-200
            let locationY = gestureRecognizer.location(in: scrollView).y/2.95-250
            scrollView.setZoomScale(1, animated: true)

            scrollView.setContentOffset(CGPoint(x: locationX, y: locationY), animated: true)
        }
        
    }




```

> 엄밀하게 이야기하면 더블탭시 해당 위치로 가는것이 아니라 눈속임(?)을 합니다.. 더블 탭한 해당 위치로 확대 되는것이 아니라 해당 x,y 값을 연산후, 수십번의 삽질(?) 계산을 통해서 대강 비슷한 위치로 이동하게 만들었습니다. 그래서 디버깅모드로 보면 위치가 말끔하게 촤~악 가는게 아니라, 확대 되고 -> 약간의 움직임이 있습니다...ㅠ_ㅠ 


---



## 여담 

- 더 보완이 필요한 부분
	1. 다익스트라가 익숙하지 않아서 아직 많이 부족합니다, 출발역 -> 도착역 지정 했을때, 모든 경우를 연산하지 않고, 필요한 부분만 연산해서, 결과를 반환하게 한다면 데이터가 많아 졌을때 조금더 빠른 성능을 가질것 같습니다. <br>
	2. 더블탭 했을때, 탭 하는 부분을 확대, 축소 하게 만들고 싶은데 위의 코드는 사실 눈속임(?) 한것 같습니다. 엄밀하게 더블탭 했을때 그 부분을 확대 한다기 보다는 확대후 -> 해당 좌표를 찾아가게 만들었습니다. 조금더 좋은 의견 있으시면 댓글 남겨 주시면 정말 감사하겠습니다 (__)...!<br>



## Reference 

[다익스트라 관련 동영상과 코드](http://blog.daum.net/imagineer_ms/20)

**패스트캠퍼스 정원님 진심으로 감사합니다🙏🙏🙏**

