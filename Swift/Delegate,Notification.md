# Delegate와 Notification 방식의 차이점

## 참고하면 좋은 글
* [Delegation, Notification, 그리고 KVO](https://medium.com/@Alpaca_iOSStudy/delegation-notification-그리고-kvo-82de909bd29)
* [Delegate, Notification, KVO 비교 및 장단점 정리](https://you9010.tistory.com/275)

## 함께 보면 좋은 답변
* [Delegate Patter에 대해 설명하시오](./DelegatePattern.md)
* [Notification 동작방식 및 활용방안에 대해 설명하시오](../iOS/NotificationCenter.md)
## Answer

Delegate, Notification그리고 KVO(Key Value Observing)까지 세가지 패턴 모두 특정 이벤트가 일어나면 원하는 객체에 알려주어 해당되는 처리를 하는 방법을 가지고 있다.

### 차이점

1. `의존관계  `
* Delegate는 위임되는 객체와 위임하는 객체간의 1:1 관계를 띈다. 
* Notification은 알림을 보내는 객체와 등록된 객체들 간의 1:n 관계를 가진다.


2. `작동`
* Delegate는 특정 protocol을 구현함으로써 위임하는 객체에 대해 알 필요 없이 프로토콜을 통해 대신 메소드를 사용할 수 있다.
* Notification은 하나의 공유되는 default NotificationCenter 객체에 observer를 등록함으로써 정보를 브로드캐스트하고, observer들은 수동적으로 정보를 받는 개념이다. 하나의 객체가 여러 specific한 notification들에 대해 개별적으로 등록은 가능하지만, 객체가 원할 때 특정 메소드를 사용한다거나 정보를 받아올 수는 없다.


3. `사용`
* iOS에서 Delegate는 주로 MVC 패턴을 만족하기 위해 View와 ViewController 사이에서 사용되거나, 프레임워크를 만들 때 사용자들이 디테일한 내용을 알지 못하더라도 프로토콜을 통해 메소드를 사용할 수 있도록 하기 위해서 사용된다.
* 반면 Notification은 특정 이벤트가 여러 객체에 전달되어야 할 때 주로 사용된다. register-> post-> receive의 간단한 메커니즘을 가지고 있지만 observer들이 많아지면 상호작용하는 객체들간의 관계를 파악하기 어려워진다는 단점이 있다.