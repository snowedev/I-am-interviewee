# Retain Count 방식에 대해 설명하시오.

## 참고한 좋은 글
* [Reference count in ARC-stackoverflow](https://stackoverflow.com/questions/40799211/reference-count-in-arc)
* [ARC뿌시기-날진님](https://sujinnaljin.medium.com/ios-arc-뿌시기-9b3e5dc23814)


## Answer

ARC는 **compile time**에 자동으로 **retain**, **release** 메서드를 적절한 위치에 삽입하는 방식으로 동작합니다.

<img width=100% src="https://user-images.githubusercontent.com/42789819/114403755-07c0fd80-9be0-11eb-9717-f74d6bf8fe35.jpg">

이런식으로 complie time에 삽입이 이루어지면, runtime때 인스턴스의 생성, 참조가 일어날 때 마다 메서드가 실행되며 자동으로 Retain Count가 이루어지는 방식입니다. 그러다가 Count가 0이 되면 `deinit`을 통해 메모리가 해제됩니다.

Retain Count(RC)는 인스턴스의 생성 시 +1이 되고 해당 인스턴스가  참조될 때 +1이 됩니다. 반대로 참조가 해제되면 -1이 됩니다. 
> strong이 아니라 weak으로 참조할 경우 RC는 증가하지 않습니다.


## 실습

클래스의 인스턴스가 생성될 때와 해지될 때 로그 출력
```swift
class Person {
    let name: String
    init(name: String) {
        self.name = name
        print("\(name) is being initialized")
    }
    deinit {
        print("\(name) is being deinitialized")
    }
}
```


3개의 변수 모두 옵셔널 => 초기값 nil
```swift
var reference1: Person?
var reference2: Person?
var reference3: Person?
```

Person 인스턴스 생성, RC = 1
```swift
reference1 = Person(name: "John Appleseed")
// Prints "John Appleseed is being initialized"
```

Person 인스턴스 RC = 3
```swift
reference2 = reference1
reference3 = reference1
```

Person 인스턴스 RC = 1
```swift
reference1 = nil
reference2 = nil
```

Person 인스턴스 RC = 0, 메모리 해지
```swift
reference3 = nil
// Prints "John Appleseed is being deinitialized"
```
