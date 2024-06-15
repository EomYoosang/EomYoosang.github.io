---
layout: post
title: 코틀린을 배워보자! 코틀린 문법 1
subtitle: 코틀린 문법을 배워보자!
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [Kotlin]
author: EomYoosang
---

# 코틀린을 배워보자! 코틀린 문법 1

이번 포스트는 코틀린 문법 정리입니다. 장황한 설명보단 간단히 정리할게요.

## 1. 변수

- var : 값 변경 가능
- val : 선언 시 초기화 (자바의 final)

```
var a: Int // 자료형 선언시 -> 변수: type
a = 123    // var타입은 선언 후 변경 가능
```

```
val a: Int = 123    // 선언 시 초기화
a = 456             // 선언 후 값 변경 불가
```

## 2. 형변환

**to변수()** 를 통해 형변환

```
var a: Int = 123
var s: String = a.toString()
```

## 3. 배열

```
fun main(){
	//int형으로 1 2 3 4 배열 생성
    var arr: Array<Int> = arrayOf(1, 2, 3, 4)
    //type 생략 가능
    var arr2 = arrayOfNulls<Int>(5)
    //Any는 데이터 타입의 최상위(어느 데이터든 다 들어갈 수 있음)
    var anyArr : Array<Any> = arrayOf(1, "awd", 3.2, 4)	
}
```

## 4. 함수

**기본함수**
```
fun main(){
    print(add(1,2,3))
}

//함수의 기본형 fun 함수이름(매개변수: type): 리턴 타입
fun add(a: Int, b: Int, c: Int): Int {
    return a + b + c;
}
```

**단일표현식 함수**
```
// 반환 타입이 Int라고 유추 가능
fun add(a: Int, b: Int, c: Int) = a + b + c
```

## 5. 조건문

조건문은 다른 언어와 동일

**if**
```
if (a > b) {
    println("a > b")
}
```

**when** : switch와 유사
```
when(a) {
    1 -> println(1)
    "one" -> println("one")
    else -> println("else")
}
```

자바13의 switch처럼 값 설정에 이용 가능
```
var b
b = when(a) {
    1 -> 1
    "one" -> "one"
    else -> "else"
}
```

6. 반복문

**while** : 다른 언어와 동일
```
while(i>0) {
    i--
    println(i)
}
```

**for**
```
for(i in 0..3){         // 0,1,2,3
    print(i)
}


for(i in 3 downTo 0){   // 3,2,1,0
    print(i)
}

for(i in 0..5 step 2){  // 0,2,4 (2씩 증가)
    print(i)
}

for(i in 'a'..'e'){     // 'a', 'b', 'c', 'd', 'e'
    print(i)
}
```

**continue, break**: 동일

## 6. 클래스
### 기본형
클래스 형태는 **class 클래스 이름(속성 이름: type)**
**생성자를 따로 안만들어도 됩니다!** 대신 클래스 멤버변수를 생성자 파라미터처럼 표시합니다.
> 원래 `class B private constructor(val name: String, val age: Int)` 형태에서 constructor가 생략되었기 때문입니다.

```
class Prs(var name:String, val birth:Int){
    fun introduce(){
        println("$name $birth")
    }
}
```

### init
아래와 같이 **init**을 사용하면 생성자 실행 시 행동을 추가할 수 있으며 init은 **여러 개 존재**할 수 있습니다.
```
class Prs(var name:String, val birth:Int){
    init{
        println("${this.name} ${this.birth}")
    }
    init{
        println(1)
    }
}
```

### 부생성자
클래스 내에 선언된 부생성자는 this를 통해 주생성자로 인자를 넘겨줍니다.
주생성자의 `constructor` 키워드는 생략할 수 있지만 부생성자에선 불가능합니다.
```
class Prs(var name:String, val birth:Int){
    // this를 사용해 기본 생성자로 값을 넘겨줘야 한다.
    constructor(name:String): this(name, 23)
    init{
        println("${this.name} ${this.birth}")
    }
}

var ys = Prs("유상") // ys.age = 25
```

> `Init`과 `부생성자` 중에선 `Init`이 먼저 호출됩니다.


### 상속
클래스 상속을 위해선 부모 클래스에 open 키워드가 있어야 합니다.
아래 Animal 클래스는 상속이 가능하며 함수또한 오버라이드 할 수 있습니다.
```
open class Animal(var name:String, var age:Int, var type:String){
    open fun introduce(){
        println("${this.name} ${this.age} ${this.type}")
    }
}
class Dog(var name1: String, var age1: Int) : Animal(name1, age1, "강아지"){
    fun introduce1(){
        super.introduce()
    }

    override fun introduce() {
        println("override")
    }
}
```

### 추상 클래스
### 인터페이스
## enum 클래스
## Collection List
## 다형성 as
## Generic


## 결론
오늘은 다른 언어나 자바와 겹치는 코틀린의 기본 문법을 알아봤습니다.
다음 포스트에서는 **@label**, 널 세이프티, 고차함수와 람다함수, 스코프 함수 등에 대해 알아보겠습니다.