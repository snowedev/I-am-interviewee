# UIView 에서 Layer 객체는 무엇이고 어떤 역할을 담당하는지 설명하시오.

## 도움을 준 좋은 블로그
* [CALayer-이동건님](https://baked-corn.tistory.com/110)


## 사전 지식

**[iOS에서 Graphic 요소들을 그려내기 위해 존재하는 것들]**
> Core Graphics 에서 UIKit 으로 갈 수록 간편해지고 코드량도 줄었지만 사용할 수 있는 기능은 제한된다. 반면 저수준의 프래임워크는 보다 많은 기능을 사용할 수 있지만 UIKit, AppKit 에서 기본적으로 제공하는 기능들을 직접 구현해야하는 경우가 생긴다

<img width=30% src=https://user-images.githubusercontent.com/42789819/113587125-5236fd00-9669-11eb-9329-297dedd0dee4.png>

* 가운데 보이는 OpenGL은 iOS 디바이스의 Graphics Hardware와 가장 빠르고 직접적인 접근을 지원한다. 하지만 그만큼 간단한 작업조차 굉장히 많은 양의 코드를 필요로 하기 때문에 효율성이 떨어진다.

* 비효율적인 OpenGL을 보완하기 위해 Core Graphics와 Core Animation이 존재한다.

* CALayer는 Core Animation이 제공하는 요소 중 하나이다.
> CALayer를 제공함으로써 보다 효율적으로 OpenGL의 장점을 가져갈 수 있음



## Answer

Layer객체는 UIView에 속하며, UIView를 지원해주는 역할을 합니다. UIView는 1.화면표시 2.터치 이벤트 처리 3.subView 등의 작업을 하는데 이때, 1.화면표시(실제로 뷰 위에 컨텐츠나 애니메이션을 그리는 행위)는 자신이 직접 행하지 않고 UIKit에 의해 Core Animation에게 위임하게 됩니다. Core Animation이 제공하는 요소 중에 하나인 CALayer가 바로 그 역할을 수행하게 됩니다.


CALayer의 속성으로는 대표적으로 cornerRadius, shadow, border, frame, bounds 등이 있습니다.


## 덭붙이기
cornerRadius나 shadow 관련 코드에서 많이 보이는 clipsToBounds, masksToBounds는 같은 역할을 하지만 각각 UIView와 CALayer에 속한 프로퍼티라는 점이 다릅니다. 

둘은 SubView나 SubLayer의 내용(글씨 등)이 루트 Layer의 경계를 무시하고 자신을 보여주느냐 경계에 맞추어 잘리느냐를 설정해주는 Bool타입의 메서드입니다.

<img width="49%" alt="image" src="https://user-images.githubusercontent.com/42789819/113591359-c1fbb680-966e-11eb-9cc3-4096d0d808f1.png">

<img width="49%" alt="image" src="https://user-images.githubusercontent.com/42789819/113591319-b4463100-966e-11eb-81ac-114815f8a004.png">
