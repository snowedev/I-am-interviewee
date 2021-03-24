## iOS 앱을 만들고, User Interface를 구성하는 데 필수적인 프레임워크 이름은 무엇인가?


<br>
<br>

### 참고하면 좋은 사이트
[UIKit](https://developer.apple.com/documentation/uikit)


### 사전지식
* 사실 iOS이전의 macOS에서는 비슷한 개념으로 `Appkit(Application kit)`이라는 프레임워크를 사용했었지만, iOS로 넘어오면서 비슷한 개념을 가진 `UIkit`으로 대체되었다.
 

### Answer
UI(User Interface)라는 이름에서 알 수 있듯이, UIKit 프레임워크는 **사용자의 인터페이스를 관리하고, 이벤트를 처리하는게 주 목적인 프레임워크이다.**

`UIkit`에서 주로 처리하는 사용자 이벤트로는 `제스처 처리`, `애니메이션`, `그림 그리기`, `이미지 처리`, `텍스트 처리` 등이 있다. 또한 테이블뷰, 슬라이더, 버튼, 텍스트 필드, 얼럿 창 등 애플리케이션의 화면을 구성하는 요소도 포함한다.


그렇기 때문에, 우리가 자주 사용하는 `UIViewController`, `UIView`(당연히 이를 상속하는 Button, TextField 등도 포함), `UIAlertController`등 앞에 `UI`가 붙는 클래스들을 사용하려면 반드시 `UIkit`을 상속해야 한다.