## 앱이 foreground에 있을 때와 background에 있을 때 어떤 제약사항이 있나요?


<br>

## 참고하면 좋은 블로그
* [App States(앱의 상태)란 [엠아이노의 iOS]](https://minosaekki.tistory.com/16)
* [Preparing Your UI to Run in the Background [Apple]](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background)


## 사전 지식
* foreground 상태의 앱은 사용자가 보고 있는 화면이다. 그렇기 때문에 CPU를 비롯한 시스템 자원의 우선순위가 높은 상태
* background 상태란 앱이 홈화면에 들어가서 사용자한테 보이지 않는 상태이다. 
    > 💡 앱이 background 상태가 되어도 계속 실행해야 될 때가 존재한다.  
    > ▶️ 음악 앱을 이용하여 노래를 듣는 경우처럼 다른 어플을 사용하더라도 멈추면 안되는 앱들

* App State(앱 상태)
    * `Not running`: 앱이 아예 실행되지 않았거나 시스템에 의해 종료되었을 때의 상황
    * `Inactive`: 앱이 foreground 상태이기는 하나 이벤트를 받지 못한 상태
    * `Active`: 앱이 foreground에서 실행 중이며 이벤트를 받았을 때의 상황
    * `Background`: 앱이 background에 있으며 코드를 실행하고 있는 상태
    * `Suspended`: 앱이 background이며 앱이 메모리에 남아 있긴 하나 코드를 실행하고 있지 않은 상태
        
        <img width=30% src=https://user-images.githubusercontent.com/42789819/112316844-d34bd700-8cee-11eb-8e31-2925e98b1231.png>

* App Launch Cycle
    * 위에서  언급한 각 App State 의 상태 변화는 Delegate 메서드들의 호출이 동반된다.
    * 각 메서드들은 앱의 상태 변화에 대해 적절한 방식으로 대응할 수 있는 기회를 가지게 해준다

        |Method|Description|
        |:----|:----|
        |application(_:didFinishLaunching:)|앱이 처음 시작될 때 실행|
        |applicationWillResignActive: |앱이 `Active` 에서 `Inactive`로 이동될 때 실행|
        |applicationDidEnterBackground: |앱이 `background` 상태일 때 실행|
        |applicationWillEnterForeground: |앱이 `background`에서 `foreground`로 이동 될때 실행 (아직 foreground에서 실행중이진 않음)|
        |applicationDidBecomeActive: |앱이 `Active`상태가 되어 실행 중일 때|
        |applicationWillTerminate: |앱이 종료될 때 실행|


<br>

## Answer
* foreground에 있을 때에는 메모리 및 기타 시스템 리소스에 대해서 background보다 높은 우선순위를 가지며 시스템은 이러한 리소스를 사용할 수 있도록 필요에 따라 background 앱을 종료합니다.
* background에 있을 때에는 가능한 적은 메모리공간을 사용해야하며(시스템 리소스 해제, 메모리에서 해제 후 데이터를 디스크에 작성) 우선순위에 의해 foreground task보다 더 낮은 자원을 할당받습니다.



