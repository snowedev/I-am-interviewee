# Generic에 대해 설명하시오.


## 참고한 좋은 글
* [Swit)Generic](https://zeddios.tistory.com/226)


## Answer

**Generic : 포괄적인**

우리가 사용하는 Array, Dictionary는 Generic으로 이루어진 컬렉션이다. 
우리가 배열을 만들 때 Int와 String 배열을 만들기 위해 선언하는 방식이 똑같다는 것을 떠올리면 된다.

만약 두 정수를 합쳐주는 다음과 같은 코드가 있다고 할 때,

```swift
func whatIfSwap(_ a: Int, _ b: Int) {
    print("a: \(b), b: \(a)")
}

whatIfSwap(3,5) // a: 5, b: 3
whatIfSwap(3.5,5.5) // error
whatIfSwap("hi","wonseok") // error
```

데이터타입이 int라면 정상적으로 출력 되지만, double이나 string일 경우에는 똑같은 동작을 하는 함수를 여러개 만들어 주어야 하는 상황이 발생한다.


이때 유용하게 사용할 수 있는 것이 Generic이다.

```swift
func whatIfSwap<T>(_ a: T, _ b: T) {
    whatIfSwap("a: \(b), b: \(a)")
}

whatIfSwap(3,5) // a: 5, b: 3
whatIfSwap(3.5,5.5) // a: 5.5, b: 3.5
whatIfSwap("hi","wonseok") // a: wonseok, b: hi
```

이렇게 해주면 이 함수 하나로 Int, String, Double 형 함수를 각각 만들지 않아도 사용할 수 있다.
> 유연함, 재사용성


T는 타입 파라미터라고 불리며, T는 그저 placeholder이기 때문에 아래와 같이 써도 무방하다.
```swift
func swap<Wonseok>(_ a: Wonseok, _ b: Wonseok) {
    print("a: \(b), b: \(a)")
}
```

하지만 Swift는 안전하며 타입에 굉장히 민감한 언어이기 때문에 Generic을 통해 자유도를 부여하였어도 위 함수가 호출될 때 정해진 a와 b의 데이터 타입이 서로 다른 것은 허용하지 않는다. 