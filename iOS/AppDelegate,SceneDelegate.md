# AppDelegate와 SceneDelegate에 대해 설명하시오


## 도움을 준 좋은 블로그
* [AppDelegate와 SceneDelegate-lena](https://velog.io/@dev-lena/iOS-AppDelegate와-SceneDelegate)
* [Architecting Your App for Multiple Windows-WWDC19](https://developer.apple.com/videos/play/wwdc2019/258/)
* [iOS13이상 버전의 SceneDelegate-김종권의 iOS](https://ios-development.tistory.com/53)
* [iOS13에서-분할된-AppDelegate와-추가된SceneDelegate이해하기](https://huniroom.tistory.com/entry/)
* [AppDelegate와 SceneDelegate-lena](https://velog.io/@dev-lena/iOS-AppDelegate와-SceneDelegate)

## 함께 보면 좋은 답변, 글
* [App Life Cycle?](./AppLifeCycle.md)
* [UIScene, UIWindowScene, UISceneSession 이란 무엇인가](https://eunjin3786.tistory.com/164)
* [UIWindow?](./UIWindow.md)

<br>

## Answer

### **iOS12 이전의 AppDelegate의 역할, SceneDelegate의 등장 배경**

iOS12까지 AppDelegate가 하던 일은 총 두가지였습니다.   
* 앱이 Launch되고, Terminate됐는지 등을 알 수 있게 해주는 **Process LifeCycle**과
* UI의 State를 알 수 있게 해주는 **UI LifeCycle**  

하지만 iOS13이 나옴에 따라 하나의 window가 하나의 UI를 갖던 기존의 방식은 하나의 window가 여러 UI를 가질 수 있게 변화하였습니다.

**변화에 대응하기 위해 SceneDelegate가 등장했는데**, SceneDelegate는 AppDelegate의 역할 중 UI의 상태를 알 수 있는 UILifeCycle에 대한 부분을 수행합니다. 그래서 기존의 AppDelegate가 하는 역할도 상당 부분 달라지게 되었습니다.

<!-- 구체적으로, SceneDelegate.swift파일은 UIWindowSceneDelegate 프로토콜을 구현하여, 현재 세션과 연결되는 새로운 화면 객체, 백그라운드와 포그라운드 간의 화면전환, 또는 앱에서 연결이 끊긴 화면과 같은 이벤트를 처리하는 메서드를 포함합니다.   -->

</br>

### **iOS13부터 AppDelegate가 하는 일?**

iOS13부터 SceneDelegate의 등장으로 AppDelegate는 아래 5가지 정도의 역할만을 수행합니다.

1. 앱의 가장 중요한 데이터 구조를 초기화하는 것
2. 앱의 scene을 환경설정(Configuration)하는 것
3. 앱 밖에서 발생한 알림(배터리 부족, 다운로드 완료 등)에 대응하는 것
4. 특정한 scenes, views, view controllers에 한정되지 않고 앱 자체를 타겟하는 이벤트에 대응하는 것(앱 시작, 종료)
5. 애플 푸쉬 알림 서비스와 같이 실행시 요구되는 모든 서비스를 등록하는것

* **정리하면, 앱 실행 및 종료와 관련된 `Process Life Cycle`은 `AppDelegate`에서, 앱이 Foreground와 Background 상태에 있을 때 상태 전환과 관련된 `UI Life Cycle`은 `SceneDelegate`에서 관리한다.**    
    > 만약 iOS12 이하 버전에 대응하기 위해 SceneDelegate를 사용하지 않도록 설정하면 예전처럼 AppDelegate가 모든 Life Cycle에 대한 관리 책임을 갖지만, 그렇지 않으면 UI Life Cycle은 SceneDelegate를 통해 관리해야 한다.

<br>


### **AppDelegate, SceneDelegate 메서드**

* `AppDelegate`
    ```swift
    func application (_ : didFinishLaunchingWithOptions :)-> Bool
    ```
    * 앱 시작시 앱 설정이 완료될때 호출.
    * iOS13 이전에는 이 메서드를 통해 UIWindow 개체를 구성하고 ViewController인스턴스를 할당했지만, iOS13 부터 애플리케이션에 scene이 있는 경우 AppDelegate는 더이상 이를 처리할 책임이 없고 SceneDelegate로 이동된다.
    
    </br>

    ```swift
    func application(_ : willFinishLaunchingWithOptions :)-> Bool	
    ```
    * 앱 최초 실행시 호출되는 메서드

    </br>

    ```swift
    func application (_ : configurationForConnecting : options :)-> UISceneConfiguration
    ```
    * 새 장면이나 새창이 필요할 때마다 호출된다.
    * 이 메서드는 앱 시작시 호출되지 않고 새 장면 또는 새 창을 가져야 하는 경우에만 호출된다.

    </br>

    ```swift
    func application (_ : didDiscardSceneSessions)
    ```
    * 멀티 태스킹 창(App Switcher)에서 한개 이상의 scene을 종료시켰을 때 또는 프로그래밍 방식으로 앱 제거시 호출된다.  

    </br>

    ```swift
    func applicationWillTerminate(_ :)
    ```
    * 앱이 사용자에 의해 종료될 때 호출된다.
    * iOS 3.x 이전이거나 백그라운드 작업이 있는 앱의 경우 그 작업을 위해 호출되지 않음.
        > 이때는 `willTerminateNotification`를 사용하여 종료를 구현할 수 있다.
    * 시스템이 판단했을 때 종료가 필요할 경우 호출될 수 있다.

</br>

* `SceneDelegate`

    ```swift
    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions)
    ```
    * scene이 앱에 추가될 때 호출된다. 단, 여기서 ViewController와 같은 클래스 객체를 만들어 사용할 때, 그 때는 아직 viewDidLoad()가 호출되지 않는다.
    * UI창을 만들고 root view controller 를 설정하고
    ["설정한 창을 ‘키’ 창으로 만듭니다"](https://huniroom.tistory.com/entry/iOSswiftUI-iOS13에서-분할된-AppDelegate와-추가된SceneDelegate이해하기).  
    == "같은 수준 이하의 다른 모든 창앞에 해당 창을 배치합니다."

    </br>

    ```swift
    func sceneWillResignActive(_ scene: UIScene)
    ```
    * InActive 상태로 전환되기 직전에 호출
    * 사용자가 scene과의 상호작용을 중지하여 앱이 백그라운드로 전환될 때 호출된다(다른 화면으로의 전환 등).

    </br>

    ```swift
    func sceneWillEnterForeground(_ scene: UIScene)
    ```
    * Active 상태로 전환되기 직전에 호출
    * scene이 foreground로 진입할 때 호출된다.

    </br>
    
    ```swift
    func sceneDidBecomeActive(_ scene: UIScene)
    ```
    * Active 상태로 전환된 직후에 호출
    * 장면이 설정되고 표시할 준비가 되었음을 알려준다.
    * App Switcher에서 선택되는 등 scene과의 상호작용이 시작될 때 호출된다.
        > App Switcher : 홈 버튼을 두번 누르거나 아이폰X 이상에서는 위로 스와이프할 때 현재 실행중인 앱들이 보이는 화면
    
    </br>

    ```swift
    func sceneDidEnterBackground(_ scene: UIScene)
    ```
    * Background 상태로 전환된 직후에 호출
    * scene이 background로 진입할 때 호출된다.

    </br>

    ```swift
    func sceneDidDisconnect(_ scene: UIScene)
    ```
    * iOS 에서는 리소스를 확보하기위해 앱의 Scene이 백그라운드로 전환시마다 Scene를 완전 폐기할지 결정할 수 있다. 이것이 앱이 종료되거나 실행되지 않음을 의미하는것은 아니다.(그건 AppDelegate의 didDiscardSceneSessions을 참고하자) Scene만 Session에서 연결해제되고 활성화 되지 않는것이다.  
        * iOS에선 사용자가 특정 Scene을 포그라운드로 전환시 세션에 다시 연결하도록 결정 할 수 있다.
        * 이 메서드는 사용하지 않는 리소스를 삭제하는데도 사용할 수 있다.

    </br>
---
## 부가 설명: SceneDelegate의 Session LifeCycle

<img width=49% src=https://i.imgur.com/CDwg5Ny.png> <img width=49% src=https://i.imgur.com/D4VfgIv.png>

AppDelegate가 우측처럼 AppDelegate & SceneDelegate로 나누어짐에 따라 AppDelegate에서 `Session LifeCycle`이 AppDelegate에 추가 되었다.  


`Session LifeCycle`에는 두 메소드가 존재한다.  
각각의 메소드는 `Scene Session`이 생성되거나 삭제될 때 `AppDelegate`에 알리는 역할을 한다.
> Scene Session은 앱에서 생성한 모든 scene의 정보를 관리한다.


### **Scene?**

UIKit는 UIWindowScene 객체를 사용하는 앱 UI의 각 인스턴스를 관리한다.  
Scene에는 UI의 하나의 인스턴스를 나타내는 windows와 view controllers가 들어있다. 또한 각 scene에 해당하는 UIWindowSceneDelegate 객체를 가지고 있고, 이 객체는 UIKit와 앱 간의 상호 작용을 조정하는 데 사용된다.  
Scene들은 같은 메모리와 앱 프로세스 공간을 공유하면서 서로 동시에 실행되기때문에 결과적으로 하나의 앱은 여러 scene과 scene delegate 객체를 동시에 활성화할 수 있다!


### **Scene Session?**

UISceneSession 객체는 scene의 고유의 런타임 인스턴스를 관리한다. 사용자가 앱에 새로운 scene을 추가하거나 프로그래밍적으로 scene을 요청하면, 시스탬은 그 scene을 추적하는 session 객체를 생성하는것이다. 그 session에는 고유한 식별자와 scene의 구성 세부사항(configuration details)가 들어있다.  
UIKit는 session 정보를 그 scene 자체의 생애(life time)동안 유지하고 app switcher에서 사용자가 그 scene을 클로징하는 것에 대응하여 해당 session을 파괴한다.  

session 객체는 직접 생성하지않고 UIKit가 앱의 사용자 인터페이스에 대응하여 생성된다.


### **요약**
`Session LifeCycle`에는 두개의 종이 있다. 하나는 `Scene`이 생겼을 때 치는 종, 하나는 `Scene`이 삭제되었을 때 치는 종. 종을 치는 이유는 `AppDelegate`에게 그 사실을 알려주기 위함이다(종 = 메소드).


그 종들을 치는 주체가 `Scene Session`이다. `Scene Session`은 `Scene`의 모든 행동들을 지켜보고 있으며 생성과 삭제가 될 때 마다 종을 쳐서 `AppDelegate`에게 그 사실을 알려주게 된다.
