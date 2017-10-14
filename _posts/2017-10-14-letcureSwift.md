---
layout:     post
title:      "Swift 기본 문법 개괄"
subtitle:   "필요한 문법만 뽑아서 쓰자! part 1"
date:       2017-10-14 17:35:00
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

1. 이름 짓기
2. 상수, 변수
3. 기본 데이터 타입
4. Any, AnyObject, nil
5. Array, Dictionary, Set(컬렉션 타입)
6. func
7. if-else if, switch
8. Optional
9. Optional wrappiong,
10. Struct
11. Class
12. Enum
13. Call by Value, Call By Reference
14. Closure 


---

## 1. 이름짓기, 콘솔창 출력 


lowerCamelCase: 함수, 매서드, 변수, 컨텐츠 
	- ex) funcName...
upperCamelCase: 클레스, 구조체 등등 
 	- ex) ClassName...
 	- 

> 암묵적으로 변수 이름을 이렇게 지정하자 라는 개발자들 사이의 약속(?) 같은것..?

- print(인스턴스의 값 출력)
- dump(인스턴스의 포함되어 잇는 모든것 출력(?)

---

## 2. 상수, 변수

> 스위프트는 함수형 프로토콜을 체택한 언어 라서, 가변적인 변수와, 불변적인 상수의 역활을 중요하게 생각한다. 

변수 정의 키워드 `var`
상수 정의 키워드 `let`

---

## 3. 기본 데이터 타입

bool, Int, UInt, Float, Double, Character, String 


- Bool

true, false 

- UInt(UnSigned integer: 양의 정수)

양의 정수

- Float(부동 소수형 타입)

- Double(더블형 타입)
 


```swift

var floatNum: Float = 0.1
var floatNum1: Float = 0.1

var doubleNum: Double = 0.1
var doubleNum1: Double = 0.1 

print(floatNum * floatNum1) // 0.01
print(doubleNum * doubleNum1) // 0.01

dump(floatNum * floatNum1) // 0.0100000007
dump(doubleNum * doubleNum1) // 0.010000000000000002


```

> Float, Double 형의 차이는 '정밀도'의 범위이다. 예제로 확인해보자
> 부동 소수점에 대한 설명은 생략한다!
> Double 형이 Float 형 보다 표현할수 있는 `정밀도` 범위가 넓다. 
> Float 형의 정밀도 범위가, 내가 하려고 하는 연산의 정밀도 범위 안에 있다면, 궂이 Double 을 사용해서 리소스를 잡아먹을 이유가 없다


- Character : 유니코드로 표현할수 있는 모든 문자를 넣어줄수 있다.
   - 문자가 한개가 아니라, 여러개를넣으면 캐릭터 타입이 아니라 문자열 타입이라서 안된다.

   
---

## 4. Any, AnyObject, nil

**Any, AnyObject 타입에 다른 타입의 값을 넣었을때, 타입 캐스팅을 해줘야 하는 예제를 작성 해놓자**

- Any : Swift의 모든 타입을 지칭하는 키워드(super 타입?)


```swift

var anyValue: Any = 10

var intValue: Int = anyValue as! Int


```

> Any 타입의 값을 Int 타입으로 넓으려고 하니까 다운 캐스팅하라고, 컴파일리 알려준다..! 


- AnyObject : 모든 클레스 타입을 지칭하는 프로토콜(AnyObject는 클레스의 인스턴스만 넣을수 있다.)

```swift

class testAnyObject { }
var anyObjects: AnyObject = testAnyObject()


var someObject: testAnyObject = anyObjects as! testAnyObject

```

- nil : 없음을 의미한다

```swift

var nilValue: Int?

print(nilValue) // nil

가능하지만, 

var nilValue = nil 

// nil 을 할당하면, 컴파일 오류가 난다

```

> 스트링, 인트, 더블 등등 타입들을 사실 구조체로 되어 있다.

변수에 nil 값 자체를 할당 할수는 없다.
  
  
---

## 5.Collection types(Araay, Dictionary, Set)

- Array
 
```swift

기본적으로 Array의 정의는 

var integers: Array<Int> = Array<Int>() 
 -> 인스턴스를 정의하는것!

var doubles: Array<Double> = [Double]()
 -> 축약을 사용해서 정의 할수 있다.
 
var doubles: Array<Double> = []
 -> 빈 어레이를 정의할때.

```

- Dictionary : 키와 값이 쌍으로 이루어진 collection type

> 딕셔너리를 능숙하게 다루면, 왠만한 데이터 핸들링이 쉬워진다, 딕셔너리를 능숙하게 다루는 방법을 연습하자



```swift

// 딕셔너리 정의하기 
var anyDictionary: Dictionary<String, Any> = [String: Any]()


// 딕셔너리 응용 
var dic: [[String:String]] = [["id" : "민준","password": "123"]]

print(dic[0]["id"],dic[0]["password"])

// dictionary 를 내가 원하는 형식으로 사용하기 위해서는 이런식으로 만들어서 사용해도 괜찮을것 같다.


```

> 딕셔너리는 순차적이지 않음, 그냥 키와 값의 타입으로만 되어 있다. 



- Set : Set은 중복된 값을 보장해준다. 그래서 같은 값들을 넣어도 한가지 값만 들어가 있는다.(수학의 집합 연산하기 좋다.)


```swift

Set 응용

var setValue: Set<Int> = Set<Int>()
var setValue1: Set<Int> = Set<Int>()

setValue = [40,10,20,40]
setValue1 = [50,50,10,10]

print(setValue, setValue1) // 40, 10, 20   50, 10

//set 의 값을 합쳐주고, sorted 로 정렬해준다!
var setUnion = setValue.union(setValue1).sorted()

print(setUnion) // 10, 20, 40, 50


```

 
 
---

## 6. 함수 기본



- 반환 값이 없는 함수 

```swift
func 함수이름(매개변수이름: 매개변수타입) -> Void {


	return 
	}                         

func notReturnValue(value: Int = 0 )  {
    
    print("아무것도 없음")
    return
}

notReturnValue()

```

> 나에게 필요한 것만 정리하자 

---

## 6-1. 함수 고급 


**전달인자 레이블을 사용하면 중복 정의를 사용할수 있음. greeting 이라는 함수 이름이라는 함수를 되었다고 생각될수 있지만, 사실은 레이블이 포함된 이름을 사용하기 때문에, 레이블을 잘 사용하면 전달 인자의 역활도 나눌수 있고, 잘 사용할수 있음. **

```swift


func greeting(xValue x: Int) {
    print("x값 출력")
}


func greeting(yValue y: Int) {
    print("y값 출력")
    
}

각각 함수 이름은 같지만, 정의가 가능하다. swift는 함수형 패러다임을 가지고 있어서, 함수의 이름만 가지고 정의 하는것이 아니라, 함수의 레이블 이름까지 포함된 이름을 함수 이름으로 정의 할수 있다. 그래서 같은 이름의 함수를 호출 하는것 같지만, 사실을 다른이름의 함수를 호출하는것 이다!


 -> 기본적으로 swift는 매서드 오버라이드가 안됨(같은 이름 정의를 하면 덮어 씌우는게 아니라, 컴파일 에러가 난다)

```

- 가변 매개 변수

```swift

func parameter(y: Int...) {
    
    for i in y {
        print(i)
    }
    
}

```

> 가변인자를 사용하게 되면 ,y 는 Array 형태로 값이 묶여러 들어간다. 그래서 그 값을 사용하기 위해서 반복문을 사용해서 각각의 값들을 뜯어서 사용해줄수 있다.
> 
> 가변 매개 변수는 함수당 한번만 정의 할수 있고, 여러개의 값을 받을때 사용하면 유용하다
> 

**스위프트는 함수형 패러타임을 가지고 있음**

--- 

## 7. 조건문 

- if-else ,switch 

```swift

switch 케이스에서는, 디폴트가 없으면 

var num = 1

switch num {
case 1:
    print("1입니다") 
    fallthrough
case 2:
    print("2입니다")

case 3:
    print("3입니다")

default:
    print("마지막 입니다")

}

위의 switch 구문을 실행하기 되면 "1입니다" 프린트후, "2입니다" 까지 자동으로 프린트 하게된다!

```

> switch 구문에서 에서는 `fallthrough` 를 사용하면, 어떤 조건이 걸리고 난 이후에, 다음 구문을 실행 시킬수 있다.


--- 

## 8. 옵셔널

- Optional : 값이 있을수도 있고, 없을수도 있음.

> Optional 이 필요한 이유는 nil의 가능성을 명시적으로 표현해서, nil의 가능성을 문서화 하지 않아도 코드만으로 충분히 표현 가능하다(코드의 문서화 시간을 줄여서 효율성을 높인다)
> 
> 전달받은 값이 옵셔널이 아니라면 nil 체크를 하지 않더라도 안심하고 사용 가능하다
> 
> 예외 상황을 최소화 하는 코딩  


- 옵셔널은 열거형과 재네릭 형의 합작품이다. `Optional = enum + general` 

```swift 

enum Optional<Wrapped> : ExpressibleByNillLiteral {
	case none
	case some(Wrapped)
}


```

> 옵셔널은 이렇게 정의가 되어 있다. 사실 우리가 옵셔널을 사용하게 되었을때 값이 없으면 enum 의 none 부분에 할당되게 되고, 값이 있을때 some부분에 할당 되게 된다.


- 옵셔널의 선언 

```swift
let optionalValue: Optional<Int> = nil
                    꺽쇠 갈호안에, 타입을 넣어야 완전한 문법이지만, 
                    
let optionalValue: Int? = nil


옵셔널은 !, ? 로 표현할수 있음.

```

- implicitly Unwrapped Optional(암시적 추출 옵셔널)

```swift

var optionalValue: Int! = 100

switch optionalValue {
case .none:
	print("값이 없음")
case .some(let value): 
	print(" \(value) 값이 있음")

옵셔널은 Enum(열거형) 이라서, 이렇게 값이 있는지 없는지를 switch 케이스로 확인할수 있다.

```

> 열거형의 값을 뽑을때 옵셔널 바인딩을 해주어야함. 이유는 바인딩을 하지 않으면 nil의 가능성이 있다는것을 내포하기 때문에, 크러쉬가 날수도 잇다.



- '!' : 옵셔널을 사용하면, 일반 변수처럼 nil 을 넣을수도 있는데, 이때 nil과, 다른 타입과 연산을 하게 되면, 크러쉬가 난다..

- '?' : 형식으로 정의를 하면, 기존 변수처럼 사용불가 - 옵셔널과 일반 값은 다른 타입으므로 연산이 불가하다.

> 아마 열거형에서 연산이 어떻게 되는지 한번 확인 해보고 응용할수 있을것 같다..

- 옵셔널 바인딩 if let, guard let

if let, guard let 을 사용해서 옵셔널을 바인딩해서 사용할수 있다.


---

## 8-1. optional Unwrapping(옵셔널 값 추출 하기) 


- Optional binding(guard let, if let) : 옵셔널의 값을 꺼내오는 방법 중 하나 nil 체크와 값을 추출 할수 있음.


```swift

// 여러값의 옵셔널을 바인딩 해줄수 있음.
if-let 변수 = 해당옵셔널 변수, let 바인딩변수 = 해당 옵셔널 변수 { 
	옵셔널 바인딩이후 실행시킬 코드
}else {
   print("값이 nil 일떄 실행")
  }
  
guard let 
-> guard let 의 구문을 함수안에서 사용하면, 함수 내부의 스코프에서 계속 살아 있기 때문에, if let 보다 강력하게 사용할수 있다.

```


- 옵셔널 강제 추출 '!'

> 강제 추출 할때 nil 을 할당할수 있음. 하지만, nil 이 어떤 연산속에 들어가면 app이 바로 죽어버린다.
> 
> '!' 의 사용은 왠만하면 자제하면서 사용하자.


---

## 9. struct(구조체)

> 스위프트에서는 대부분의 정의가 구조체로 정의 되어 있다. apple 의 프레임워크는 대부분 class 로 정의 되어 잇다.

- 구조체 정의 

```swift
struct 이름 {
	구현부
	
	
	}
	
	//구조체 정의
struct Sample {
    var mutableProperty: Int = 100
    let immutableProperty: Int = 100
    
    var `class`: Int = 100
    
    static var typeProperty: Int = 100
    
    //인스턴스 메서드
    func instanceMethod() {
        print("instanceMethod")
        
    }
    
    // 타입 메서드
    static func typeMethod() {
        print("typeMethod")
    }
    
    
}

// 구조체 인스턴스 정의
var mutable: Sample = Sample()

mutable.mutableProperty = 200
// 애초에 값 변경이 안됨..mutable.immutableProperty = 300


// struct 자체에 접근해서 변경할수 있는 값임.. static
Sample.typeProperty = 300

print(Sample.typeProperty)

dump(mutable)

mutable.class = 100

print(mutable)

```

> 구조체에서는 내부에 함수를 만들면, 그것이 인스턴스가 됨. struct 안에서 static 을 정의하면, struct 안에서만 사용할수 있는 property 가됨.
> 
> 시스템 예약어를 변수 이름 으로 사용하려면 `class` 를 사용해서 정의할수 있음.




**구조체는 값 타입이고, 클래스는 참조 타입니다. CallByValue, CallByReference 편 참조 하자**



---

## 10. class(클레스)

**swift 의 class 는 다중상속을 지원 하지 않는다.**


- 타입 메서드 : static, class -> class 내부에서 정의 가능 
	- 상속을 받았을때, 재정의 불가 타입 메서드 static 	- 상속을 받았을때 재정의 가능 타입 매서드 class

---

## 11. Enum(열거형)

**스위프트의 열거형은, 다른 언어의 열거형과 다르게 swift에서 강력함.**

**각각의 케이스가 값으로 취급이 된다**


- Enum 정의

```swift

enum 이름 {
	case 이름1
	case 이름2
	// 각각 하나씩 넣어도 되고, 이렇게 연속적으로 넣어두됨.
	case 이름3, 이름4, 이름5,
	...
	
	enum Fruit: Int {
	case apple = 0
	case grape = 1
	case peach 

}

```

> 원시값은 Hashable 프로토콜을 따르는 모든 타입이 원시값의 타입으로 지정될 수 있습니다.
> 
> 열거형에 원시값을 가지게 해줄수 있다., 문자열도 원시값으로 넣어줄수 있다.
> 
> 문자열은 순차적으로 무엇이 오는지 알수 없기 때문에, 작성하지 않으면, case 값의 이름이 온다..!


- rawValue(원시값)

```swift

표현 문법, 열거형은 케이스 자체가 하나의 값으로 인식이된다!

enum WeekDay {
    case mon
    case tue
    
    // 위처럼 하나씩 정의 해줘도 되지만 아래 처럼 ',' 를 이용해서 여러개를 정의 할수 있다.
    
    case wed, fri
}

var x: WeekDay = WeekDay.fri

x = .fri

//print(x)



// 원시값을 넣으면, 자동으로 원시값을 넣지 않은 곳에 값이 순차적으로 들어감.
enum Friut: Int {
    case apple = 0
    case grape = 20
    case a          // 21
    case b          // 22
    
}

print(Friut.a.rawValue) // 21

// rawValue 값 자체가 index 가 되는것 같음..

var y: Friut! = Friut(rawValue: 0)

print(y) // apple -> 이 값은 옵셔널이다!


```

- Enum 응용

```swift

enum WeekDay1 {
    case mon
    case tue
    case wed, fri
    
    func printWeekDay1(){
        
        switch self {
        case .mon:
            print("\(self) 입니다")
        case .tue:
            print("\(self) 입니다")
        case .wed:
            print("\(self) 입니다")
        case .fri:
            print("\(self) 입니다")
            
        }
        
        
    }
}

// 이런식으로 enum 안에서 넣은 값을 처리해줄수도 있다.
WeekDay1.fri.printWeekDay1() // "fri 입니다"



```

---

## 12. call by Value, call by reference(값 타입과, 참조 타입)


- Class 
	- 전통적인 OOP 관점에서 클래스
	- 단일 상속
	- (인스턴스/타입) 메서드
	- (인스턴스/타입) 프로퍼티
	-  **참조 타입** 
	-  apple 프레임워크의 대부분의 큰 뼈대는 모두 클래스로 구성 

- Struct
	- c 언어 등의 구조체보다 다양한 기능
	- 상속 불가
	- (인스턴스/타입) 메서드
	- (인스턴스/타입) 프로퍼티
	- **값 타입**
	- Swift의 대부분의 큰 뼈대는 모두 구조체로 구성 

- Enum
	- 다른 언어의 열거형과는 많이 다른 존재
	- 상속 불가
	- (인스턴스/타입) 메서드
	- (인스턴스/타입) 프로퍼티
	- **값 타입**
	- Enumeration
	- 유사한 종류의 여러 값을 유의미한 이름으로 한곳에 모아 정의
	- 예) 여일, 상태값, 월(Month) 등
	- 열거형 자체가 하나의 데이터 타입, 열거형의 case 하나하나 전부 하나의 유의미한 값으로 취급



