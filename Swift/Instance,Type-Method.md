# Instance 메서드와 Type 메서드의 차이점을 설명하시오.


## 참고한 좋은 글
* [Swift - 타입 메소드 & 인스턴스 메소드](https://jiseobkim.github.io/swift/2018/10/05/swift_basic-타입-메소드-&-인스턴스-메소드.html)
* [Method-zeddios](https://zeddios.tistory.com/258)
* [The Swift Programming Language-Methods](https://docs.swift.org/swift-book/LanguageGuide/Methods.html)

## Answer

### **`1. 인스턴스 메서드`**
> **정의**: 특정 타입의 인스턴스에서 호출되는 Method.

아래의 코드는 globalThing()이라는 전역 함수가 하나 있고, 각각 firstInstance, secInstance 라는 인스턴스 메서드를 가진 FirstClass, SecondClass가 있다. 


전역 함수는 어디에서든 접근이 가능하지만 클래스 내의 메서드는 해당 클래스의 인스턴스를 생성하고 호출해야 한다. 만약 다른 클래스의 메서드를 호출하고 싶다면 해당 클래스를 상속해야 한다.

```swift
// MARK: - 전역 함수
func globalThing() -> String {
    return "나는 전역 함수"
}

class FirstClass {
    var something: String?
    
    // MARK: - 인스턴스 메서드
    func firInstance() -> String {
        return "Instance!!"
    }
}

class SecondClass {
    var anything: Int?
    
    // MARK: - 인스턴스 메서드
    func secInstance() -> String {
        return "Second Instance!!"
    }
}

print(globalThing()) // 나는 전역 함수

var first = FirstClass()
print(first.firInstance()) // MARK: - 인스턴스 메서드 호출
// Result: Instance!!

var sec = SecondClass()
print(sec.secInstance()) // MARK: - 인스턴스 메서드 호출
// Result: Second Instance!!
```

> #### 인스턴스 메서드는 mutating 키워드와도 연관성이 깊다. 같이 보면 좋음!   
> #### [Modifying Value Types from Within Instance Methods](./mutating.md)


<br>
<br>

### **`2. 타입 메서드`**
> **정의**: 타입 자체에서 호출되는 Method.


**타입 메서드의 키워드로는 `static`과 `class`가 있다. 이 두 키워드가 붙은 메서드는 인스턴스의 생성 없이 사용이 가능하다.** 
> class 키워드는 class 에서만 사용가능하다.


* `class` keyword  

    * 장점: 동물 울음소리 내기와 같은 반복적으로 사용 해야하는 경우에 유용하게 쓰일 수 있다. 인스턴스를 따로 생성하지 않기 떄문에 메모리의 걱정을 할 필요도 없다.

    * 단점: 하지만 확장성이 구리다. 당연한 값들로만 리턴을 하고, 초기화가 없기 떄문에 변수를 쓰지 못한다.
    ```swift
    class SomeClass {
        // MARK: - Instance Method
        func useInstance() -> String {
            return "Instance!!"
        }

        // MARK: - Type Method
        class func getSomething() -> String {
            return "Type!!"
        }
    }

    var some = SomeClass()
    print(some.useInstance()) // Instance!! , 인스턴스 메서드
    
    print(some.getSomething) // error    
    print(SomeClass.getSomething()) // "Type!!" , 타입 메서드
    ```

* `static` keyword

    `class` 키워드와의 차이점은 override 가능 여부이다. `class` 키워드는 해당 클래스를 상속 받았을 경우 **오버라이드가 가능하다.**

    ```swift
    class SubClass: SomeClass {
        override class func getSomething() -> String {
            return "wow"
        }
    }

    print(SubClass.getSomething()) // wow
    ```

    또한, class func을 하위 클래스에서 static func로 override 하는 것도 가능하다.

    ```swift
    class SubClass: SomeClass {
        override static func getSomething() -> String {
            return "wow"
        }
    }

    print(SubClass.getSomething()) // wow
    ```

    하지만 SomeClass의 getSomething 키워드를 `static`으로 변경해준다면?

    ```swift
    class SomeClass {
        func useInstance() -> String {
            return "Instance!!"
        }

        // static으로 선언
        static func getSomething() -> String {
            return "Type!!"
        }
    }

    class SubClass: SomeClass {

        // error: Can not override static method
        override class func getSomething() -> String {
            return "wow"
        }
    }
    ```

    **static method는 오버라이드가 불가하다는 오류 메시지를 내뿜는다.**
    > `class` 키워드도 앞에 `final`키워드를 붙이게 되면 override를 막을 수 있다.

<br>
<br>

### `관련 문제`
스위프트 Method에 관한 설명이다. 아래 보기 중에서 **맞는 것을 모두 고르시오.**

~~① Instance Methods와 Type Methods는 특정 타입의 인스턴스에서 호출되는 Method를 뜻한다.~~

~~② Type Methods 중 class func을 하위 클래스에서 static func로 override할 수 없다.~~

③ Type Methods 중 static func을 하위 클래스에서 class func로 override할 수 없다.

④ Type Methods 중 class func은 class에만 선언이 가능하며, struct, enum에서는 선언이 불가능하다.

<br>
<br>

### **`+ mutating static은 왜 안될까?`**
* [Mutating in Swift](https://devmjun.github.io/archive/Mutating_Struct)

static 변수나 함수는 object 생성없이 사용할 수 있다. 하지만 mutating은 object 그 자체를 바꾼다는 의미이므로 같이 사용할 수 없다.
