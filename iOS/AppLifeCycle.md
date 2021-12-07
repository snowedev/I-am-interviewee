# App LifeCycle과 동작 시나리오에 대해 설명하시오


## 도움을 준 좋은 블로그
* [App Life Cycle[ rnfxl92]](https://velog.io/@rnfxl92/앱-생명주기-Application-Life-Cycle)


## 사전지식
* 앱이 Active일때와 In-Active일 때를 합쳐서 foreground라고 한다.
* In-Active는 앱이 실행중이지만 이벤트를 받지 않는 상태(백그라운드랑은 다름)
    * multitasking window로 진입하거나 app 실행중 전화, 알림 등에 의해 app을 사용할 수 없게 되는 경우 이 상태로 진입.

## 함께 보면 좋은 답변
* [상태 변화에 따라 다른 동작을 처리하기 위한 AppDelegate 메서드들을 설명하시오.](./AppDelegate.md)
* [SceneDelegate에 대해 설명하시오](./SceneDelegate.md)


## Answer

### `App State`
<p align="center"><img width=50% src="https://user-images.githubusercontent.com/42789819/144992111-0ec5f94f-1021-4375-b661-f3c78740b7e8.png"></p>
    
1. `Not Running`
    * 앱이 아직 실행되지 않았거나, 완전히 종료되어 동작하지 않는 상태.

2. `In Active` - Foreground
    * 앱이 실행중이지만 이벤트를 받지 않는 상태(백그라운드랑은 다름)
    * multitasking window로 진입하거나 앱 실행중 전화, 알림 등에 의해 앱을 사용할 수 없게 되는 경우 이 상태로 진입.

3. `Active` - Foreground
    * 앱이 실제 실행중이고 사용자 event를 받아서 상호작용할 수 있는 상태.(바로 Active가 되지 않고 Inactive 상태를 거쳐 Active상태가 된다.)
4. `Running`  - Background
    * 홈화면으로 나가거나 다른 앱으로 전환되어 현재 앱이 실질적인 동작을 하지 않는 상태. 
    * 음악 앱처럼 홈 화면으로 나가도 재생한 음악이 계속 실행되는 경우. 사용자가 앱을 사용하지 않는 동안 서버와 데이터를 동기화하거나 타이머가 실행되어야 하는 등의 작업을 이 상태에서 할 수 있다.

5. `Suspended` - Background
    * 앱 다시 실행했을 때 최근 작업을 빠르게 로드하기 위해 메모리에 관련 데이터만 저장되어있는 상태. 
    * 앱이 background 상태에 진입했을 때 다른 작업을 하지 않으면 Suspended 상태로 진입하게 된다(보통 2~3초 이내). 
    * Suspended 상태의 앱들은 iOS의 메모리가 부족해지면 가장 먼저 메모리에서 해제된다. 따라서 app을 종료시킨 적이 없더라도 app을 다시 실행하려고 하면 처음부터 다시 실행됨.

</br>

### `App Life Cycle`

<img width = 49% src="https://user-images.githubusercontent.com/42789819/144992588-501bbea3-ae9c-4b36-9ae1-999c55e41641.png"> <img width = 49% src=https://user-images.githubusercontent.com/42789819/144991832-6c8fc110-57de-4788-9105-2b4c95710f66.png>

> 좌측은 iOS13 이전 AppDelegate의 App Life Cycle  
> 우측은 iOS13 이후 SceneDelegate 생성 후 App Life Cycle

1. 앱 클릭하여 실행(Not Running -> InActive -> Active)
    * ```swift
        func application (_ : willFinishLaunchingWithOptions :)-> Bool
        ```
    * ```swift
        func application (_ : didFinishLaunchingWithOptions :)-> Bool
        ```
    * ```swift
        func application (_ : configurationForConnecting :)-> UISceneConfiguration
        ```
    * ```swift
        func scene (_ : willConnectTo)
        ```
    * ```swift
        func sceneWillEnterForeground(_ scene: UIScene)
        ```
    * ```swift
        func sceneDidBecomeActive(_ scene: UIScene)
        ```
2. 앱에서 다른 앱으로 이동할 때 or 홈화면으로 이동(Active -> InActive -> Background)

    * ```swift
        func sceneWillResignActive(_ scene: UIScene)
        ```
    * ```swift
        func sceneDidEnterBackground(_ scene: UIScene)
        ```

3. 앱이 켜진상태로 알림센터나 제어센터 확인 or 앱 사용중 전화수신 or 홈버튼/제스쳐를 통해 앱스위쳐로 진입(Active -> InActive)
    * ```swift
        func sceneWillResignActive(_ scene: UIScene)
        ```
    * ```swift
        func sceneDidBecomeActive(_ scene: UIScene)
        ```

4. 앱 사용하다가 종료(Active -> Not Running)
     * ```swift
        func sceneWillResignActive(_ scene: UIScene)
        ```
    * ```swift
        func sceneDidEnterBackground(_ scene: UIScene)
        ```
    * ```swift
        func application (_ : didDiscardSceneSessions :)
        ```
