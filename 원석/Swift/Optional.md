# 옵셔널 이란 무엇인지 설명하시오


## Answer

Optional(옵셔널): 값이 있을 수도 있고 없을 수도 있는 것

```swift
let optionalConstant: Int? = nil // possible
let someConstant: Int = nil // error
```

### 📌 Definition
```swift
enum Optional<Wrapped> : ExpressibleByNilLiteral {
		case none  //값이 없는 것
		case some(Wrapped) //값이 있는 것 둘 다 표현 가능
}

// 둘 다 사용 가능
let optionalValue: Optional<Int> = nil
let optionalValue: Int? = nil
```


<br>

### 📌 옵셔널을 사용하는 이유
* 명시적 표현
    1. nil의 가능성을 코드만으로 표현 가능
    2. 문서/주석 작성 시간 절약

* 안전한 사용
    1. 전달받은 값이 옵셔널이 아니라면 nil 체크를 하지 않고 사용 가능
    2. 예외 상황을 최소화 하는 안전하고 효율적인 코딩


<br>

### 📌 옵셔널 표현

* **`!`**(Implicitly Unwrapped Optional)
    * 임시적 추출 옵셔널
    * 기존 변수처럼 사용 가능

* **`?`**(General Optional)
    * 기존 변수처럼 사용 불가 - 옵셔널과 일반 값을 다른 타입이므로 연산 불가


<br>

### 📌 옵셔널 바인딩(Optional Binding)
* if-let
    * nil 체크 & 안전한 값 추출
    ```swift
    var myName: String? = "wonseok"
    var yourName: String? = nil

    if let name = myName, let friend = yourName {
            print("\(name) and \(friend)")
    }
    // yourName이 nil이기 때문에 실행되지 않는다

    yourName = "sujeong"
    if let name = myName, let friend = yourName {
            print("\(name) and \(friend)")
    }
    // wonseok and sujeong
    ```
* Force Unwrapping(강제추출)
    * nil 체크 없이 강제로 값을 꺼내는 방식, 값이 없을 경우 런타임 오류가 발생하여 앱이 멈추기 때문에 추천되지 않는 방식 
    ```swift
    var myName: String? = nil
    print(myName!) // error 값이 없어서 런타임 오류
    ```


<br>

### 📌 옵셔널 체이닝(Optional Chaining)

* 옵셔널 체이닝

    ```swift
    // 사람 클래스
    class Person {
        var name: String
        var job: String?
        var home: Apartment?
        
        init(name: String) {
            self.name = name
        }
    }
    // 사람이 사는 집 클래스
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

    let wonseok: Person? = Person(name: "wonseok")
    let apart: Apartment? = Apartment(dong: "101", ho: "202")
    ```

    이런 코드가 있을 때 Person A의 Apartment의 경비원 직업이 궁금하다면?

    ```swift
    // 옵셔널 체이닝을 사용하지 않는다면 이렇게 됨
    func guardJob(owner: Person?) {
            if let owner = owner { // 1.사람의 존재 여부
                    if let home = owner.home { // 2.그 사람의 집 여부
                            if let 'guard' = home.guard { // 3.그 사람의 집의 경비원 여부
                                    if let guardJob = 'guard'.job { // 4.그 사람의 집의 경비원의 직업 여부
                                                print("우리집 경비원의 직업은 \(guardJob)입니다")
                                    } else {
                                                print("우리집 경비원은 직업이 없어요")
                                    }
                            }
                    }
                }
        }
    ```

    ```swift
    // 옵셔널 체이닝을 사용한다면
    func guardJobwithOptionalChaining(owner: Person?) {
                    if let guardJob = owner?.home?.guard?.job {
                                    print("우리집 경비원의 직업은 \(guardJob)입니다")
                    } else {
                                                print("우리집 경비원은 직업이 없어요")
                    }
    }

    guardJobwithOptionalChaining(owner: wonseok)
    // 우리집 경비원은 직업이 없어요
    ```

* nil 병합 연산자
    - Optional **`??`** Value
    - 옵셔널 값이 **nil**일 경우, 우측의 값을 반환합니다.
    ```swift
    var house: String

    house = wonseok?.home?.owner?.name ?? "본인"
    // -> '??' 앞의 값이 nil이라면 "본인"을 할당해달라!
    print(house) // 본인 // 위에서 '본인'을 넣어줘서 nil이 아님
    ```