# SceneDelegate에 대해 설명하시오


## 도움을 준 좋은 블로그
* [AppDelegate와 SceneDelegate-lena](https://velog.io/@dev-lena/iOS-AppDelegate와-SceneDelegate)
* [Architecting Your App for Multiple Windows-WWDC19](https://developer.apple.com/videos/play/wwdc2019/258/)
* [iOS13이상 버전의 SceneDelegate-김종권의 iOS](https://ios-development.tistory.com/53)


## 필요한 사전 지식
* 객체 (Object): 클래스의 인스턴스.
* 인스턴스 (Instance): 클래스(즉, 객체)나 구조체, 열거형의 구조로 컴퓨터 저장공간에서 할당된 실체. 의미상으로 객체와 비슷
* 클래스 (Class): 특정한 타입의 객체에 공통되는 동작과 속성을 묘사한 코드 조각으로 본질적으로 객체의 청사진을 제공.


## 함께 보면 좋은 답변, 글
* [상태 변화에 따라 다른 동작을 처리하기 위한 AppDelegate 메서드들을 설명하시오.](./AppDelegate.md)
* [앱이 In-Active 상태가 되는 시나리오를 설명하시오](./Progress_In-Active.md)
* [UIScene, UIWindowScene, UISceneSession 이란 무엇인가](https://eunjin3786.tistory.com/164)

<br>

## Answer

iOS12까지 AppDelegate가 하던 일은 총 두가지였습니다.   
* 앱이 Launch되고, Terminate됐는지 등을 알 수 있게 해주는 **Process LifeCycle**과
* UI의 State를 알 수 있게 해주는 **UI LifeCycle**  

하지만 iOS13이 나옴에 따라 하나의 window가 하나의 UI를 갖던 기존의 방식은 하나의 window가 여러 UI를 가질 수 있게 변화하였습니다.

변화에 대응하기 위해 SceneDelegate가 등장했는데, SceneDelegate는 AppDelegate의 역할 중 UI의 상태를 알 수 있는 UILifeCycle에 대한 부분을 수행합니다.

구체적으로, SceneDelegate.swift파일은 UIWindowSceneDelegate 프로토콜을 구현하여, 현재 세션과 연결되는 새로운 화면 객체, 백그라운드와 포그라운드 간의 화면전환, 또는 앱에서 연결이 끊긴 화면과 같은 이벤트를 처리하는 메서드를 포함합니다.  


<br>

---
## 부가 설명: AppDelegate와 SceneDelegate

<img width=49% src=https://i.imgur.com/CDwg5Ny.png> <img width=49% src=https://i.imgur.com/D4VfgIv.png>

AppDelegate가 우측처럼 AppDelegate & SceneDelegate로 나누어짐에 따라 AppDelegate에서 `Session LifeCycle`이 AppDelegate에 추가 되었다.  


`Session LifeCycle`에는 두 메소드가 존재한다.  
각각의 메소드는 `Scene Session`이 생성되거나 삭제될 때 `AppDelegate`에 알리는 역할을 한다.
> Scene Session은 앱에서 생성한 모든 scene의 정보를 관리한다.


### Scene?

UIKit는 UIWindowScene 객체를 사용하는 앱 UI의 각 인스턴스를 관리한다.  
Scene에는 UI의 하나의 인스턴스를 나타내는 windows와 view controllers가 들어있다. 또한 각 scene에 해당하는 UIWindowSceneDelegate 객체를 가지고 있고, 이 객체는 UIKit와 앱 간의 상호 작용을 조정하는 데 사용된다.  
Scene들은 같은 메모리와 앱 프로세스 공간을 공유하면서 서로 동시에 실행되기때문에 결과적으로 하나의 앱은 여러 scene과 scene delegate 객체를 동시에 활성화할 수 있다!


### Scene Session?

UISceneSession 객체는 scene의 고유의 런타임 인스턴스를 관리한다. 사용자가 앱에 새로운 scene을 추가하거나 프로그래밍적으로 scene을 요청하면, 시스탬은 그 scene을 추적하는 session 객체를 생성하는것이다. 그 session에는 고유한 식별자와 scene의 구성 세부사항(configuration details)가 들어있다.  
UIKit는 session 정보를 그 scene 자체의 생애(life time)동안 유지하고 app switcher에서 사용자가 그 scene을 클로징하는 것에 대응하여 해당 session을 파괴한다.  

session 객체는 직접 생성하지않고 UIKit가 앱의 사용자 인터페이스에 대응하여 생성된다.


### 요약
`Session LifeCycle`에는 두개의 종이 있다. 하나는 `Scene`이 생겼을 때 치는 종, 하나는 `Scene`이 삭제되었을 때 치는 종. 종을 치는 이유는 `AppDelegate`에게 그 사실을 알려주기 위함이다(종 = 메소드).


그 종들을 치는 주체가 `Scene Session`이다. `Scene Session`은 `Scene`의 모든 행동들을 지켜보고 있으며 생성과 삭제가 될 때 마다 종을 쳐서 `AppDelegate`에게 그 사실을 알려주게 된다.