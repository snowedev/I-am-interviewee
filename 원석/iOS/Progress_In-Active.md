# 앱이 In-Active 상태가 되는 시나리오를 설명하시오


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
앱이 In-Active 상태가 되는 시나리오는 다음과 같습니다.

**[시나리오]**
1. 사용자가 앱을 실행합니다: `Not Running → In-Active → Active`
2. 앱 실행 도중 홈으로 나갑니다: `Active → In-Active → Background`  
    2-1. 앱을 다시 켭니다: `Background → Active`  
    2-2. 앱이 백그라운드에 있다가 Suspended 상태로 전이됩니다: `Active → In-Active → Background → Suspended`
