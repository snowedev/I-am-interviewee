# 모든 View Controller 객체의 상위 클래스는 무엇이고 그 역할은 무엇인가?

## 참고한 좋은 글
* [공식문서-UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)
* [공식문서-UIResponder](https://developer.apple.com/documentation/uikit/uiresponder)
* [iOS의 Responder와 Responder Chain 이해하기](https://seizze.github.io/2019/11/26/iOS의-Responder와-Responder-Chain-이해하기.html)

## 함께보면 좋은 답변
* [Responder Chain 구조에 대해 설명하고, First Responder 역할에 대해 설명하시오.](./Advanced/Responder-Chain.md)

## 답변

UICollectionViewController, UITableViewController 등 모든 ViewController의 상위클래스는 `UIViewController`입니다.

그리고 그 `UIViewController`의 핵심적인 상위 클래스는 `UIResponder`입니다. 

***이하 직역***

## Declaration
```swift
@MainActor class UIViewController : UIResponder
```

## Overview
UIViewController 클래스는 모든 view controller들의 공통적으로 분배되는 행위를 정의합니다.

`UIViewController` 클래스는 아래의 행위들에 대한 주요 책임을 가지고 있습니다.

* view 내부 내용들을 update한다. 보통은 데이터의 변경 따라서 내용을 update한다.
* view를 통해 사용자와의 상호작용에 대해 반응한다.
* interface 전반적인 레이아웃을 관리하고 view를 리사이징한다.
* 앱 내의 다른 객체들(다른 view controller 등)과 상호작용을 한다.

view controller는 뷰들과 단단히 묶여있어서 뷰의 계층 구조 안에서 event의 처리를 참여하고 관리합니다. 특히, view controller는 `UIResponder 객체`로서 view controller의 root view와 해당 뷰의 super view(일반적으로 다른 view controller에 속하는) 사이의 [`Responder Chain`](./Advanced/Responder-Chain.md)에 삽입됩니다. 만약 view controller의 뷰들 중 그 누구도 event를 처리하지 않는다면, view controller는 이벤트를 직접 처리하거나 super view에 전달할 수 있습니다.

---
### **더 알아보기 `UIResponder`**
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

### +
[Responder Chain 구조에 대해 설명하고, First Responder 역할에 대해 설명하시오.](./Advanced/Responder-Chain.md)