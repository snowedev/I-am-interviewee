# 앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체는 무엇인가?

## 참고한 좋은 글
* [iOS ) UIApplication](https://zeddios.tistory.com/539)
* [UIApplication](https://developer.apple.com/documentation/uikit/uiapplication/)
* [UIApplicationMain( _ : _ : _ : _ :)-공식문서](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain)

## 같이 보면 좋은 답변
* [상태 변화에 따라 다른 동작을 처리하기 위한 AppDelegate 메서드들을 설명하시오.](../iOS/AppDelegate.md)
* [SceneDelegate에 대해 설명하시오](../iOS/SceneDelegate.md)
* [Responder Chain 구조에 대해 설명하고, First Responder 역할에 대해 설명하시오.](../Advanced/Responder-Chain.md)

## 답변

`UIApplication` 싱글턴 객체가 생성된다.

```swift
import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
```

Obj-C에서는 main.c 에서 **UIApplicationMain(_: _: _: _:)** 를 호출했지만, Swift에서는 AppDelegate.swift에서 AppDelegate클래스 앞에 어노테이션으로 붙은 `@UIApplicationMain`를 통해 **UIApplicationMain(_: _: _: _:)** 를 호출한다.

***이하 직역***

### **Declaration**
```swift
@MainActor class UIApplication : UIResponder
---
@available(iOS 2.0, *)
open class UIApplication: UIResponder {
    open class var shared: UIApplication {get}
}
```
> [UIResponder?](../Advanced/Responder-Chain.md)

<br>

### **OverView**
The centralized point of control and coordination for apps running in iOS.
> iOS에서 동작중인 앱들의 집중 제어 및 조정 지점입니다.  
> 그만큼 중요한 역할을 한다는거겠죠?

<br>

모든 iOS 앱에서 UIApplication은 하나의 인스턴스(`Singleton(싱글톤)`)만 존재합니다. 앱이 동작할 때 시스템은 [UIApplicationMain(_: _: _: _:)](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain)
이라는 함수를 호출합니다. 이 함수가 바로 shared로 접근 가능한 UIApplication의 싱글톤 객체를 만드는거에요.
> 정리  
> 앱이 시작되면 시스템이 **UIApplicationMain(_: _: _: _:)** 를 호출해서 UIApplication의 싱글톤 객체를 만든다.

그렇다면 `UIApplicationMain(_: _: _: _:)`이 함수는 무슨 역할을 하길래 앱이 얘를 통해 싱글톤 객체를 만드는걸까요?  
`UIApplicationMain(_: _: _: _:)` 함수는 **application 객체와 앱의 delegate를 만들고 이벤트 싸이클을 설정**하는 역할을 합니다. 생긴건 아래처럼 생겼어요.
```swift
func UIApplicationMain(_ argc: Int32, 
                     _ argv: UnsafeMutablePointer<UnsafeMutablePointer<CChar>?>, 
                     _ principalClassName: String?, 
                     _ delegateClassName: String?) -> Int32
```
> - `argc` : argv에 있는 argument의 개수. 대게 main과 일치합니다.
> - `argv` : argument의 변수 목록. 대게 main과 일치합니다.  
> - `prinsipalClassName` : UIApplication클래스 또는 하위 클래스의 이름입니다. nil을 지정하면, UIApplication으로 가정됩니다.  
> - `delegateClassName` : application delegate가 인스턴스화 되는 클래스 이름입니다. prinsipalClassName이 UIApplication의 하위클래스를 지정하는 경우, 하위 클래스를 delegate로 지정 할 수 있습니다. 하위클래스 인스턴스는 앱의 delegate 메세지를 받습니다. 앱의 기본 nib파일에서 delegate 객체를 로드하는 경우, nil을 지정합니다.

다시 본론으로 돌아와서 더 직역해보자면, 그래서 이 함수로 만들어진 **앱의 application 객체**는 들어오는 사용자 이벤트의 초기 라우팅 처리를 담당합니다. 이들은 control 객체를 통해(UIControl class의 인스턴스)적절한 타겟 객체에 action 메시지들을 전송합니다. 또한, application 객체는 열린 window(UIWindow의 객체)의 목록을 관리하는데, 이를 통해 앱 내의 UIView 객체들을 검색할 수 있습니다.

UIApplication클래스는 UIApplicationDelegate프로토콜을 준수하고, 일부 프로토콜 메소드를 구현해야하는 delegate를 정의합니다. **application 객체는 delegate에게 중요한 런타임 이벤트(예: 앱 시작, 메모리 부족 경고 및 앱 종료)를 알리고**, 적절히 응답 할 기회를 제공합니다.

대부분의 앱에서는 UIApplication을 상속할 필요가 없습니다. 시스템과 앱 간의 상호작용을 처리하고 싶다면 AppDelegate를 통해 처리하면 됩니다.
> 만약, 상호작용이 무조건 시스템이 하기 전에 이루어져야해서 상속이 불가피하다면, 상속을 하고  **sendEvent(_:)** 그리고/혹은 **sendAction(_:to:from:for:)** 메서드를 override하여 처리하면됩니다. 추천되지는 않습니다.


</br>

### **이제 종합적으로 앱이 시작되는 그 시점을 좀 더 자세히 살펴보면 아래와 같습니다.**

<p align="center"><img width=65% src="https://user-images.githubusercontent.com/42789819/140506063-693223ba-de9e-4d1e-8a1c-5f7f7556f166.png"></p>

1. 앱이 시작되는 순간 AppDelegate의 @UIApplicationMain을 통해 `UIApplicationMain(_: _: _: _:)`를 호출해서 application 객체와 delegate를 만든다.
    > 이 때, `UIApplicationMain(_: _: _: _:)`의 파라미터 중 delegateClassName에는 이 함수를 호출한 "AppDelegate"가 할당된다.

2. 그럼 AppDelegate.Swift는 AppDelegate 클래스의 인스턴스를 만들고, 이를 위에서 만든 application 객체에 할당한다.

3. application 객체가 delegate 메서드인 `application:didFinishLaunchingWithOptions:` 를 호출한다.

4. UIApplicationDelegate를 채택중인 AppDelegate는  `application:didFinishLaunchingWithOptions:`가 호출된다.

5. 앱이 실행된다.
> 함께 보면 좋은 답변  
> [상태 변화에 따라 다른 동작을 처리하기 위한 AppDelegate 메서드들을 설명하시오.](../iOS/AppDelegate.md)  
> [SceneDelegate에 대해 설명하시오](../iOS/SceneDelegate.md)