| * |Class | Stuct | Enum |
| :------------ | :-----------: | :-----------: | -----------: |
| type | Reference | Value | Value1 |
| Subclassing | O | X | X |
| Extension | O | O | O |


**구조체는 언제 사용 하나?**

- 연관된 몇몇의 값들을 모아서 하나의 데이터타입으로 표현하고 싶을때
- 다른 객체 똔느 함수 등으로 전달될 때 *참조가 아닌 복사를 원할때* 
- 자신을 상속할 필요가 없거나, 자신이 다른 타입을 상속받을 필요가 없을때
- Apple 프레이뭐크에서 프로그래밍을 할 때에는 주로 클래스를 많이 사용 

 -> 구조체는 어떤 유의미한 값들을 연속적으로 핸들링 해야할때 사용하면 좋다. 
 
```swift

struct CallByValue {
    var value = 1
}

class CallByReference {
    var value = 1
}



var firstCallByValue = CallByValue()
var secondCallByValue = firstCallByValue

withUnsafePointer(to: &firstCallByValue) { pA in
    print(pA)  }

withUnsafePointer(to: &secondCallByValue) { pA in
    print(pA)  }


secondCallByValue.value = 100

print(firstCallByValue.value, secondCallByValue.value) // 1, 100

var firstCallbyReference = CallByReference()
var secondCallByReference = firstCallbyReference

secondCallByReference.value = 200



withUnsafePointer(to: &firstCallbyReference) { pA in
    print(pA)  }

withUnsafePointer(to: &secondCallByReference) { pA in
    print(pA)  }
    
  print(firstCallbyReference.value, secondCallByReference.value)  // 200, 200 



```

