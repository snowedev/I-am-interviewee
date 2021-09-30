# UIWindow 객체의 역할은 무엇인가

## 도움을 준 좋은 블로그
* [iOS ) UIWindow. 그리고 UIView](https://zeddios.tistory.com/283)

## 사전지식


## Answer

<img width=49% src="https://user-images.githubusercontent.com/42789819/135480642-4957858d-afd3-43f8-8c3c-633880c60b39.jpeg">

<img width=49% src="https://user-images.githubusercontent.com/42789819/135480650-b7da73bd-0f69-4edc-b725-8517fef473e3.jpeg">

> UIWindow를 설명하는 그림

위 그림을 보면 `UIWindow`가 `UIView`의 부모라는 느낌을 매우 많이 주고 있다. 하지만 실제로는 그렇지 않다!

<p align="center"> <img width=50% alt="image" src="https://user-images.githubusercontent.com/42789819/135481320-40a3b312-96ab-4a42-8db0-a488911fe9a7.png"></p>

> UIWindow의 Definition을 살펴보면 오히려 UIWindow의 뷰모가 UIView인 것을 알 수 있다. 


### 공식 문서 읽어보기

공식 문서를 살펴보면 UIWindow는 아래와 같이 정의되어있다.

"The backdrop for your app’s user interface and the object that dispatches events to your views."
> APP의 UI에 대한 배경이며 View에 이벤트를 전달하는 개체입니다.


"Windows work with your view controllers to handle events and to perform many other tasks that are fundamental to your app’s operation. UIKit handles most window-related interactions, working with other objects as needed to implement many app behaviors."
> Window들은 이벤트를 처리하고 많은 앱 실행의 기본이 되는 작업들을 수행하기 위해 ViewController와 함께 작동한다. UIKit은 대부분의 Window 관련 상호작용을 처리하고, 앱 동작을 구현하는 데 필요한 다른 객체들과 작동한다.

앱은 필수적으로 콘텐츠를 표시할 기본 Window를 제공해야하는데, 이를 Xcode가 대신 제공해줍니다. 그래서 프로젝트를 생성하고나면 별도의 작업 없이도 `AppDelegate(iOS 13부터는 SceneDelegate)` 내부에 window 속성이 있는 것을 볼 수 있는건가봐요! 

제가 UIWindow를 건드렸던 건 
1. Window의 Root View Controller변경
2. Window가 표시되는 스크린 변경

이 두가지 였는데 이 두가지 말고도 아래와 같은 작업을 수행할 때 사용된다고 합니다.

1. 다른 window들과의 가시성에 영향을 주는 window의 z축 level 설정
2. window를 표시하고 키보드 이벤트의 대상으로 만들기
3. 좌표 값을 window의 좌표계로, 혹은 그 반대로 변환


"Windows do not have any visual appearance of their own. Instead, a window hosts one or more views, which are managed by the window's root view controller."
> Window는 자신만의 어떤 모양이 있는 것은 아니지만, window는 window의 루트 뷰컨에 의해 관리되는 하나 혹은 그 이상의 뷰들을 가질 수 있다.

그리고 UIWindow 객체를 하위 클래스로 두는 경우는 극히 드물다고 해요. 상위 레벨 ViewController에서 더 쉽게 구현할 수 있기 때문이래요. 하위 클래스로 지정하게 되는 몇 가지 경우 중 하나는 window의 키 상태가 변경될 때 사용자 지정 동작을 구현하기 위해 `becomeKey()` 또는 `registeredKey()` 메서드를 재정의할 때 정도라고 하네요!


## 요약
* **iOS앱은 모든 View들의 컨테이너 역할을 하는 UIWindow 인스턴스를 가진다.**
* **UIWindow는 UIView의 하위클래스이다.**
* 그러므로 UIWindow는 그 자체만으로 View이다.
* window은 모양이 있는건 아니지만 루트 뷰컨 이하의 많은 뷰들을 가질 수 있다.
* window를 하위 클래스로 두는 경우는 드물다.

