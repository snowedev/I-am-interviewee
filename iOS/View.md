# View 객체에 대해 설명하시오.

## 도움을 준 글
* [UIView - Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uiview)

## Answer

### View
사용자 인터페이스의 기본 구성 요소


### UIView
```swift
class UIView: UIResponder
```

UIView의 [공식문서](https://developer.apple.com/documentation/uikit/uiview)를 직역해보자

UIView는 화면의 직사각형 영역에 대한 콘텐츠를 관리하는 객체이다.

View는 앱의 UI에 있어서 가장 기본적으로 쌓아지는 블록이며 UIView 클래스는 모든 뷰의 공통적인 동작을 정의한다. 

View 객체는 자신의 영역 내에서 콘텐츠를 렌더링하고 그 콘텐츠와의 모든 상호작용을 처리한다. 또한 더 정교한 콘텐츠를 그리기 위해서 우리는 UIView를 상속받을 수 도 있다.

View 객체는 앱이 사용자와 상호작용하는 주된 방법이기 때문에 몇가지 의무를 지닌다.

1. 그리기, 애니메이션
    * view들은 그들의 영역 안에서 UIKit과 Core Graphics를 사용하여 컨텐츠를 그린다
    * 일부 view 프로퍼티들을 새로운 값으로 애니메이션할 수 있다.

2. 레이아웃과 자식 뷰(subview) 관리
    * view들은 자식 뷰를 갖지 않을 수 도 있고, 여러 개 가질 수도 있다.
    * view들은 자신의 자식뷰들의 사이즈와 위치를 조정할 수 있다.
    * Auto Layout을 사용하여 뷰 계층 구조의 변화에 따른 응답으로 크기 와 위치를 재조정하는 규칙을 정의할 수 있다. 

3. 이벤트 처리
    * view는 UIResponder를 상속하고 있다. 그래서 터치나 다른 타입의 이벤트에 대해 반응 할 수 있다.
        > UILabel, UITextView 등 텍스트를 보여주는 view는 기본적으로 터치 이벤트가 비활성화되어 있음
    * view들은 일반적인 제스쳐를 다루기 위해 gesture recognizer를 적용할 수 있다.


### UIView와 연결된 개념 [Frame, Bounds](./frame,bounds.md)