**포인터가 Value 의 녀석들은 메모리 주소를 정확하게 표시하는데, Reference 타입의 녀석들은 값을 정확하게 이상하게 찾아준다.. 뭐지..?...**

 

- Call By Value, Call By Reference 차이를 정확하게 알수 있는 예제

```swift

struct CallValue {
    var n: String = "1"
}

class CallReference {
    var n: Int = 10
    
}

var tester: CallValue = CallValue()


func CheckValue(x: CallValue) {
    
    var testValue = x
    testValue.n = "100"
}

CheckValue(x: tester)

print(tester.n) // 10



func CheckReference(x: CallReference) {
    
    var testReference = x
    testReference.n = 100
}

var tester1: CallReference = CallReference()

CheckReference(x: tester1)

print(tester1.n) // 100, inout 을 사용하지 않았는데, 값이 변햇다.

```

> Data types in Swift
> 
> 사실 스위프트의 타입들은 구조체로 되어 있다. public struct Int, public struct Double 

---

## 13. Closure

**클로저는 일급함수라서, 변수, 상수 등으로 저장, 전달인자로 전달이 가능하다. 함수는 사실 클로져 인데, 이름이 있는 클로져 이다**

- closure 문법

```swift
{ (매개변수 목록) -> 반환 타입 in 
	실행코드
	}
	
```
	
