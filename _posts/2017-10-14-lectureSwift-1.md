---
layout:     post
title:      "Swift 기본 문법 개괄 "
subtitle:   "필요한 문법만 뽑아서 쓰자! part 2"
date:       2017-10-14 17:36:00
author:     "MinJun"
header-img: "img/tags/Swift-bg.jpg"
comments: true
tags: [Swift]
---

[야곰의 스위프트 기본 문법 강좌](https://www.inflearn.com/course/스위프트-기본-문법/)<br>
약 5시간 정도의 시간으로 swift의 기본적인 문법 내용을 개괄 할수 있습니다.

---

## 여담 

프로그래밍을 완전 처음 접하는 분이 듣기에는 내용은 쉽지만 각각의 내용들을 연결하는데 조금 힘든 부분이 있는것 같습니다. <br>
하지만 다른 언어가 아니라, Swift를 조금 배우셧던 분이 들으시면 알고 있는 내용들을 빠르게 한번 정리 할수 있습니다. 정리는 저에게 필요한 부분만 정리해서 생략된 부분이 많습니다..! 

---

## 목차

1. property
2. inheritance
3. init, deinit
4. optional chaining & nil-coalescing operaton
5. type casting
6. protocol
7. extension
8. error handling
9. higher-order function
	1. map
	2. filter
	3. reduce
	




---

## 1. 프로퍼티 종류 

- 저장 프로퍼티(stored property)
- 연산 프로퍼티(computed property)
- 인스턴스 프로퍼티(instance property)
- 타입 프로퍼티(type property)

- 프로퍼티는 구조체, 클래스, 열거형 내부에 구현할 수 있습니다.
- 다만 열거형 내부에는 연산 프로퍼티만 구현할 수 있습니다
- 연산 프로퍼티는 var 로만 선언할 수있습니다.


---

## 1-1. computed property(연산 프로 퍼티)

- 연산 프로퍼티의 동작은 변수 연산프로퍼티 변수를, 저장 프로퍼티에 연결 시켜 놓고, 저장 프로퍼티에 값이 들어가면, 연산 프로퍼티의 get 부분에서 값 들어가는 부분이 동작하고 저장 프로퍼티에 값을 넣으면, 연산프로퍼티의 get부분이 불려온다.

- 연산 프로퍼티에 값을 넣으면, set 부분이 불려오면서, 연결되어 있는 저장 프로퍼티의 값이 불려온다.

- set의 값에 매개변수 이름을 사용하지 않으면 `newValue` 라고 암시적으로 사용할수 있다.

```swift
1.

var x: Int {
    get {
        return tempX
    }
    set(newValue) {
        tempX = newValue * 2
    }
}
    
x = 10 // 컴파일 에러! 
 -> 이유는 현재 연산 프로퍼티에 저장소가 없기때문에 값을 저장할수가 없음
 
2.

var tempX : Int = 1
var x: Int {
    get {
        return tempX * 10
    }
    set(newValue) {
        tempX = newValue * 2
    }
}
    
tempX = 10
print(tempX, x) // 10, 100

x = 10 
print(tempX, x) // 20, 200
 -> tempX 값이 20이 되고(set불리고) -> (get 불림) -> x값이 200
 
**읽기전용 프로퍼티**

get 만있으면 읽기전용 프로퍼티가 되는데, set만 있는건 안됨, get, set 둘다 있어야 한다.
 
```

---

## 1-2. property observer(프로퍼티 옵져빙)

프로퍼티의 값이 변경 되었을때, 변한값과, 변한이후 값을 출력해줄수 있다.

```swift

var intObserver: Int = 1000 {
    willSet {
        print("\(newValue)")
    }didSet {
        print("\(oldValue)")
    }
}



```

> intObserver = 10 // 10, 1000 - > willset 은 변한값, didSet은 변하기전값, 호출은  옵져빙의 호출은 둘다 된다.
> 
> willSet은 바뀌기 직전에 호출이 되고, 
> 
> didSet은 변경이 될때 호출이 된다..! 
> 
> 암시적으로 wiilSet() 파라미터를 작성하지 않으면 willSet에는 newValue, didSet에는 oldValue 가 암시적으로 사용이 된다. 
> 

---

## 2. inheritance(상속) 

- 스위프트의 상속은 클래스, 프로포토콜 등에서 사용이 가능하다 
- 열거형, 구조체는 상속이 불가능 하다
- 다중상속을 지원하지 않음

- 문법

```swift

class 이름: 상속받을 클래스 이름 {
    구현부
    
}

class SubClass: superClass {

	fianl sayHello() {
			print("hi")
		}
		-> fianl 을 사용하면 자식 클레스에서 재정의를 방지 할수 있습니다. 
		
	// 타입 매서드
	
	// 재정의 불가(overrid 불가)
	static func typeMethod() {
		print("type method - static")
	}
	
	// 재정의 가능 
	class func classMethod() {
		print("classMethod")
		}
		
	
}

```

> 재정의가 가능한 타입 매서드는 'class'
> 
> 재 정의가 불가능한 타입 매서드는 'static', final', 'final class' 
> 
> 저장 프로퍼티는 상속을 받아서 온 것이라서, 오버라이드를 할수 없습니다.

---

## 3. 인스턴스의 생성과 소멸 init, deinit



- 인스턴스 생성시 조건주기, 이때 init은 옵셔널이됨..

```swift

class Age {
    var age: Int
    
    
    init?(age: Int) {
        if (0...100).contains(age) == false {
            return nil
        }
        self.age = age
    }
}


var test = Age(age: 200)

```

- 인스턴스 소멸

```swift


class test {
    var name: String?
    
    // 변수에 옵셔널을 넣어주면, init을할때, 그 부분은 있어도, 없어도 상관이 없을수 있다.
    
    deinit {
        print("인스턴스 해제")
    }
}

var aa: test? = test()
aa?.name = "민준"

aa = nil
-> nil을 할당할수 없는데, 클레스 타입 자체를 옵셔널로 만들어서, nil 을 할당 가능 하게 만들어주어서 해제 시켜 보았습니다.

```

---

## 4. optional chaining(옵셔널 체이닝)

optional chaining & nil-coalescing operaton(옵셔널 체이닐과 nil 병합 연산자)


- 옵셔널 체이닝 문법


```swift

step?step2?step3? ->

step 이 옵셔널이 아니면 step2로 넘어가고, step3 까지 넘어간다. 하지만, step,step2,step3 중에 nil이 있으면, 연쇄작용을 통해서 nil을 검사해서, nil이 있으면 멈추게됨.

```

- 소스 코드

```swift

class Person {
    var name: String
    var job: String?
    var home: Apartment?
    
    init(name: String) {
        self.name = name
    }
}

class Apartment {
    var buildingNumber: String
    var roomNumber: String
    var `guard`: Person?
    var owner: Person?
    
    init(dong: String, ho: String) {
        buildingNumber = dong
        roomNumber = ho
    }
}

let yagom: Person? = Person(name: "yagom")
let apart: Apartment? = Apartment(dong: "101", ho: "202")
let superman: Person? = Person(name: "superman")

yagom?.home?.guard?.job // nil

yagom?.home = apart

yagom?.home // Optional(Apartment)
yagom?.home?.guard // nil

yagom?.home?.guard = superman

yagom?.home?.guard // Optional(Person)

yagom?.home?.guard?.name // superman
yagom?.home?.guard?.job // nil

yagom?.home?.guard?.job = "경비원"

-> 이런식으로 옵셔널 체이닝을 사용할수 있다.
-> 사실 개인적으로 옵셔널 채이닝 보다, if let, guard let 이 더 편한것 같다.

```

---

## 4-1. nil 병합 연산자

- 값이 옵셔널일때, 다른 값을 넣으라고 디폴트 값을 지정 해줄수 있다.

```swift

var nilValue: Int?

var changeValue: Int = 10

changeValue = nilValue? ?? 10

print(changeValue) // 10 

```

---

## 5. type casting(타입 캐스팅)

- is, as 를 사용해서 타입을 캐스팅 한다. 

```swift

var intValue: Int = 10

var doubbleValue: Double = Double(intValue) 

-> 이건 엄밀하게 타입캐스팅이 아니다. Double 타입의 인스턴스를 하나 더 생성 한거다. 래퍼런스 문서를 들어가보면, doubble 가 init이 굉장히 많다는것을 알수 있음. 


```

- is 

taget인스턴스 is 확인할 instance -> Bool 값을 반환한다. 

대상인스턴스가, 확인할 instance가 맞는지 확인한다. 

- as 

as, as?, as! 사용할떄 다르다


- as 

- as?
	- 조건부 반환 캐스팅: 반환값을 옵셔널로 반환이 될수 있다.

- as!
	- 강제 다운 캐스팅: nil 일 경우 런타임 오류가 발생 한다.


> 조금더 정리가 필요하다 . as, as?, as! 를 사용했을때, 어떤 경우에 nil인지 확인 해보자, 그리고 nil일때 nil이 아니게 만들어 주는 방법은 무엇일까?


---

## 6. protocol(프로토콜)

- 정의: 특정 역할을 수행하기 위한 메서드, 프로퍼티, 이니셜라이저 등의 요구사항을 정의 합니다.
 
> 너는 이 기능이 꼭 필요하니까 정의 해놔!


- 문법

```swift

protocol 프로토콜 이름 {
	구현부
	
	}
	
	
protocol Talkable{

 	//프로퍼티 요구, 읽기, 쓰기, 혹은 읽기만 가능 한지 확인 해주어야한다
	var topic: String { get set }
	var language: String { get }
	
	// 메서드 요구 
	func talk()
	
	//이니셜 라이저 요구
	
	init(topic: String, language: String)
	

```

- 프로토콜 체텍

```swift

 -> 구조체가 Talkable 프로토콜을 체텍함.
struct Person: Talkable {
	var topic: String
	let language: String 
	
	func talk() 
	
	

```

> class 에서는 상속받을 class 명을 먼저 사용하고 그 이후에 프로토콜을 체택해야 합니다. **순서를 지켜야함!**
> 
> 프로토콜의 장점은 컴파일 입장에서 조금더 명확하게 사용자가 무엇을 하는지 알수 있다.
> 

---

## 7. extension(익스텐션)  

**기능에 기능을 덧 붙여서 연장시킨다!**

익스텐션은 구조체, 클래스, 열거형, 프로토콜 타입에 새로운 기능을 추가할 수 있는 기능 입니다.
기능을 추가하려는 타입의 구현된 소스 코드를 알지 못하거나 볼 수 없다 해도, 타입만 알고 있다면 그 타입의 기능을 확장할 수도 있습니다.

- 문법

```swift

extension 확장할 타입 이름

ex) 

extension String 이렇게 사용할수 있음

extension String: 프로토콜1, 프로토콜2, .... 이런식으로 요구 할수 있다. 


- 예제


extension Int {
    var minjunNumber: Int {
        return self * 100
    }
    var fastcumpusNumber: Int {
        return self * 0
    }
    
    func minjunFunc(n: Int) -> Int {
        return self * n
    }
}


print(10.minjunNumber) // 1000
print(10.fastcumpusNumber) // 0

print(10.minjunFunc(n: 10)) // 100

-> extension 은 Python Decoration 같이 사용되는것 같다. 


```

> 강력하게 사용할수 있는것 같다. 내가 원하는 기능만 떼어 와서 원하는 클레스에 붙여 놓아서 사용해도 좋을것 같다...


---

## 8. error handling(에러 헨들링)

Enum 에 Error 프로토콜을 체텍해서, Error 를 발생시켜서, 프로그램을 중지 시킬수 있다. 
에러를 발생시킨는 명령어는, 에러를 던져준다고 해서 `throw` `enum.case` 를 작성한다

- 문법

```swift

enum errorHandlr: Error {
    
    case notMoney
    case Ok
}




var value: Int = 10

if value < 10 {
    throw errorHandlr.notMoney
}else {
    throw errorHandlr.Ok

}

- > value 값이 무엇이든간에 무조건 오류가 발생함... 

``` 

> do, try, catch 도 있지만, 지금은 여기까지만 사용하자.
 
---

## 9. higher-order function(고차 함수)

- 스위프트의 문법은 아니지만, 유용한 녀석들이다. 
	- map, filter, reduce 

- map: 컨테이너 내부의 기존 데이터를 변형 하여 새로운 컨테이너 생성 후, Array 안에 있는 값들을 계산함. map은 일종의 함수 역활을 하고, 대상  Array 를 설정해놓은 함수에 집에 넣는다고 생각하자 

```swift

var number: [Int] = [1,2,3,4,5,6]

var test1 = number.map { (value: Int) -> Int
    in
    return value * 10
}

print(test1) //10 20 30 40 50 60

var test2 = number.map { $0 * 10
}

print(test2) //10 20 30 40 50 60 
 -> 클로져의 생략을 사용했습니다.



```

- filter: filter 는 return 타입이 bool 이다. 설정해놓은 조건이 true일때, 내가 설정해놓은 값을 filter의 container에 집에넣고 그 값을 핸들링 한다 

```swift

var test3 = number.filter { (value: Int) -> Bool in
    
    // 짝수인 조건을 필터함..!
        return value % 2 == 0
    
}
print(test3)

var test4 = number.filter { $0 % 2 == 0
}

print(test4)


```

- reduce : Array 에 담긴 값 모두를 각각 더한다 피보나치와 같은 원리입니다.

```swift


var test5 = number.reduce(0) { (first:Int, second: Int) -> Int in
    return first + second
}

print(test5)


var test6 = number.reduce(0) { $0 + $1 }

print(test6)

```

---

