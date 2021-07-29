# Escaping Closure(탈출 클로저)에 대해 설명하시오

## 참고한 좋은 글
* [Escaping 클로저(@escaping)](https://jusung.github.io/Escaping-Closure/)
* [Escaping Closure(탈출 클로저)](https://dongminyoon.tistory.com/14)
* [[Swift] @escaping closure, 탈출 클로저 쉽게 이해하기](https://baechukim.tistory.com/38)


## Answer

Escaping Closure(`@escaping`) : 클로저가 함수의 인자로 전달 됐을 때, 함수의 실행이 종료된 후 실행되는 클로저.

### Non-Escaping Closure
```swift
func runClosure(closure: () -> Void) {
    closure()
}
```
1. 클로저가 `runClosure()`함수의 `closure` 인자로 전달됨
2. 함수 안에서 `closure()`가 실행됨
3. `runClosure()` 함수가 값을 반환하고 종료됨


<br>

### Escaping Closure(@escaping)
```swift
class ViewModel {
    var completionhandler: (() -> Void)? = nil

    func fetchData(completion: @escaping () -> Void) {
        completionhandler = completion
    }
}
```
1. 클로저가 `fetchData()` 함수의 `completion` 인자로 전달됨
2. 클로저 `completion`이 `completionhandler` 변수에 저장됨
3. `fetchData()` 함수가 값을 반환하고 종료됨
4. 클로저 `completion`은 아직 실행되지 않음


<br>

### 보다 명시적인 예시

```swift
class Myclass {
    var x = 10
    
    func callFunc() {
        withEscaping { self.x = 100 }
        withoutEscaping { x = 200 } 
    }

    var completionHandlers: [() -> Void] = []

    func withEscaping(completion: @escaping () -> Void) {
        completionHandler.append(completion)
    }

    func withoutEscaping(completion: () -> Void) {
        completion()
    }
}
```
& 
```swift
let mc = MyClass()

mc.callFunc() // 1.
print(mc.x) 	// 2. result:200

mc.completionHandlers.first?()
print(mc.x)	// 3. result:100
```
1. 클래스 인스턴스를 생성하여 callFunc() 호출
2. @escaping로 명시해주지 않았던 withoutEscaping 메소드가 클로저를 바로 호출하므로 200 출력
     - 이때 @escaping으로 명시해둔 메서드에서는 클로저를 completionHandler에 append하여 저장
3. completionHandler 배열에 저장해둔 가장 앞에 있는 클로저를 호출하여 그제서야 100 출력

**`이처럼 탈출 클로저를 사용하면 클로저를 함수 구문 밖에서도 호출 할 수 있다는 장점이 있습니다.`**


<br>
<br>

📌 탈출 클로저를 구현하기 위해서는 꼭 @escaping으로 명시해주어야 한다. 하지만 명시해두고 Non-Escaping 클로저를 사용해도 상관 없다.

### 그러면 그냥 @escaping 선언을 항상 default로 해두면 둘 다 사용할 수 있으니 더 좋지 않을까?

굳이 Non-Escaping, Escaping으로 나누는 이유

바로 컴파일러의 퍼포먼스와 최적화 때문이다.

Non-Escaping 클로저의 경우, 컴파일러가 클로저의 실행 종료 시점이 언제인지 알기 때문에 때에 따라 특정 객체에 대한 retain, release의 처리를 생략하여 life-cycle을 효율적으로 관리할 수 있다.

하지만 Escaping 클로저는 함수의 밖에서 실행 되기 때문에 이를 보장하기 위해 클로저에서 사용하는 객체에 대한 추가적인 참조싸이클(reference cycles) 관리를 해주어야 한다. 

이러한 이유로 Swift에서는 필요시에만 Escaping클로저를 사용하도록 구분되어 있다!