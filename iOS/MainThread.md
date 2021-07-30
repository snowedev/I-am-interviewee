# UIKit 클래스들을 다룰 때 꼭 처리해야하는 애플리케이션 쓰레드 이름은 무엇인가

## 도움이 된 글
* [Why the UI need to be updated on Main Thread](https://medium.com/@duwei199714/ios-why-the-ui-need-to-be-updated-on-main-thread-fd0fef070e7f)
* [왜 main.sync를 하면 안될까 - Zedd](https://zeddios.tistory.com/519)

## 도움이 될 답변
* [GCD](./GCD-API.md)


## Answer

UI 처리는 Main Thread에서 업데이트가 이루어져야한다. 
> main queue
> * 메인 스레드(UI 스레드)에서 사용 되는 **Serial Queue**
> * 모든 UI 처리는 메인 스레드에서 처리해야 함

UI업데이트가 Background Thread에서 이루어지면 어떻게 될까? 이게 Main Thread의 Block현상을 막기 위해서는 더 좋은 방법으로 생각 될 수 있지만 UI는 Main Thread에서 업데이트가 이루어져야한다. 

</br>

그 이유는 다음과 같다.

### `1. UIKit은 Thread-Safe하지 않다.`
> [atomic/nonatomic](https://hcn1519.github.io/articles/2019-03/atomic) 

UIKit의 대부분의 요소들은 Thread-Safe하지 않다(nonatomic). Apple이 UIKit을 이렇게 만든 이유는 가장 단순하고 큰 이유로 UIKit과 같이 매우 큰 프레임워크의 모든 요소들이 Thread-Safe하게 동작하도록 만드는것은 매우 복잡하고 성능면에서 효율적이지 않기 떄문이다. UI는 사용자에게 직접적으로 보여지는 부분이므로 성능과 효율, 안정성은 매우 중요하다.

Serial로 구현하면 위와같은 문제는 발생하지 않으므로. UI는 Main Thread에서 synchronously하게 동작하게 된다.


</br>

### `2. iOS의 Graphics rendering(graphics pipeline)은 궁극적으로 sync이다.`

![](https://user-images.githubusercontent.com/42789819/113587125-5236fd00-9669-11eb-9329-297dedd0dee4.png)

[요기](./Layer.md)에서도 공부했듯, 화면표시(실제로 뷰 위에 컨텐츠나 애니메이션을 그리는 행위)는 UIKit자신이 직접 행하지 않고 UIKit에 의해 Core Animation에게 위임하게 된다. 

이 떄, Core Animation에서는 Core Animation Pipeline을 사용하여 rendering을 하는데

Core Animation Pipeline의 처리 과정은 총 4단계로(Commit Transaction -> Render Server -> GPU -> Display)로 이루어져있다.

![](https://miro.medium.com/max/1400/1*MHtDsFMpROhOF7yVwYVvCA.jpeg)

> 대충 이런식인데..자세한건 [Why the UI need to be updated on Main Thread](https://medium.com/@duwei199714/ios-why-the-ui-need-to-be-updated-on-main-thread-fd0fef070e7f)을 참고하자..

무튼 이러한 전 과정이 사용자가 보는데에 불편함이 없으려면 현 60hz 주사율 기준으로 1/60초에 이루어져야하며, 과정 중에 application이 충돌이 나면 안된다. 

이를 Main Thread에서 단일화하여 처리하지 않고 여러 Thread에서 각각 업데이트를 진행하게 된다면, 각기 다른 뷰 계층 구조를 인코딩하여 렌더 서버로 전송할 것이고, 이에 따라 GPU 에 많은 렌더링 요청을 보내게 된다. 렌더링은 시스템 리소스가 많이 드는 작업이므로 GPU 가 이를 처리할 수 없어 심각한 문제가 발생한다.
> 만약(Main Thread가 아닌) BackGround Thread에서 각자의 Run-Loop로 업데이트를 하게 된다면, View가 제멋대로 동작할 수 있다. (예를 들어, 기기를 회전 했을때, 동시에 View의 Layout이 재배치되지 않고 각각 따로따로 동작하는 등)

이에 반해, Main Thread에서는 뷰의 변경사항을 받아서 자신의 Run Loop의 끝에서 한번에 반영한다. 이렇게하면 앱이 모든 변경 사항을 처리 할 수 있으며, 그것들이 동시에 활성화 될 수 있다. 이를 Main Thread의 View Drawing Cycle이라고한다.