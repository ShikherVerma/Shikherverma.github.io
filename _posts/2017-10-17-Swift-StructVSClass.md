---
layout:     post
title:      "Swift. 값 타입 VS 참조타입 비교하기"
subtitle:   "Class, Struct 뜯어보기"
date:       2017-10-17 00:39:00
author:     "MinJun"
header-img: "img/tags/Swift-bg.jpg"
comments: true
tags: [Swift]
---

## Swift. 값 타입 VS 참조 타입 

---

## Struct VS Class 

> Struct 와 Class 를 설명할때 기계적으로 Struct는 값 타입이고, Class 는 참조 타입입니다. 라고 이야기 합니다. 저는 그 이야기를 들었을때 그렇다면 왜? 일까 라는 생각이 먼저 떠올랐고, 눈으로 보면 좀더 쉽게 이해할수 있지 않을까를 생각으로 출발 했습니다. 
> 

---

## Struct, Class 기본 구조 



```swift

// class 정의
class VideoMode {var resolution = Resolution()var interlaced = falsevar frameRate = 0.0var name: String?}

// Struct 정의 
struct Resolution {}var width = 0var height = 0

// Class 인스턴스 생성
var value = VideoMode()

// Struct 인스턴스 생성

var value = Resolution()

  -> 동일하게 인스턴스를 생성합니다. 하지만, 인스턴스를 생성할때 각각의 인자를 전달하는 방식이 달라지게 됩니다. 

```



---

## Struct VS Class

- Class는 참조 타입이며, Struct는 값 타입 입니다.(사실 Class와 Struct의 차이는 이 차이때문에 나머지 차이들이 파생되어서 생기게 됩니다.)<br>
- Class는 상속을 통해 부모클래스의 특성을 상속받을수 있습니다.- Class는 Type Casting을 사용할수 있습니다.(Struct 불가)- Struct의 프로퍼티는 instance가 `var`를 통해서 만들어야 수정 가능하다.- Class는 Reference Counting을 통해 인스턴스의 해제를 계산합니 Calss는 deinitializer를 사용할수 있습니다.

> 여러가지 특징이 있지만, 여기에서는 값, 참조 타입에 대해서만 다루겠습니다.

---

## 값 타입 VS 참조 타입

- Memory 구조 

![screen](/img/posts/ClassVSStruct-5.jpg) <br>

> 메모리는 그냥 사용하면 비효율적이기때문에, 어느정도 논리적으로 구분을 시켜서 사용하는것이 효율적이기 떄문에 메모리의 구역을 나눈다(stack, heap, data, code)



- Memory 에 Class, Struct가 어떻게 들어가는지 확인해보자!

**Struct 인스턴스**

var num: Int = 4
var num2: Int = 5

**Class 인스턴스**

let lb:UIView = UIVIew()

> 각각 메모리에 어떻게 들어가는지 확인해보자


| Struct Instance  | Class Instance | 
| :------------ | -----------: | 
| ![screen](/img/posts/ClassVSStruct-6.jpg) | ![screen](/img/posts/ClassVSStruct-7.jpg) | 

- 결과 

| 비교 결과  |
| :------------ |
|![screen](/img/posts/ClassVSStruct-8.jpg) |




> Struct로 인스턴스로 만들어진 녀석들을 STACK, CODE 영역에 저장이 됬습니다.
> 
> 이유는 Struct는 값 타입이라서, 기본적으로 Struct 인스턴스를 생성할때, 인스턴스의 값을 다른 공간에 새롭게 복사해서, 복사된 인자를 전달합니다. 그렇기 때문에 기본적으로 Stack에 쌓이게됩니다. 하지만 Struct의 값이 커지게 되면 Heap 영역에 저장 되기도 한다. 
> 
> Class Instance 는 HEAP 영역에, 그 인스턴스의 주소값(이 인스턴스가 가리키는 '값' 이라고 표현하는게 조금더 정확할 것 같다)은 Stack 영역에 쌓였다.
> 
> 하지만 엄밀하게 얘기하면 Swift의 모든 타입은 변수들은 heap 저장되어서 참조 타입이다. 사실 Struct도 인스턴스로 만들어져서 어떤 인스턴스를 '참조' 하고 있을 테니 말이다. 
> 
> 이렇게 생각하게 되면 엄밀하게 내부에서 어떻게 동작하는지 뜯어서 눈으로 확인해야 하지만 실질적으로 그렇게 확인하는게 시간적으로나, 능력(?) 적으로나 매우 까다롭다. 그래서 값, 타입과 참조 타입 두개로 나누어서 생각하기 보다는, 참조 할때는, Heap 영역에 있는 어떤 '값'을 바라보고 있고, 값으로서 사용될떄는 Stack Frema 안에서 사용 되어 진다고 생각하자.

