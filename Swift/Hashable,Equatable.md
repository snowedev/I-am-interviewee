# Hashable이 무엇이고, Equatable을 왜 상속해야 하는지 설명하시오.

## 참고
* [Apple Developer - Hashable](https://developer.apple.com/documentation/swift/hashable/)

## Answer

#### 사전 지식

> hash란, 해시 함수에 의해 얻어지는 값으로 해시값(hashValue), 해시코드, 해시 체크섬으로도 불립니다.

> hash function, 임의의 길이의 데이터를 고정된 길이의 데이터로 매핑하는 함수입니다.

> HashTable에서 hash값을 찾기 위해선 key가 필요하고 이 key는 unique 해야 합니다.


#### Hashable

```swift
protocol Hashable
```

공식문서에 따르면, Hashable이란 **정수형의 해시값을 제공하는 "Hasher"로 해싱될 수 있는 타입**을 의미합니다.

* 우리는 set이나 dictionary의 key값으로 Hashable 프로토콜을 준수하는 타입을 사용할 수 있습니다. 대다수의 standard library 내의 타입들은 Hashable을 준수하고 있죠.(String, Integers, Float, Boolean 등)
* 그 외 다른 타입들에는 Optional, Array, range 등이 있는데 이들의 type argument들이 동일하게 구현될 때 자동으로 Hashable이 됩니다. Bool?, [String] 등이 있을 수 있겠죠?  
* 또한, associated 값이 없는 enum(열거형)을 정의한다면, enum은 자동으로 Hashable을 준수하게됩니다. 
* 커스텀 타입의 경우에도 Hashable한 타입이 될 수 있는데, hash(into: )메서드를 구현한다면 Hashable 준수가 가능해집니다.
* (저장 property들이 모두 Hashable한 구조체, 모두 Hashable한 enum 타입을 가진 enum 이라면 컴파일러가 hash(into:) 메서드를 자동으로 제공합니다.)

값을 해싱한다는 것은 필수 구성 요소(essential component)를 해시 함수(Hasher 타입으로 표시되는)에 제공하는 것을 의미합니다.  
필수 구성 요소는 타입의 Equatable 구현에 기여하는 구성 요소입니다. 동일한 두 인스턴스는 같은 순서로 hash(into:)에서 같은 값을 Hasher에 공급해야 합니다.

여기서 Equatable에 대한 언급이 나오네요. Hashable은 Equatable 프로토콜을 상속 받고 있습니다. 


#### Equatable을 왜 상속해야 할까?

```swift
protocol Equatable {
    static func == (lhs: Self, rhs: Self) -> Bool
}
```

해싱을 하게 되면 해시값이라는 **일정한 자릿수의 고유값**을 만들어냅니다. 이 값이 고유값이라는 것을 보장하기 위해서 Hashable은 `값이 동일한 지 비교 할 수 있는 타입`인 Equatable을 준수합니다.  Equatable을 준수하는 타입은 == 혹은 != 사용하여 동등성을 비교할 수 있습니다.

Set, Dictionary의 key값 또한 중복을 허용하면 안되는 특성이 있습니다. 그래서 그 타입 안에서 유일성이 보장되는 Hashable을 준수하는 타입만 받도록 되어있는 것입니다. Set, Dictionary의 key값의 유일성 보장 노하우(?)는 Hasing기법이라고 할 수 있겠네요.

