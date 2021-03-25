# 상태 변화에 따라 다른 동작을 처리하기 위한 AppDelegate 메서드들을 설명하시오.
> SceneDelegate가 등장함에 따라 SceneDelegate메소드도 함께 설명


<br>

## 참고하면 좋은 블로그
* [iOS13에서-분할된-AppDelegate와-추가된SceneDelegate이해하기](https://huniroom.tistory.com/entry/)
* [AppDelegate와 SceneDelegate-lena](https://velog.io/@dev-lena/iOS-AppDelegate와-SceneDelegate)


## 사전지식

* [SceneDelegate란?](./SceneDelegate.md)
* **iOS13부터 AppDelegate가 하는 일?**  


    이전에는 앱이 foreground에 들어가거나 background로 이동할 때 앱의 상태를 업데이트하는 등의 앱의 주요 생명 주기 이벤트를 관리했었지만 SceneDelegate로 위임되면서 현재 하는 일은 이하 5가지이다.

    1. 앱의 가장 중요한 데이터 구조를 초기화하는 것
    2. 앱의 scene을 환경설정(Configuration)하는 것
    3. 앱 밖에서 발생한 알림(배터리 부족, 다운로드 완료 등)에 대응하는 것
    4. 특정한 scenes, views, view controllers에 한정되지 않고 앱 자체를 타겟하는 이벤트에 대응하는 것(앱 시작, 종료)
    5. 애플 푸쉬 알림 서비스와 같이 실행시 요구되는 모든 서비스를 등록하는것

* 정리하면, 앱 실행 및 종료와 관련된 Process Life Cycle은 AppDelegate에서, 앱이 Foreground와 Background 상태에 있을 때 상태 전환과 관련된 UI Life Cycle은 SceneDelegate에서 관리한다.    
    > 만약 iOS12 이하 버전에 대응하기 위해 SceneDelegate를 사용하지 않도록 설정하면 예전처럼 AppDelegate가 모든 Life Cycle에 대한 관리 책임을 갖지만, 그렇지 않으면 UI Life Cycle은 SceneDelegate를 통해 관리해야 한다.


## 함께 보면 좋은 답변
* [앱이 In-Active 상태가 되는 시나리오를 설명하시오](./Progress_In-Active.md)
* [SceneDelegate에 대해 설명하시오](./SceneDelegate.md)


<br>

## Answer

**[call stack]**  
> iOS13이상 버전 : App하나에 여러 UI 인스턴스 존재 가능 (App Delegate가 Session을 관리하므로)  
> general: App Delegate  
> **`highlight`**: Scene Delegate  

앱 클릭 → didFinishLaunchingWithOptions → configurationForSession → **`willConnectToSession`**(아직 화면에는 앱이 안뜬 상태) → **`scene(_:willConnectTo)에서, window = UIWindow(windowScene: scene as! UIWindowScene)`**(화면에 앱이 등장) → **`willResignActive, didEnterBackground`** → **`didDisconnect`**(앱 화면을 명시적으로 종료할 시 ) → didDiscardSceneSceneSessions: scene이 didDisconnect됐을 경우(유저의 포커스에서 벗어난 후 다시 포커스를 받은 경우 데이터를 유지 하기 위해, 이곳에서 복구관련 정보를 획득 == 한 앱을 여러 화면 띄울 수 있음)


<br>

<details>
<summary> "AppDelegate의 메서드들" </summary>
<div markdown="1">       

<br>

```swift
func application (_ : didFinishLaunchingWithOptions :)-> Bool
```
* 앱 시작시 앱 설정이 완료될때 호출.
* iOS13 이전에는 이 메서드를 통해 UIWindow 개체를 구성하고 ViewController인스턴스를 할당했지만,iOS13 부터 애플리케이션에 장면이 있는 경우 AppDelegate는 더이상 이를 처리할 책임이 없고 SceneDelegate로 이동된다.


<br>


```swift
func application (_ : configurationForConnecting : options :)-> UISceneConfiguration
```
* 새 장면이나 새창이 필요할 때마다 호출된다.
* 이 메서드는 앱 시작시 호출되지 않고 새 장면 또는 새 창을 가져야 하는 경우에만 호출된다.


<br>

```swift
func application (_ : didDiscardSceneSessions :)
```
* 멀티 태스킹 창에서 스와이프 하는것과 같이 장면을 삭제할 때 또는 프로그래밍 방식으로 앱 제거시 호출된다.  


</div>
</details>


<br>

<details>
<summary> "SceneDelegate의 메서드들" - Real Answer</summary>
<div markdown="1">   


> AppDelegate의 UIWindow와 관련된 것은 이제 SceneDelegate의 UIScene입니다.


<br>

```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions)
```
* UI창을 만들고 root view controller 를 설정하고
["설정한 창을 ‘키’ 창으로 만듭니다"](https://huniroom.tistory.com/entry/iOSswiftUI-iOS13에서-분할된-AppDelegate와-추가된SceneDelegate이해하기).  
== "같은 수준 이하의 다른 모든 창앞에 해당 창을 배치합니다."
* 새로운 화면 객체가 앱에 추가 될 때마다 호출된다.


<br>

```swift
func sceneDidDisconnect(_ scene: UIScene)
```
* iOS 에서는 리소스를 확보하기위해 앱의 Scene이 백그라운드로 전환시마다 Scene를 완전 폐기할지 결정할 수 있다. 이것이 앱이 종료되거나 실행되지 않음을 의미하는것은 아니다. Scene만 Session에서 연결해제되고 활성화 되지 않는것이다.  
    * iOS에선 사용자가 특정 Scene을 포그라운드로 전환시 세션에 다시 연결하도록 결정 할 수 있다.
    * 이 메서드는 사용하지 않는 리소스를 삭제하는데도 사용할 수 있다.


<br>

```swift
func sceneWillResignActive(_ scene: UIScene)
```
* 앱이 백그라운드로 전환시 실행된다.


<br>

```swift
func sceneWillEnterForeground(_ scene: UIScene)
```
* 백그라운드에서 포그라운드로 전환시 실행된다.


<br>

```swift
func sceneDidBecomeActive(_ scene: UIScene)
```
* sceneWillEnterForeground 메서드 다음에 호출된다. 
* 장면이 설정되고 표시할 준비가 되었음을 알려준다.


<br>

```swift
func sceneDidEnterBackground(_ scene: UIScene)
```
* sceneWillEnterForeground 이후에 실행된다.
* 백그라운드에서 포그라운드로 전환 완료시 실행된다.



</div>
</details>


<br>
<br>