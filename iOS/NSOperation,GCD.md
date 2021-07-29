# NSOperationQueue 와 GCD Queue 의 차이점을 설명하시오.

## 도움을 준 좋은 블로그
* [GCD Queue와 NSOperation Queue의 특징과 차이점
](https://www.zehye.kr/ios/2020/04/23/11iOS_GCD_NSOperation_queue/)


## 필요한 사전 지식
쉽고 편한 멀티 스레딩의 처리를 위해 Apple에서 두가지 API를 제공한다.
* GCD(Grand Central Dispatch) - C기반의 저수준 API

* NSOperation - Obj-C기반의 고수준 API  
    * GCD보다 약간의 오버헤드 발생 및 느린 속도를 갖고 있지만 GCD에서 직접 처리해야하는 작업(KVO관찰, 작업취소 등)을 지원하고 있기에 감수하고 사용할 만 함 


## Answer

* `OperationQueue`는 들어간 작업 사이의 의존성 관계(우선순위, 준비 상태)에 따라 동작하지만, GCD 큐인 `DispatchQueue`는 FIFO로 동작.

* `OperationQueue`는 **큐 안에 있는 시작되지 않은 작업을 취소할 수 있지만**, `GCD Queue`는 큐에 넣는 즉시 실행된다.
    > 취소 가능하다는 점을 이용하면, Rx를 사용하지 않는 경우에 비동기로 이미지를 받아올 경우 이를 취소하는데에 사용될 수 있다(21.07.29 곰튀김님의 RxSwift 4시간만에 끝내기 강좌를 듣다가 추가).

* `OperationQueue`는 복잡한 일을 처리하는 작업에 사용하고, `GCD Queue`는 단순하고 메모리의 오버헤드가 적은 작업에 사용한다.

    > **Operation Queue** : 비동기적으로 실행되어야 하는 작업을 객체 지향적인 방법으로 사용하는 데 적합하다. KVO(key Value Observing)를 사용해 작업 진행 상황을 감시하는 방법이 필요할 때도 적합하다.(GCD KVO 사용불가)
    >
    > **GCD :** 작업이 복잡하지 않고 간단하게 처리하거나 특정 유형의 시스템 이벤트를 비동기적으로 처리할 때 적합하다. 예를 들면 타이머, 프로세스 등의 관련 이벤트.

> Cell의 재사용은 내부적으로 Operation Queue를 사용


