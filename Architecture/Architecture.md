# Architecture(MVC, MVVM, MVP)


## 참고하면 좋은 글
* [iOS 아키텍처 패턴(MVC, MVVM, VIPER)](http://labs.brandi.co.kr/2018/02/21/kimjh.html)
* [protocorn93님-iOS Architecture](https://github.com/protocorn93/iOS-Architecture)


## Answer

### MVC
<img width=49% src=https://user-images.githubusercontent.com/42789819/119346603-61cfdb00-bcd5-11eb-8edf-fac28ebd0000.png> <img width=50% src=https://user-images.githubusercontent.com/42789819/119346608-64323500-bcd5-11eb-8f34-2189931aba28.png>

`M`odel(모델), `V`iew(뷰), `C`ontroller(컨트롤러). 


Model에서는 애플리케이션에서 사용할 데이터들을 관리하고, View는 유저 인터페이스를 표현 및 관리한다. Controller는 View와 Model의 다리 역할을 해 View의 입력을 Model이 반영하고, Model의 변화를 View에 갱신하는 역할을 한다. 


**하지만, 애플의 MVC 패턴(우측)은 기존 MVC 패턴과 다르다.** View와 Controller가 강하게 연결되어 있어 View Controller가 거의 모든 일을 한다.
> View Controller에서는 Controller가 View의 life cycle(라이프 사이클)에 관여하기 때문에 View와 Controller를 분리하기 어렵다. 개발자들 사이에서는 Massive View Controllers라고도 불린다. 이는 앱을 테스트할 때, Model은 따로 분리되어 테스트를 할 수 있어도 View와 Controller는 강하게 연결되어 있기 때문에 각각 테스트하기 어렵다는 데에서 나온 별칭이다. 


MVC는 독립적인 테스팅이 힘들고 Massive View Controller라는 별칭이 붙을 정도로 규모가 큰 프로젝트로 갈수록 장점보다 단점이 더 부각되는 아키텍쳐이다.

* Distribution : View와 Model은 확실히 분리되어 있다. 하지만 View와 Controller는 강하게 연결되어 있다.
* Testability : View와 Controller가 강하게 연결되어 있기 때문에 오로지 Model만 테스팅을 진행할 수 있다.
* Easy of Use : 여러 아키텍쳐 중 가장 적은 코드를 필요로 하며 가장 친숙한 아키텍쳐 패턴으로 많은 경험이 없는 개발자들도 쉽게 유지 보수할 수 있다.


<br>

### MVP
<img width=70% src=https://user-images.githubusercontent.com/42789819/119347959-2fbf7880-bcd7-11eb-99fe-f23069436f14.png>

`M`odel(모델), `V`iew(UIView/UIViewController), `P`resenter

 MVC와는 다르게 MVP에서는 UIView나 UIViewController 둘 모두 View에 해당한다(이로인해 MVC에서의 테스팅 한계를 어느정도 극복했다). MVC에서 UIViewController는 Controller에 해당했었고 그로인해 View와 강하게 연결되어 있었다. 이 둘을 View로 분류하는 대신 MVP 패턴에서는 Presenter라는 것이 등장한 것이다.

Presenter는 View(UIView, UIViewController)의 Life Cycle에 영향을 받지 않고 레이아웃 코드 역시 Presenter에 존재하지 않는다. 보다 Controller의 역할에 맞게 View를 데이터와 상태에 맞추어 갱신하는 역할을 갖게 된다. 즉, Presenter는 Model로 부터 갱신된 데이터를 받아와 뷰를 갱신하는 역할을 한다.

View는 Presenter를 소유하고 있어야 하며 Presenter는 유저 액션, 데이터 갱신, 상태 갱신에 따라 View를 갱신해주어야 한다. 이를 코드로써 구현할 때 View는 Presenter를 강한 참조로 소유하고 있고 Presenter는 약한 참조로 View를 단순히 가리키고만 있는다. 그렇기 때문에 View의 Life Cycle의 영향과 레이아웃 코드와 액션 코드가 공존하는 등의 의존성에서는 벗어날 수 있지만 참조에 의한 1:1 의존성에서는 벗어날 수 없다는 한계가 존재한다.

* Distribution : 전통적인 MVC에서 발생한 Model과 View의 의존성 문제는 해결하였다. 참조에 의한 View와 Controller의 의존성은 존재하지만 비교적 셋 모두 역할별로 적절히 나누어져 있다고 말할 수 있다.
* Testability : 각각의 요소를 독립적으로 테스팅하기 용이하다.
* Easy of Use : Presenter의 추가와 이를 구현하기 위한 프로토콜등의 추가로 코드가 MVC보다 길어진다.


<br>

### MVVM
<img width=70% src=https://user-images.githubusercontent.com/42789819/119346611-64cacb80-bcd5-11eb-891c-0f0e101c8ecd.png>

`M`odel(모델), `V`iew(뷰), `V`iew`M`odel(뷰모델). 


Controller를 빼고 ViewModel을 추가한 패턴이다. 여기서 View Controller가 View가 되고, ViewModel이 중간 역할을 한다. View와 ViewModel 사이에는 Binding(바인딩-연결고리)이 있다. ViewModel은 Model에 변화를 주고, ViewModel을 업데이트하는데 이 때, 바인딩으로 인해 View도 업데이트된다. 

ViewModel은 View에 대해 아무것도 모르기 때문에 테스트가 쉽고 바인딩으로 인해 코드 양이 많이 줄어든다는 장점이 있다.

MVVM에서 바인딩을 직접 작성하지 않으려면 
1. KVO 기반 라이브러리를 사용하거나 
2. Delegation
3. Property Observer
4. RxSwift같은 Reactive Programming을 사용해야한다.

MVVM하면 RxSwift를 주로 떠올리는 이유이다.