> 함수는 클로져의 일종 이기때문에, 클로져로 만들것 자체를 인자로 저장, 전달 할수 있다.
> 
> 클로져는 주로 함수의 전달인자로서 사용을함. 

- clousre 설계

```swift


func account(name: String, money: Int) -> (Int) -> Int {
    let myName = name
    var myMoney = money
    
    func addMoney(m:Int) -> Int{
        
        myMoney += m
        print(myName, myMoney)
        return myMoney
    }
    
    return addMoney(m:)
    
}

var test = account(name: "민준", money: 1000)
//dump(test)



for i in 1...100 {
    
    test(100)
    
}



```

---

## 13-1 Closure 조금 깊게 들어가기

1. 후행 클로져
2. 반환타입 생략
3. 단축 인자 이름
4. 암시적 반환 표현 

> closure 는 너무나 다양한 표현법이 있기 때문에, 사용했을때 남들이 이해할수 있는 정도 까지 만든다. 

- 후행 클로져

```swift

// 변수들에 클로져를 정의 할수 있다.
let add: (Int, Int) -> Int

add = { (a:Int, b:Int) in
    return a+b
}

func calAdd(x: Int, y:Int, Method: (Int, Int) -> Int) -> Int {
    
    return Method(x,y)
}


//뭐 이녀석은 마지막에 전달될 인자로 클로져구나... 라고 이해해야 함.. 
var testClosure = calAdd(x: 10, y: 20) { (left: Int, right: Int) -> Int in
    return left * right
}

dump(testClosure)

```

