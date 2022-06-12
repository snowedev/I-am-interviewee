# setNeedsLayout와 setNeedsDisplay의 차이에 대해 설명하시오.

## 참고한 글
* [Apple Developer - UIView](https://developer.apple.com/documentation/uikit/uiview)
* [View Programming Guide for iOS](https://developer.apple.com/library/archive/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/WindowsandViews/WindowsandViews.html#//apple_ref/doc/uid/TP40009503-CH2-SW9)
* [Drawing and Printing Guide for iOS](https://developer.apple.com/library/archive/documentation/2DDrawing/Conceptual/DrawingPrintingiOS/GraphicsDrawingOverview/GraphicsDrawingOverview.html#//apple_ref/doc/uid/TP40010156-CH14-SW1)
* [ZeddiOS - View/레이아웃 업데이트 관련 메소드](https://zeddios.tistory.com/359)

## Answer

### setNeedsLayout와 setNeedsDisplay의 역할

UIView의 공식문서를 살펴보면 다음과 같은 내용이 나온다.  

View의 실제 컨텐츠가 변경될 때, **view가 다시 그려질 필요가 있음을 시스템에 알리는 것은 개발자의 책임입니다.**  
개발자는 **view의 setNeedsLayout나 setNeedsDisplay를 호출함으로써 이를 수행할 수 있습니다.** 이 메서드들은 시스템이 다음 drawing cycle 동안  view를 업데이트 해야함을 알 수 있게 해줍니다.  view를 업데이트 하기 위해 다음 drawing cycle까지 기다리기 때문에, 여러 view에서 이 메서드를 호출하여 동시에 업데이트 할 수 있습니다.

내용에서 알 수 있듯이 `setNeedsLayout`, `setNeedsDisplay` 두 메서드 모두 UIView의 하위 인스턴스 메서드이다. 그리고 이 메서드들은 view의 업데이트가 일어나고 이를 시스템에 알리기 위해 호출된다. 그러면 시스템은 drawing cycle이 돌아왔을 때 이를 인지하고 view를 업데이트해주게된다.

#### [setNeedsDisplay()](https://developer.apple.com/documentation/uikit/uiview/1622587-setneedsdisplay/)
* View의 내용을 다시 그려야 함을 시스템에 알린다. 그리고 즉시 리턴된다. 요청은 `다음 drawing cycle`에 반영된다.  
> 다음 drawing cycle에 받은 요청들이 한번에 반영되기 때문에 요청만 하고 즉시 리턴되는 것  
* 이 메서드는 UIKit, Core Graphics를 사용하는 View에서만 사용해야 효과가 있다.  
* 이 메서드는 View의 내용이나 모양이 변경된 경우에만 View를 다시 그린다 위치의 변화는 적용되지 않는다.  
* 이 메서드를 호출하면, `draw(_ rect:)` 메서드가 내부적으로 호출된다.
* 메서드가 요청한 작업(View의 내용을 다시 그리는)은 요청 시점보다 나중에 일어나게 됨으로 이 메서드는 `비동기 액티비티`이다.

#### [setNeedsLayout()](https://developer.apple.com/documentation/uikit/uiview/1622601-setneedslayout/)
* receiver(수신자)의 현재 레이아웃을 무효화하고, `다음 업데이트 주기 동안` 레이아웃 업데이트를 트리거한다.    
* View의 하위 View에 대한 레이아웃을 조정하려면, App의 main 쓰레드에서 이 메서드를 호출한다.  
* 이 메서드를 호출하면, `[layoutSubViews()](https://developer.apple.com/documentation/realitykit/arview/layoutsubviews()/)`메서드가 내부적으로 호출된다. 
* 메서드가 요청한 작업(View의 레이아웃을 다시 그리는)은 요청 시점보다 나중에 일어나게 됨으로 이 메서드는 `비동기 액티비티`이다.

#### 번외 - [layoutIfNeeded()](https://developer.apple.com/documentation/uikit/uiview/1622507-layoutifneeded/)
* 대부분의 내용은 setNeedsLayout()과 같고 아래 두가지에 핵심적으로 차이가 있다.
* receiver(수신자)의 현재 레이아웃을 무효화하고, `호출 즉시` 레이아웃 업데이트를 트리거한다.
* 메서드가 요청한 작업(View의 레이아웃을 다시 그리는)이 바로 시행됨으로 `동기적`으로 작동한다.

#### 번외 - setNeedsLayout()와 layoutIfNeeded()의 차이점 [출처](https://github.com/lmacfadyen/UIViewLifecycleLayoutDisplay)  
<p align="center"><img src="https://user-images.githubusercontent.com/42789819/173237950-054b38c4-addc-433e-acaf-8f1bf55d092c.gif"></p>



### drawing cycle

UIView 클래스는 컨텐츠를 표시할 때, `on-demand drawing model(주문형 드로잉 모델)`을 사용한다. 말 그대로 주문에 의해 동작하는 거라고 생각하면 될 것 같다.  
view가 처음 화면에 나타나면, 시스템은 컨텐츠를 그려달라고 주문을 넣는다. 그러면 시스템은 컨텐츠의 스냅샷을 캡쳐하고, 해당 스냅샷을 view의 시각적인 표현으로 사용한다.  
view의 내용이 변경되지 않으면 view의 드로잉 코드를 다시 호출 할 수 없다. **반대로 컨텐츠가 변경되었다면 view가 변경되었음을 시스템에 알리고,** view가 변경된 내용에 따라 drawing을 수행하면 그 결과의 스냅샷을 캡쳐하는 프로세스를 반복하게 된다.

view가 처음 나타나거나 일부를 다시 그려야하는 경우, iOS는 view의 `draw(_ rect:)` 메서드를 호출해서 view에게 컨텐츠를 그려달라고 요청한다.  
`draw(_ rect:)` 메서드를 호출시킬 수 있는 트리거 이벤트는 아래와 같다. 

* View를 부분적으로 가리고 있던 다른 view의 이동 또는 제거
* hidden 프로퍼티를 false로 설정하여, 이전에 숨겨진 view를 다시 볼 수 있게 만들기
* view를 화면 밖으로 스크롤 한 다음, 화면으로 다시 이동하기
* view의 `setNeedsDisplay` 또는 `setNeedsDisplayInRect(_:)` 메서드를 명시적으로 호출하기  
> 지금까지 setNeedsDisplay나 setNeedsDisplayInRect(_:)를 호출하지 않았는데 뷰가 제대로 업데이트 되었다면 그 외 3가지 케이스에 해당되어 내부적으로 호출이 되었을 것이다.  

**view의 업데이트는 바로 이루어지는 것이 아니라 drawing cycle이 돌아왔을 때 한꺼번에 된다.** 따라서 위 설명처럼, 현재 view의 스냅샷을 캡쳐해놓았다가 컨텐츠의 변경이 일어나서 시스템에게 업데이트 요청을 한다면 들어온 요청들은 즉각 반영이 되는것이 아니다. 다음 drawing cycle이 돌아왔을 때 일괄 처리가 이루어진다. 그리고 이 때, view의 업데이트를 `draw(_ rect:)` 메서드를 호출하여 한다. 이 메서드를 호출시킬 수 있는 트리거 이벤트는 위 4가지 경우이다.

추가적으로, `draw(_ rect:)` 메서드는 직접 호출해서는 안된다. 이 메서드는 view 객체들이 메모리에 올라간 뒤(viewDidLoad 이후) 최초로 호출되는데, 만약 그 이후에 필요에 의해 이 메서드를 호출하기 위해서는 `setNeedsDisplay()`, `setNeedsDisplayInRect(_:)` 메서드를 호출해야한다.




