# 상태 변화에 따라 다른 동작을 처리하기 위한 AppDelegate 메서드들을 설명하시오.
> SceneDelegate가 등장함에 따라 SceneDelegate메소드도 함께 설명


## 참고하면 좋은 블로그
* [iOS13에서-분할된-AppDelegate와-추가된SceneDelegate이해하기](https://huniroom.tistory.com/entry/)
* [AppDelegate와 SceneDelegate-lena](https://velog.io/@dev-lena/iOS-AppDelegate와-SceneDelegate)


## Answer


### iOS13부터 AppDelegate가 하는 일?
이전에는 앱이 foreground에 들어가거나 background로 이동할 때 앱의 상태를 업데이트하는 등의 앱의 주요 생명 주기 이벤트를 관리했었지만 더이상 하지 않습니다.
현재 하는 일은
1. 앱의 가장 중요한 데이터 구조를 초기화하는 것
2. 앱의 scene을 환경설정(Configuration)하는 것
3. 앱 밖에서 발생한 알림(배터리 부족, 다운로드 완료 등)에 대응하는 것
4. 특정한 scenes, views, view controllers에 한정되지 않고 앱 자체를 타겟하는 이벤트에 대응하는 것.
5. 애플 푸쉬 알림 서브스와 같이 실행시 요구되는 모든 서비스를 등록하는것.
입니다.


### `AppDelegate`


```swift
func application (_ : didFinishLaunchingWithOptions :)-> Bool
```
* 앱 시작시 앱 설정이 완료될때 호출됩니다.
* iOS13 이전에는 이 메서드를 통해 UIWindow 개체를 구성하고 ViewController인스턴스를 할당합니다. 하지만,iOS13 부터 애플리케이션에 장면이 있는 경우 AppDelegate는 더이상 이를 처리할 책임이 없고 SceneDelegate로 이동됩니다.


```swift
func application (_ : configurationForConnecting : options :)-> UISceneConfiguration
```
* 새 장면이나 새창이 필요할 때마다 호출됩니다.
* 이 메서드는 앱 시작시 호출되지 않고 새 장면 또는 새 창을 가져야 하는 경우에만 호출됩니다.

```swift
func application (_ : didDiscardSceneSessions :)
```
* 멀티 태스킹 창에서 스와이프 하는것과 같이 장면을 삭제할 때 또는 프로그래밍 방식으로 앱 제거시 호출됩니다.  


### `SceneDelegate`
AppDelegate의 UIWindow와 관련된 것은 이제 SceneDelegate의 UIScene입니다.
```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions)
```
UI창을 만들고 root view controller 를 설정하고
설정한 창을 ‘키’ 창으로 만듭니다.
키창으로 만든다라는 말이 무슨말인지 좀더 자세히 알아보기 위해 해당 링크를 첨부합니다.
Apple Developer Documentation
“설정한 창을 ‘키’ 창으로 만듭니다.”
=> 같은 수준 이하의 다른 모든 창앞에 해당 창을 배치합니다.

```swift
func sceneDidDisconnect(_ scene: UIScene)
```
앱에 Scene이 백그라운드로 전환시마다 iOS 에서는 리소스를 확보하기위해 Scene를 완전 폐기할지 결정할 수 있다.
이것은 앱이 종료되거나 실행되지 않음을 의미하진 않지만 Scene만 Session에서 연결해제되고 활성화 되지 않습니다.
iOS에선 사용자가 특정 Scene을 포그라운드로 전환시 세션에 다시 연결하도록 결정 할 수 있습니다.
이 메서드는 사용하지 않는 리소스를 삭제하는데도 사용할 수 있습니다. 

```swift
func sceneDidBecomeActive(_ scene: UIScene)
```
WillEnterForeground 메서드 다음에 호출된다. 장면이 설정되고 표시할 준비가 되었음을 알려준다.

```swift
func sceneWillResignActive(_ scene: UIScene)
```

앱이 백그라운드로 전환시 실행된다.

```swift
func sceneWillEnterForeground(_ scene: UIScene)
```
백그라운드에서 포그라운드로 전환시 실행

```swift
func sceneDidEnterBackground(_ scene: UIScene)
```
백그라운드에서 포그라운드로 전환시 실행
sceneWillEnterForeground 이후에 실행됨.


SceneDelegate.swift파일은 UIWindowSceneDelegate 프로토콜을 구현하여, 현재 세션과 연결되는 새로운 화면 객체, 백그라운드와 포그라운드 간의 화면전환, 또는 앱에서 연결이 끊긴 화면과 같은 이벤트를 처리하는 메서드를 포함한다.
SceneDelegate의 모든 메서드는 앱의 생명주기 동안 초기화와 초기화 해제 작업을 수행하는데 유용하다.
하지만, 이 파일에서 가장 궁요한 델리게이트 메서드는 새로운 화면 객체가 앱에 추가 될 때마다 호출되는 willConnectTo메서드다.
디폴트로, Xcode에 의해 구현되는 willConnectTo 델리게이트 메서드는 ContentView.swift 파일에 선언된 ContentView의 인스턴스를 생성하고 사용자에게 보이도록 한다. 이 메서드 내에서 UIKit 아케텍처와 SwiftUI 간의 간격이 해소된다.
UIKit 프로젝트 내에SwiftUI 뷰가 포함되기 위해서 SwiftUI뷰는 UIHostingController 인스턴스에 포함된다.
