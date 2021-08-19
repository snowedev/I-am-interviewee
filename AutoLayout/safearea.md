# SafeArea

## 참고한 좋은 글
* [iOS Safe Area
](https://medium.com/rosberryapps/ios-safe-area-ca10e919526f)

## Answer

iOS 7에서 Apple은 topLayoutGuide과 bottomLayoutGuide이라는 속성을 소개하였다. 

<img width=30% src=https://user-images.githubusercontent.com/42789819/130085430-dcb90eff-fc6e-4588-8680-0872ac63ec4d.png>

이는 상태바(Status Bar), 내비게이션 바(Navigation Bar), 탭바(Tabbar) 등에 의해서 View가 가려지지 않기 위해서 제공 되던 속성들이었는데 이는 이름에서도 알 수 있듯 상단과 하단의 공간을 보호(?)해주는 역할을 하고 있었다.

하지만 iPhoneX부터 노치가 생겨났고 이로 인해 Apple은 iOS11에서 SafeArea를 소개한다. 그냥 원래 속성을 써도 문제가 되지 않을 것이라 생각할 수 있지만 문제가 되었던 이유는 휴대폰의 가로모드 전환(Landscape)에서 노치가 우측 혹은 좌측으로 갈 때 컨텐츠가 가려지기 때문이었다. 그로인해 노치 디자인부터는 top/bottom 마진이 아니라 명확한 SafeArea영역이 필요하게되었다.

<img width=50% src=https://miro.medium.com/max/1400/1*rwWMirhEosAfaBnkWOwBpg.png>


그래서 iOS11부터는 기존에 쓰이던 속성 두가지가 Deprecated되고 Top, Bottom, Leading, Trailing 마진을 모두 가지는 `SafeArea`가 등장하게 된다.

그래서 Xcode9 부터는 기본적으로 SafeArea가 적용이 되어있지만 속성값의 변경을 통해 AutoLayout을 잡을 때 특정 값에 SuperView를 지정해줄 수도 있다. Apple에서는 화면에 꽉 차야하는 경우가 아니라면 SafeArea를 준수할 것을 권고하고 있다.