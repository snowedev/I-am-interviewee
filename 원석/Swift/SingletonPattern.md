# Singleton Pattern에 대해 설명하시오.

## 참고하면 좋은 글
* [싱글톤 패턴(Singleton Pattern)](https://babbab2.tistory.com/66)


## Answer

Singleton Pattern: 특정 용도로 객체를 하나만 생성하여, 공용으로 사용하고 싶을 때 사용하는 디자인 유형.

Singleton의 장점은 다음과 같다.
1. 메모리 낭비를 방지할 수 있다(싱글톤 생성을 남발하지 않는다면).
2. 하나의 객체로 데이터를 공유할 수 있다.


```swift
class UserInfo {
    var id: String?
    var password: String?
    var name: String?
}
```
이러한 UserInfo 클래스가 있을 때, 이 UserInfo의 id, password, name 변수를 각각 하나의 ViewController에서 입력받아 전달해야 한다고 생각해보자.

전달 방법에는 두가지가 있다.

1. 하나의 클래스 인스턴스를 만들고 전달해가며 입력받기(클래스는 참조 타입이기 때문)

2. UserInfo를 싱글톤으로 만들어 사용하기

둘 다 가능한 방법이지만 1번의 경우 계속해서 인스턴스를 넘겨주어야 하기 때문에 번거롭고 코드가 지저분해진다.   2번을 사용하게 되면 객체를 하나만 생성하고 그것을 전역에 두기 때문에 1번에서의 번거로운 과정을 거칠 필요가 없어진다.

<img width="50%" src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FVmsQc%2FbtqOYt0xgaU%2Fk4fR7SVzSexrukeToKNAKk%2Fimg.png">

요런식으로 인스턴스 하나로 해결 가능!

보통 네트워킹을 할 때 네트워킹 객체를 싱글톤으로 많이 사용한다.


### 주의사항
> 1. 싱글톤은 한번 생성되고 나면 프로그램 종료시까지 메모리에 올라가 있으므로 남발해서는 안된다.
> 
> 2. 싱글톤은 하나의 객체만을 생성하는 것이 목적이기 때문에 혹시나 init() 함수의 호출로 두개 이상의 객체 생성이 되는 것을 막기 위해 init() 함수의 접근 제어자를 private으로 선언해주어야 한다.