---

## Struct VS Class 뜯어 보기 

> 위의 예제만으로는 감이 오지 않습니다. Struct 와 Class 를 뜯어서 눈으로 확인하는 작업을 해봅시다.

- Struct Vs Class 차이 예제 코드

```swift

class Value {
    var x: Int = 0
}

func changeValue(variable: Value) {
    var otherValue = variable
    otherValue.x = 100
    print(otherValue.x) // 100
}

var rootInstance: Value = Value()
print(rootInstance.x) // 0

changeValue(variable: rootInstance)

print(rootInstance.x) // 100 

 -> 함수의 stack Frema 속에서 참조된 rootInstance.x 의 값이 변경되었다.
 
 
 struct NotChangeValue {
    var x: Int = 0
}

func notChangeValue(StructVariable variable: NotChangeValue) {
    var otherValue = variable
    otherValue.x = 100
    print(otherValue.x) // 100
}

var rootStructInstance: NotChangeValue = NotChangeValue()
print(rootStructInstance.x) // 0

notChangeValue(StructVariable: rootStructInstance)
print(rootStructInstance.x) // 0

-> 같은 형태(?)로 정의 했는데, 결과값이 다르게 나왔다. 간단하게 설명하면 struct는 인스턴스 생성시에 값을 복사해서 인자로 전달했고, class 는 인자로 전달될때 값이 아니라, 그 값의 주소값이 전달이 되어서 위와 같은 결과를 가져온다고 할수 있다



```

> 하지만 여기까지 해봐도 잘 와닿지가 않는다. 그래서 메모리 주소와, heap 의 참조하는 값을 직접 눈으로 확인해보자!


- memory 주소 값, heap 주소 값 확인 

`withUnsafePointer` 이 명령어는  `메모리` 주소 값을 가져 옵니다. <br>

> 여기서 조금 혼동되는게 변수를 선언 할때 메모리 주소와, 변수에 들어 있는 값이 들어 있는 주소 값이 같을수도 있고, 다를 수도있습니다.(엄밀하게 표현하면 정확한 표현은 아니지만, 이렇게 생각하면 조금 쉽게 접근 할수 있습니다)

**디버깅 창에서 heap, Stack 메모리 주소 보는방법**

1. <br>

| *  | * |
| :------------ | -----------: | 
| ![screen](/img/posts/ClassVSStruct-10.jpg) | ![screen](/img/posts/ClassVSStruct-11.jpg) | 


> 1. 체크 포인트를 설정후 실행 합니다
> 
> 2. 디버깅창 왼쪽 부분에서 memory 주소를 확인할 변수를 오른쪽 마우스로 클릭한후 `View Memory Of '변수명'` 을 클릭 합니다.

2. <br>

![screen](/img/posts/ClassVSStruct-12.jpg)

> 헥스 코드로 된 값을 볼수 있는데 화면 중앙에 잘 보시면 `Address` 값을 확인할수 있습니다. 


- 메모리 주소값 비교 

```swift

// Struct 정의 
struct NotChangeValue {
    var y = 0
}

// Struct 인스턴스 생성  -> 인자 전달 
var rootStructInstance = NotChangeValue(y: 10)
var structInstance = rootStructInstance


withUnsafePointer(to: &rootStructInstance) { Pa in print(Pa) } // 0x0000000100442218
                                             // debug_window memory addres 0x100442218
                                                            
withUnsafePointer(to: & structInstance) { Pa in print(Pa) } // 0x0000000100442220
                                            // debug_window memory addres 0x100442220

-> `withUnsafePointer` : 메모리 주소값을 출력, 둘은 서로 다른 메모리 주소값에 정의 되어 있고, 
-> debug_window : 통해서 서로 다른 참조를 하고 있는것을 확인할수 있다.


// class 정의 
class changeValue {
    var x = 0
    
    init(x: Int) {
    self.x = x
    
 }

var rootInstance = changeValue(x: 10)
var classInstance = rootInstance

->


withUnsafePointer(to: & rootInstance) { Pa in print(Pa) } // 0x00000001004420e0
                                                            // debug_window memory addres 0x102824af0
                                                            
withUnsafePointer(to: & classInstance) { Pa in print(Pa) }  // 0x00000001004420e8
                                                            // debug_window memory addres 0x102824af0
                                                            
-> `withUnsafePointer` : 메모리 주소값을 출력, 둘은 서로 다른 메모리 주소값에 정의 되어 있고, 
-> debug_window : 서로 같은 heap 영역의 인스턴스를 참조하고 있는것을 확인할수 있다.                                                            


```





