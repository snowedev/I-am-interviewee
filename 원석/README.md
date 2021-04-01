# iOS 기술 면접을 준비하는 레포
> [👨🏻‍💻👩🏻‍💻iOS 면접에 나올 질문들 총 정리](https://github.com/JeaSungLEE/iOSInterviewquestions)를 참고하여 공부하는 Repository입니다.


<br>

# Required
## iOS
> 면접시기가 WWDC이후 (7월~11월)이라면 해당년도 WWDC세션보기!  
> [Apple All Videos](https://developer.apple.com/videos/all-videos/)

|Question|Answer|
|:----------|:-----:|
|Bounds 와 Frame 의 차이점을 설명하시오.|[✋🏽](./iOS/frame,bounds.md)|
|실제 디바이스가 없을 경우 개발 환경에서 할 수 있는 것과 없는 것을 설명하시오.|[✋🏽](./iOS/the-things-only-simulator.md)|
|앱이 foreground에 있을 때와 background에 있을 때 어떤 제약사항이 있나요?|[✋🏽](./iOS/foreground,background.md)|
|상태 변화에 따라 다른 동작을 처리하기 위한 AppDelegate 메서드들을 설명하시오.|[✋🏽](./iOS/AppDelegate.md)|
|Scene delegate에 대해 설명하시오.|[✋🏽](./iOS/SceneDelegate.md)|
|앱이 In-Active 상태가 되는 시나리오를 설명하시오.|[✋🏽](./iOS/Progress_In-Active.md)|
|NSOperationQueue 와 GCD Queue 의 차이점을 설명하시오.|[✋🏽](./iOS/NSOperation,GCD.md)|
|GCD API 동작 방식과 필요성에 대해 설명하시오.|-|
|자신만의 Custom View를 만들려면 어떻게 해야하는지 설명하시오.|[✋🏽](./iOS/xib.md)|
|iOS 앱을 만들고, User Interface를 구성하는 데 필수적인 프레임워크 이름은 무엇인가?|[✋🏽](./iOS/UIkit.md)|
|Foundation Kit은 무엇이고 포함되어 있는 클래스들은 어떤 것이 있는지 설명하시오.|-|
|Delegate란 무언인가 설명하고, retain 되는지 안되는지 그 이유를 함께 설명하시오.|-|
|NotificationCenter 동작 방식과 활용 방안에 대해 설명하시오.|[✋🏽](./iOS/NotificationCenter.md)|
|UIKit 클래스들을 다룰 때 꼭 처리해야하는 애플리케이션 쓰레드 이름은 무엇인가?|-|
|TableView를 동작 방식과 화면에 Cell을 출력하기 위해 최소한 구현해야 하는 DataSource 메서드를 설명하시오.|-|
|하나의 View Controller 코드에서 여러 TableView Controller 역할을 해야 할 경우 어떻게 구분해서 구현해야 하는지 설명하시오.|-|
|App Bundle의 구조와 역할에 대해 설명하시오.|-|
|View 객체에 대해 설명하시오.|-|
|UIView 에서 Layer 객체는 무엇이고 어떤 역할을 담당하는지 설명하시오.|-|
|UIWindow 객체의 역할은 무엇인가?|-|
|UINavigationController 의 역할이 무엇인지 설명하시오.|-|
|모든 View Controller 객체의 상위 클래스는 무엇이고 그 역할은 무엇인가?|-|
|앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체는 무엇인가?|-|
|UIApplication 객체의 컨트롤러 역할은 어디에 구현해야 하는가?|-|
|앱의 콘텐츠나 데이터 자체를 저장/보관하는 특별한 객체를 무엇이라고 하는가?|-|
|앱 화면의 콘텐츠를 표시하는 로직과 관리를 담당하는 객체를 무엇이라고 하는가?|-|
|Swift의 클로저와 Objective-C의 블록은 어떤 차이가 있는가?|-|
|App의 Not running, Inactive, Active, Background, Suspended에 대해 설명하시오.|-|
|App thinning에 대해서 설명하시오.|-|
|Global DispatchQueue 의 Qos 에는 어떤 종류가 있는지, 각각 어떤 의미인지 설명하시오.|-|


<br>

## Autolayout
|Question|Answer|
|:----------|:-----:|
|오토레이아웃을 코드로 작성하는 방법은 무엇인가? (3가지)|-|
|hugging, resistance에 대해서 설명하시오.|-|
|Intrinsic Size에 대해서 설명하시오.|-|
|스토리보드를 이용했을때의 장단점을 설명하시오.|-|
|Safearea에 대해서 설명하시오.|-|
|Left Constraint 와 Leading Constraint 의 차이점을 설명하시오.|-|


<br>

## Swift
|Question|Answer|
|:----------|:-----:|
|Optional 이란 무엇인지 설명하시오.|-|
|Fast Enumeration 이란 무엇인지 설명하시오. |-|
|Struct 가 무엇이고 어떻게 사용하는지 설명하시오.|-|
|instance 메서드와 class 메서드의 차이점을 설명하시오.|-|
|Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오.|-|
|Singleton 패턴을 활용하는 경우를 예를 들어 설명하시오.|-|
|KVO 동작 방식에 대해 설명하시오.|-|
|Delegates와 Notification 방식의 차이점에 대해 설명하시오.|-|
|멀티 쓰레드로 동작하는 앱을 작성하고 싶을 때 고려할 수 있는 방식들을 설명하시오.|-|
|MVC 구조에 대해 블록 그림을 그리고, 각 역할과 흐름을 설명하시오.|-|
|프로토콜이란 무엇인지 설명하시오.|-|
|Hashable이 무엇이고, Equatable을 왜 상속해야 하는지 설명하시오.|-|
|mutating 키워드에 대해 설명하시오.|-|
|탈출 클로저에 대하여 설명하시오.|-|
|Extension에 대해 설명하시오.|-|
|접근 제어자의 종류엔 어떤게 있는지 설명하시오|-|
|defer란 무엇인지 설명하시오.|-|
|defer가 호출되는 순서는 어떻게 되고, defer가 호출되지 않는 경우를 설명하시오.|-|


<br>

## ARC
|Question|Answer|
|:----------|:-----:|
|ARC란 무엇인지 설명하시오.|-|
|Retain Count 방식에 대해 설명하시오.|-|
|Strong 과 Weak 참조 방식에 대해 설명하시오.|-|
|ARC 대신 Manual Reference Count 방식으로 구현할 때 꼭 사용해야 하는 메서드들을 쓰고 역할을 설명하시오.|-|
|retain 과 assign 의 차이점을 설명하시오.|-|
|순환 참조에 대하여 설명하시오.|-|
|강한 순환 참조 (Strong Reference Cycle) 는 어떤 경우에 발생하는지 설명하시오.|-|
|특정 객체를 autorelease 하기 위해 필요한 사항과 과정을 설명하시오.|-|
|Autorelease Pool을 사용해야 하는 상황을 두 가지 이상 예로 들어 설명하시오. |-|


<br>

## Functional Programming
|Question|Answer|
|:----------|:-----:|
|함수형 프로그래밍이 무엇인지 설명하시오.|-|
|고차 함수가 무엇인지 설명하시오.|-|
|Swift Standard Library의 map, filter, reduce, compactMap, flatMap에 대하여 설명하시오.|-|