- 반환 타입을 생략 해줄수 있다.

> 클로져는 method 의 전달인자가 뭔지 알고 잇을때, 반환 타입을 생략 해줄수 있다. 
> 

```swift


//인자 목록을 생략해줄수 있다. 생략이 가능한이유는, 이미 앞에서 인자 두개를 받는다는것을 알고 있기 때문이다..!
var testClosure = calAdd(x: 10, y: 20, Method: {
    return $0 * $1
})
    
dump(testClosure)

잘 보면, 변수에 () 정의 안에 클로져 까지 집어 넣었다.
```

- 당연히 후행 클로저와 함께 사용할 수 있다.

```swift

//인자 목록을 생략해줄수 있다. 생략이 가능한이유는, 이미 앞에서 인자 두개를 받는다는것을 알고 있기 때문이다..!
var testClosure = calAdd(x: 10, y: 20){
    return $0 * $1
}
    
dump(testClosure)

```


- 암시적 반환 표현

> 클로저가 반환하는 값이 있다면 
> 
> 클로저의 마지막 줄의 결과값은 암시적으로 반환값으로 취급을 한다.


```swift

이런식으로 반환 값을 축약할수 있음. 클로져에서는 마지막 줄의 값을 암시적으로 반환값을 취급을 한다..!!
var testClosure = calAdd(x: 10, y: 20){ $0 * $1 }


```

---



 
 


