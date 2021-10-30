# UINavigationController 의 역할이 무엇인지 설명하시오.

## 참고한 좋은 글
* [UINavigationController-공식문서](https://developer.apple.com/documentation/uikit/uinavigationcontroller)

## 답안
***이하 직역***
* [Overview](#overview)
* [Navigation Controller Views](#navigation-controller-views)
* [Updating the Navigation Bar](#updating-the-navigation-bar)

## Declaration
```swift
@MainActor class UINavigationController : UIViewController
```

## Overview
계층적 콘텐츠를 탐색하기 위한 스택 기반의 체계를 정의하는 컨테이너 뷰 컨트롤러이다.

네비게이션 컨트롤러는 하나 혹은 그 이상의 자식 뷰 컨트롤러들을 네비게이션 인터페이스 안에서 관리하는 하나의 `container view controller`이다. 이 인터페이스 안에서는 한 시점에 오직 하나의 자식 뷰 컨트롤러만이 보여질 수 있다.
> 네비게이션 인터페이스는 데이터 계층 조직을 모방하는데, 이 인터페이스는 당신의 앱에 의해 관리된다.

<p align="center"><img width="70%" alt="image" src="https://user-images.githubusercontent.com/42789819/139540080-267a8505-871c-4c2a-8fbb-3de9c1f22a52.png"></p>

> Figure 1 A sample navigation interface

뷰 컨트롤러 내부에 어떤 요소를 선택하여 새로운 뷰 컨트롤러를 띄울 수 있다. 이 때, 새로운 뷰 컨트롤러는 이전의 뷰 컨트롤러 위로 애니메이션과 함께 올라오게 된다. 마찬가지로 네비게이션 바에 위치한 뒤로가기 동작을 취할 때에는 현재 뷰 컨트롤러를 스택에서 지움으로써 밑에 가려져 있던 이전의 뷰 컨트롤러가 등장하게 된다.

네비게이션 컨트롤러 객체는 `Navigation Stack`  이라고 알려진 **정렬된 배열**을 사용하여 이들의 자식 뷰 컨트롤러들을 관리한다. 배열의 가장 처음에 있는 뷰 컨트롤러는 `root view controller`라고 부르며, 이는 스택 기준으로 가장 밑에 위치해있다.

**segue** 혹은 **UINavigationController 클래스**의 메서드를 사용해서 뷰 컨트롤러들을 추가하거나 지울 수 있다. 또한 사용자는 **뒤로가기 버튼** 혹은 **left-edge swipe gesture**를 통해 가장 최상위 뷰 컨트롤러를 지울 수 있다(이전 뷰로 돌아가기).

네비게이션 컨트롤러는 인터페이스의 최상단에 위치한 `navigation bar`와 인터페이스 최하단에 위치한 옵셔널(`?`) `toolbar`를 가진다.  

`navigation bar`는 항상 보여지며 네비게이션 컨트롤러 자체적으로 자식 뷰컨트롤러에서 제공되는 내용을 토대로 업데이트 시키면서 관리된다. [isToolbarHidden](https://developer.apple.com/documentation/uikit/uinavigationcontroller/1621875-istoolbarhidden) 속성이 **false** 인 상태라면 위와 비슷한 방식으로 `toolbar` 또한 최상위 뷰가 제공하는 내용을 기반으로 업데이트 되며 관리된다.

네비게이션 컨트롤러는 자신의 delegate 객체와에 맞춰 행동한다. delegate객체는 아래의 것들을 할 수 있다. 

1. 뷰 컨트롤러의 push/pop 재정의
2. 사용자 지정 애니메이션 전환 제공
3. 네비게이션 인터페이스의 기본 방향 명시

사용자가 생성한 delegate는 반드시 [UINavigationControllerDelegate](https://developer.apple.com/documentation/uikit/uinavigationcontrollerdelegate) 프로토콜을 준수해야한다.

<p align="center"><img width="50%" alt="image" src=https://user-images.githubusercontent.com/42789819/139540162-bfce7c8e-1879-4a2e-9ddb-d4ee477aa337.jpg></p>

> Figure 2 Objects managed by the navigation controller


## Navigation Controller Views

네비게이션 컨트롤러는 `container view controller`이다. 즉, 다른 뷰 컨트롤러들의 내용이 자신의 안에 내장되어 있다. 우리는 `view` 프로퍼티를 통해 네비게이션 컨트롤러의 뷰에 접근할 수 있다. 이 `view`는 navigation bar, 옵셔널인 toolbar, 그리고 최상위 뷰 컨트롤러의 content view를 통합한다. 비록 navigation bar, toolbar의 내용 자체는 변경이 될 지라도, 뷰 자체는 변경되지 않는다. `view`가 통합하고 있는 여러 요소들 중 변하는 것은 `navigation stack`에 의해 제공되는 최상위 뷰 컨트롤러의 content view이다.

<p align="center"><img width="70%" alt="image" src=https://user-images.githubusercontent.com/42789819/139541119-85bbb389-23cf-4c8f-b019-cac740fee399.png></p>

> Figure 3 The views of a navigation controller    
> iOS7 부터는 content view가 navigation bar 아래 있기 때문에 뷰를 구성할 때 이를 고려해야한다.

네비게이션 컨트롤러는 navigation bar와 toolbar의 생성과 설정그리고 보이는 것을 관리한다. 그렇기 때문에 navigation bar의 외형적인 프로퍼티들을 사용자화 하는것이 허락되지만 `frame`, `bounds`, `alpha`는 바꿔서는 안된다. 만약 **UINavigationBar**를 상속한다면, 무조건 [init(navigationBarClass:toolbarClass:)](https://developer.apple.com/documentation/uikit/uinavigationcontroller/1621866-init)를 통해 initialize해야한다. navigation bar를 숨기거나 보이게 하고 싶다면 [isNavigationBarHidden](https://developer.apple.com/documentation/uikit/uinavigationcontroller/1621850-isnavigationbarhidden) 혹은 [setNavigationBarHidden(_:animated:)](https://developer.apple.com/documentation/uikit/uinavigationcontroller/1621885-setnavigationbarhidden) 메서드를 사용해야한다.

네비게이션 컨트롤러는 [UINavtigationItem](https://developer.apple.com/documentation/uikit/uinavigationitem) class의 인스턴스인 navigation item 객체를 이용하여  navigation bar 를 구성한다. navigation bar의 전반적인 외형을 사용자화 하고 싶다면 [UIAppearance](https://developer.apple.com/documentation/uikit/uiappearance) API를 사용하면 된다.

## Updating the Navigation Bar

최상위 뷰 컨트롤러가 바뀔 때마다 네비게이션 컨트롤러는 navigation bar를 그에 맞게 업데이트한다. 특히 네비게이션 컨트롤러는 좌측, 중앙, 우측에 위치하는 `bar button item`의 디스플레이를 업데이트 한다. `Bar button items`은 [UIBarButtonItem](https://developer.apple.com/documentation/uikit/uibarbuttonitem) 클래스의 인스턴스이다. 우리는 필요에 맞게 이를 변경할 수 있다.

navigation bar에 색상을 넣는 것은 `tintColor`, `barTintColor` 속성에 의해 변경 가능하다.

`tintColor`는 bar 내부 item들의 tint color를 변경한다. 그리고 bar 자체의 tint color를 변경할 때에는 `barTintColor`를 사용한다. Navigation Bar는 현재 보여지고 있는 뷰 컨트롤러로의 tint color를 상속하지 않는다.

