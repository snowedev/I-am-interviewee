# 프로토콜은 무엇인지 설명하시오

## 함께보면 좋은 답변
* [Delegate](./DelegatePattern.md)

## 참고한 좋은 글
* [Protocol(1)](https://zeddios.tistory.com/255)
* [Protocol(2)](https://zeddios.tistory.com/256)
* [왕초보를 위한 delegate](https://zeddios.tistory.com/8)


## Answer

프로토콜은 서로간의 '약속'이다.

좀 더 자세히 말하면 `특정 작업이나 기능에 적합한 메소드, 프로퍼티 및 기타 요구사항의 청사진(blue print)를 정의하는 것`을 의미한다.


`선생님` 이라는 프로토콜이 있다고 가정한다.

변수에는 가르치는 과목이 있고
teaching()과 giveHomework()라는 메서드가 있다고 했을 때

Protocol은 과목, teaching(), giveHomework()를 해야한다는 약속만 구성할 뿐 실질적인 행동을 정의하지는 않는다.

그 실질적인 행동 즉, 구현은 선생님 프로토콜을 준수하는 곳에서 이루어진다.

과학선생님, 국어선생님은 모두
각각 "과학", "국어"라는 과목명을 가질 것이며, teaching()과 giveHomework()를 구현해야한다.

Delegate는 프로토콜로 구현이 된다.


<br>

### 프로토콜의 구현(중요)--------------

클래스 뿐만 아니라 구조체, 열거형 또한 프로토콜을 채택하여 특정 기능을 실행하기 위한 프로토콜의 요구사항을 구현할 수 있다.

### 📌 프로퍼티 요구 사항 
1. 프로토콜은 프로퍼티가 저장 프로퍼티인지, 연산 프로퍼티인지 명시 하지 않는다.

2. 대신 { get } 인지 { get set }인지 명시해주어야 한다.(setter만 있는건 없다).
    * { get }만 요구할 경우 모든 종류의 프로퍼티(연산 프로퍼티든 저장 프로퍼티든)에서 충족 & 필요하다면 setter를 추가적으로 구현 가능
    * { get set }일 경우에는 상수(let) 저장 프로퍼티를 사용하거나 { get }만 구현해서는 안된다.

3. 프로퍼티 요구사항은 항상 var로 선언되어야 한다.
4. 타입 프로퍼티는 **프로토콜 내에서** `static` 키워드를 붙여야 한다.(`class` X)

### 📌 메소드 요구사항

1. '정의'만 이루어질 것
    ```swift
    protocol SomeProtocol {
        func someMethod()
        func anotherMethod(name: String, age: Int) -> Int
        func protocolMethod() -> String
    }
     ```

2. 타입 프로퍼티를 프로토콜에 정의한 경우, `static` 키워드를 접두사로 붙어야 한다.
3. [mutating](./mutating.md)
    * 인스턴스에 속한 메서드를 수정, 변경하기 위해서는 func 앞에 mutating이라는 키워드를 두어야 한다.
    * 단, 프로토콜을 준수하는 주체가 구조체와 열거형일 경우만 mutating 키워드를 붙이며 클래스의 경우에는 붙이지 않아도 된다. 그 이유는 Value Type인 구조체와 열거형과 달리 클래스는 Reference Type으로 본래부터 메소드 내에서 프로퍼티 값의 변경이 가능하기 때문에 mutating 키워드를 생략할 수 있다.

    ```swift
    protocol SomeProtocol {
        mutating func SomeMethod(_ num: Int)
    }

    struct SomeStruct: SomeProtocol {
        var x = 0
        mutating func SomeMethod(_ num : Int) {
            x += num
        }
    }

    class SomeClas: SomeProtocol {
        var x = 0
        // mutating 키워드 생략
        func SomeMethod(_ num: Int) [
            x += num
        ]
    }
    ```

    ### 📌 이니셜라이저 요구사항
    
    1. 프로토콜은 프로토콜을 준수하려는 타입에게 특정한 이니셜라이저를 구현하도록 요구할 수 있다. 일반 이니셜라이저와 동일하지만 `{ body }` 를 작성하지 않는다
        * 프로토콜을 준수하는 주체가 class일 경우 이니셜라이저 앞에 `required`를 사용하면 프로토콜을 준수하는 클래스의 모든 하위 클래스들도 이니셜라이저 요구사항 구현을 보장받을 수 있다.
        * `final` 키워드를 사용한다면 override 불가하기 때문에 `required`사용할 필요 없음
        * 같은 이유에서 구조체는 `required` 필요 없음 

        ```swift
        protocol SomeProtocol {
            init(someParameter: Int)
        }

        class SomeClass: SomeProtocol {
            // required를 사용하면 프로토콜을 준수하는 클래스의 모든 하위 클래스들도 이니셜라이저 요구사항 구현을 보장받을 수 있다.
            required init(someParameter: Int) {
                // initializer implementation goes here
            }
        }

        struct SomeStruct : SomeProtocol {
            init(someParameter: Int) {
                // initializer implementation goes here
            }
        }
        ```