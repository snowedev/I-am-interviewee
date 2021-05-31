# Delegate Pattern에 대해 설명하시오. / Delegate는 retain 될까?

## 사전 지식
* [Protocol-basic[이동건의 이유있는 코드]](https://baked-corn.tistory.com/24)
* [Protocol-advanced[이동건의 이유있는 코드]](https://baked-corn.tistory.com/26)
* [ARC](../ARC/ARC.md)
* [Reference Count](../ARC/Reference-count-in-ARC.md)

## 참고하면 좋은 글
* [iOS Delegate 패턴에 대해서 알아보기](https://magi82.github.io/ios-delegate/)
* [Delegation Pattern](https://baked-corn.tistory.com/23)
* [Delegate Retain Cycle in Swift](https://medium.com/macoclock/delegate-retain-cycle-in-swift-4d9c813d0544)

## Answer

### 📌 정의

`Delegate`: 대리자, 위임자  


Delegate는 어떤 객체가 해야 하는 일을 부분적으로 확장해서 대신 처리를 한다.


Delegate = ["일을 하는 객체", "일을 시키는 객체"]


```xml
고객이 여러곳에서 사용할수 있는 메시지창을 하나 만들어 달라고 합니다.
메시지창에는 버튼이 있고 누르면 뷰컨트롤러는 무슨 버튼이 눌렸는지 체크 해서
이벤트 발생시 버튼이 해야된다고 짜놓은 것들을 처리 해야 한다고 합니다.
```
여기서!  
* `뷰컨트롤러`: 일을 하는 객체
* `메시지 창`: 일을 시키는 객체


또한, 메시지 창이 뷰컨에 일을 시키기 위해서는 시키는 대상을 가지고 있어야하기 때문에 
```swift
msg.delegate = self
```
와 같은 구문이 존재하는 것!


우리가 자주 사용하는 TableView의 UITableViewDelegate, UITableDataSource의 경우에
테이블 뷰에 들어간 뷰컨트롤러가 "일을 하는 객체"이다. 따라서
```swift
tableview.delegate = self
```
이렇게 되는 것.


### 📌 사용하는 이유

범용성과 재사용성을 위해서.

상속을 했을 때 어쩔 수 없는 클래스 갯수의 증가, 객체를 전달할 때 처리할 필요가 없는 부분까지 넘어가는 것을 해결할 수 있다.

프로토콜은 하나의 약속으로써 정해진 메소드만 구현하겠다! 라고 선언한다. 그래서 해당 프로퍼티에서는 약속된 메서드만 실행 가능하게 된다.


### 📌 Delegate는 Retain될까?

<img width="572" alt="image" src="https://user-images.githubusercontent.com/42789819/120108483-43b81e00-c1a0-11eb-8057-9a50fd2a0763.png">


Delegate는 기본적으로 retain하지 않는다. 만약 A라는 ViewController가 테이블 뷰의 delegate 대상으로 설정될 때, delegate가 강한 참조를 하고 있다면, 추후 A에 nil을 할당하더라도 테이블 뷰는 A를 계속 붙잡고 있게 되므로 해당 VC는 메모리에서 해제되지 않는 현상이 발생할 수 있다(`강한 순환 참조`).

```swift
// 강한 순환 참조. 
protocol SomeDelegate {
    func something()
}
class SomeClass {
    var delegate: SomeDelegate?
}

var some1: SomeClass? = SomeClass()
var some2: SomeClass? = SomeClass()

some1.delegate = some2
some2.delegate = some1

some1 = nil
some2 = nil
// SomeClass의 delegate가 strong이기 때문에 상위 객체에 nil을 할당해도 메모리에서 해제되지 않음
```

```swift
class SomeClass {
    // 약한 참조를 통해 해결
    // 이렇기 때문에 delegate는 retain 되지 않는다.
    weak var delegate: SomeDelegate?
}

var some1: SomeClass? = SomeClass()
var some2: SomeClass? = SomeClass()

some1.delegate = some2
some2.delegate = some1

some1 = nil
some2 = nil
```

Definition을 보아도 Delegate는 weak(약한참조)를 하고 있는 것을 알 수 있다. 
<img width="334" alt="image" src="https://user-images.githubusercontent.com/42789819/120108530-77934380-c1a0-11eb-9093-0799f49e2272.png">
