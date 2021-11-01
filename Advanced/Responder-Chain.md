# Responder Chain 구조에 대해 설명하고, First Responder 역할에 대해 설명하시오.

## 참고한 좋은 글
* [공식문서-Using Responders and the Responder Chain to Handle Events](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/using_responders_and_the_responder_chain_to_handle_events)
* [공식문서-Responder Object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Responder.html)
더 알아보기(UIResponder)
* [iOS의 Responder와 Responder Chain 이해하기](https://seizze.github.io/2019/11/26/iOS의-Responder와-Responder-Chain-이해하기.html)

## 함께보면 좋은 답변
* [모든 View Controller 객체의 상위 클래스는 무엇이고 그 역할은 무엇인가?](../iOS/UIViewController.md)

## 답변
## UIResponder
```swift
@available(iOS 2.0, *)
open class UIResponder : NSObject, UIResponderStandardEditActions {

    open var next: UIResponder? { get }
    
    open var canBecomeFirstResponder: Bool { get } // default is NO
    open func becomeFirstResponder() -> Bool

    open var canResignFirstResponder: Bool { get } // default is YES
    open func resignFirstResponder() -> Bool

    open var isFirstResponder: Bool { get }
    // ...
}
```
`Responder 객체`는 이벤트를 핸들링하고 이벤트에 반응할 수 있는 `UIReponder의 인스턴스`입니다. `UIApplication`, `UIViewController 객체들`, `모든 UIView 객체들(UIWindow 객체 포함)`을 포함한 많은 주요 객체들 또한 reponder입니다. 이벤트가 일어나면, UIKit은 이벤트 핸들링을 위해 해당 이벤트를 앱의 reponder 객체들에게 보내게 됩니다.

<p align="center"><img width=30% src="https://user-images.githubusercontent.com/42789819/139632670-84d25ccb-6f40-4874-8fd2-54768e8ced7e.png"></p>

이벤트의 종류엔 **touch events**, **motion events**, **remote-control(원격조종) events**, 그리고 **press events** 등이 있습니다. 이러한 특정 이벤트들을 핸들링하기 위해서는 responder가 해당 이벤트에 대응되는 메서드들을 오버라이드하여 구현해야 합니다. 예를 들어, 터치 이벤트를 핸들링하기 위해서는 responder가 **touchesBegan(_:with:)**, **touchesMoved(_:with:)**, **touchesEnded(_:with:)**, **touchesCancelled(_:with:)** 메서드를 구현해야 합니다. 
> 터치의 경우에, responder는 터치의 변화를 트래킹하고 앱의 인터페이스를 적절히 업데이트하기 위해서 UIKit에서 제공하는 이벤트 정보를 이용합니다.

`Responder`들은 UIEvent를 처리할 수도 있지만 input view를 통해 커스텀된 input을 받아들일 수도 있습니다. system keyboard가 후자의 경우를 가장 잘 나타내는 예시입니다. 만약 유저가 UITextField나 UITextView를 탭하면 view는 first responder가 되고 input view를 띄우게 됩니다. 여기서 input view는 system keyboard이죠.   
이와 비슷하게 우리도 커스텀 input view를 만들고 다른 responder가 활성화 될 때 커스텀된 input view를 띄울 수 있습니다. 커스텀 input view를 responder에 연결하려면, 해당 뷰를 responder의 inputView 프로퍼티에 할당하세요.


</br>

## Responder Chain

UIKit `Responder`들은 이벤트 처리 작업 외에, 처리 되지 않은 이벤트를 다른 객체로 **forwarding**하는 역할도 수행합니다.  
`Responder Chain`은 responder 객체들이 이벤트나 액션 메시지를 처리할 책임을 앱 내의 다른 객체들에게 전송할 수 있도록 해줍니다. 이를 통해 정해진 responder가 이벤트를 처리하지 않을 경우, 해당 responder는 그 이벤트를 responder chain에 존재하는 다음 객체에게 **forwarding**하게 됩니다. 이와 같은 과정이 계속 반복되며 마지막까지 처리되지 않을 경우, 앱이 해당 메시지를 버립니다.

### **Responder Chain내에서 event가 전달되는 경로**

일반적인 이벤트는 responder chain 내의 first responder 혹은 터치된 뷰에서 시작하여 뷰 계층을 따라 window 객체를 거쳐 app 객체에 도달할 때까지 이동합니다. UIKit은 일정한 규칙을 사용하여 responder chain을 동적으로 관리하는데, 이 규칙에 의해 어떤 객체가 다음 순서로 이벤트를 받을지가 결정됩니다. 예를 들어, 뷰는 자신의 슈퍼뷰에게도 이벤트를 forwarding하고, 뷰 계층의 루트 뷰는 자신의 뷰 컨트롤러에게로 이벤트를 포워딩합니다.

<img width=38% src="https://user-images.githubusercontent.com/42789819/139702055-7f34b0e6-73a6-4ace-8ef5-c7b27e06dd94.png"> <img width=61% src="https://user-images.githubusercontent.com/42789819/139696784-7d7b189a-6f4a-4698-be30-bcdcd2288926.png">

> Responder chains in an app
1. Text field가 이벤트를 핸들링하지 않으면, UIKit은 text field의 부모인 UIView 객체에게로 이벤트를 보내고, 이어서 윈도우의 루트 뷰에게로 보냅니다.
2. 루트 뷰에서, Responder chain은 이벤트를 윈도우로 보내기 전에 방향을 바꾸어 루트 뷰를 소유하고 있는 뷰 컨트롤러에게로 보냅니다.
3. 만약 윈도우가 이벤트를 처리하지 못하면, UIKit은 이벤트를 UIApplication 객체에게로 보냅니다. 그리고 app delegate가 UIResponder의 인스턴스이고 responder chain의 일부가 아니라면 이벤트를 app delegate에게로 보냅니다.


</br>

## First Responder

위에서 알아봤듯 Responder는 raw event data를 수신하고 이벤트를 처리하거나 다른 resopnder 객체로 전달하는 역할도 해야합니다.
그렇기 때문에 앱이 이벤트를 수신하면 UIKit은 자동적으로 가장 적절한 responder 객체에게 연결해줍니다. 이 때, 가장 적절한 responder객체를 `first responder`라고 칭합니다.

이벤트를 받기 위해서 responder는 자신이 `first responder`가 될 수 있음을 나타내야 합니다. 그러기 위해서 우리는 UIResponder의 서브클래스에서 `canBecomeFirstResponder`프로퍼티를 오버라이드하여 true를 리턴하도록 만들어야 합니다.


### **Determining an Event’s First Responder**
UIKit은 받은 이벤트의 종류에 따라서 특정 객체를 해당 이벤트의 first responder로 지정합니다.

|Event type| First Responder|
|:---|:---|
|Touch events|터치가 일어난 뷰|
|Press events|포커스를 가진 뷰|
|Shake-motion events|UIKit이 지정한 객체 또는 직접 지정|
|Remote-control events|UIKit이 지정한 객체 또는 직접 지정|
|Editing menu messages|UIKit이 지정한 객체 또는 직접 지정|

Control은 그들과 관련된 타겟 객체와 액션 메시지를 사용하여 직접 소통합니다. 사용자가 control과의 상호작용을 할 때, control은 액션 메시지를 타겟 객체로 전송합니다. **액션 메시지는 event는 아니지만 여전히 responder chain을 사용합니다.** 만약 control의 타겟 객체가 nil일 경우, UIKit은 first responder에서 시작하여 적절한 액션 메시지를 구현한 객체를 만날 때까지 responder chain을 따라 이동합니다.


### **Determining Which Responder Contained a Touch Event**

UIKit은 어디서 터치 이벤트가 발생했는지를 결정하기 위해 뷰 기반 hit-testing을 사용합니다. UIKit은 터치 위치를 뷰 계층에 있는 뷰 객체의 바운드와 비교합니다. UIView의 hitTest(_:with:) 메서드는 특정 터치를 포함하는 가장 깊은 서브뷰를 찾기 위해 뷰 계층을 따라서 이동하고, 이 가장 깊은 서브뷰가 터치 이벤트의 first responder가 됩니다.

터치 위치가 뷰의 경계 밖이라면, hitTest(_:with:) 메서드는 해당 뷰와 그 뷰의 모든 서브뷰들을 무시합니다. 결과적으로, 뷰의 clipToBounds 프로퍼티가 false라면, 그 뷰의 밖에 있는 서브뷰들은 터치를 포함하더라도 반환되지 않습니다.

터치가 일어나면, UIKit은 UITouch 객체를 만들고 뷰와 연결합니다. 터치 위치나 다른 파라미터들이 변경되면, UIKit은 같은 UITouch 객체를 새로운 정보로 업데이트합니다. 변경되지 않는 유일한 프로퍼티는 뷰입니다. 심지어 터치 위치가 원래 뷰의 바깥으로 이동했더라도, 터치의 view 프로퍼티의 값은 변하지 않습니다. 터치가 끝나면, UIKit은 UITouch 객체를 메모리에서 해제합니다.