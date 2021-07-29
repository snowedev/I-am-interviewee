# GCD API 동작 방식과 필요성에 대해 설명하시오.

## 도움을 받은 글
 - [GCD (Grand Central Dispatch) - 끄적이는 개발 노트](https://beenii.tistory.com/155)
 - [GCD에 대해서 알아보기 (DispathQueue) - 마기](https://magi82.github.io/gcd-01/)
 - [Concurrency Programming Guide - Dispatch Queues - Zedd](https://zeddios.tistory.com/513)
 - [GCD - Dispatch Queue사용법 (1) - Zedd](https://zeddios.tistory.com/516)
 - [Concurrency Programming Guide - DispatchQueue](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW1)

## 관련된 답변
- [Operation Queue, GCD Queue](./NSOperation,GCD.md)

## Answer


### **`개념`**
쉽고 편한 멀티 스레딩의 처리를 위해 Apple에서는 두가지 API를 제공한다.
* GCD(Grand Central Dispatch) - C기반의 저수준 API
* NSOperation - Obj-C기반의 고수준 API  
    * GCD보다 약간의 오버헤드 발생 및 느린 속도를 갖고 있지만 GCD에서 직접 처리해야하는 작업(KVO관찰, 작업취소 등)을 지원하고 있기에 감수하고 사용할 만 함 

그 중에서 GCD에는 **DispatchQueue**라는 클래스가 존재한다. 그리고 이 클래스는 실제로 해야할 Task를 담아두면 선택된 스레드에서 실행을 해주는 역할을 한다.

**DispatchQueue**
> Serial/Concurrent와 sync/async는 별개의 개념이다
* 종류
    * `Serial Dispatch Queue`
        * Serial Queue는 등록된 작업을 **한번에 하나씩 차례대로 처리**
        * 처리중인 작업이 **완료되면** 다음 작업을 처리
    * `Concurrent Dispatch Queue`
        * Concurrent Queue는 등록된 작업을 한번에 하나씩 처리 하지 않고 여러 작업들을 **동시에 처리**

* 메서드
    * `sync`
        * 동기처리 메서드
        * 큐에 작업을 추가했으면 그게 끝날때까지 “기다리는” 것(세탁기 돌려놓고 다 돌아갈때까지 중지)
    * `async`
        * 비동기처리 메서드
        * 큐에 작업을 추가한 뒤, 기다리는것도 없고 그냥 다른 작업을 바로 할 수 있는 것(세탁기 돌려놓고 책 읽는 것)


**앱 실행시 시스템에서 기본적으로 2개의 Queue를 만들어준다.**
> let wsQueue = DispatchQueue(label: "ws") 이런식으로 커스텀 큐 생성도 가능하다 

* `main queue`
    * 메인 스레드(UI 스레드)에서 사용 되는 `Serial Queue`
    * 모든 UI 처리는 메인 스레드에서 처리해야 함
* `global queue`
    * 편의상 사용할수 있게 만들어 놓은 `Concurrent Queue`
    * Global Queue는 처리 우선 순위를 위한 [qos(Quality of service)](./qos.md) 파라미터를 제공
    * 병렬적으로 동시에 처리를 하기때문에 작업 완료의 순서는 정할 수 없지만 우선적으로 일을 처리하게 할 수 있다


</br>
</br>


### **`동작방식`**

* `Concurrent && sync`
    ```swift
    // MARK: - Concurrent && sync
    DispatchQueue.global().sync {
        for i in 1...5 {
                print(i)
            }
            print("==================")
    }

    for i in 100...105 {
            print(i)
        }
        print("==================")
    ```
    * result
        > 큐가 Concurrent이긴 하지만 sync이기때문에 순차적으로 실행된다.
        ```xml
        1
        2
        3
        4
        5
        ==================
        100
        101
        102
        103
        104
        105
        ==================
        ```

</br>

* `Serial && sync`
    > [main.sync로 코드를 작성하지 않은 이유?](https://zeddios.tistory.com/519)
    ```swift
    // MARK: - Serial && sync
    let wsQueue = DispatchQueue(label: "ws")

    wsQueue.sync {
        for i in 1...5 {
                print(i)
            }
            print("==================")
    }

    for i in 100...105 {
            print(i)
        }
        print("==================")
    ```
    * result 
        > Concurrent && sync 와 결과가 같다.
        ```xml
        1
        2
        3
        4
        5
        ==================
        100
        101
        102
        103
        104
        105
        ==================
        ```

</br>

* `Concurrent && async`
    ```swift
    // MARK: - Concurrent && async
    DispatchQueue.global().async {
        for i in 1...5 {
            print("\(i)🚀")
        }
        print("==================")
    }
    DispatchQueue.global().async {
        for i in 200...205 {
            print("\(i)🥕")
        }
        print("==================")
    }

    for i in 100...105 {
        print("\(i)👻")
    }
    ```
    * result
        > 실행시마다 순서는 다르다.  
        > async이며 큐도 Concurrent이므로 동시다발적으로 코드가 실행된다.
        ```xml
        100👻
        200🥕
        1🚀
        101👻
        201🥕
        2🚀
        102👻
        202🥕
        203🥕
        3🚀
        204🥕
        103👻
        4🚀
        205🥕
        ==================
        5🚀
        ==================
        104👻
        105👻
        ```

</br>

* `Serial && async`
    ```swift
    // MARK: - Serial && async
    wsQueue.async {
        for i in 1...5 {
            print("\(i)🚀")
        }
        print("==================")
    }
    wsQueue.async {
        for i in 200...205 {
            print("\(i)🥕")
        }
        print("==================")
    }

    for i in 100...105 {
        print("\(i)👻")
    }
    ```
    * result
        > 실행시마다 순서는 다르다.  
        > async이기 때문에 👻이랑은 같이 나오지만 큐가 Serial이므로 무조건 🚀이 끝나야 🥕이 나온다.
        ```xml
        100👻
        101👻
        102👻
        1🚀
        103👻
        2🚀
        3🚀
        4🚀
        5🚀
        ==================
        104👻
        105👻
        200🥕
        201🥕
        202🥕
        203🥕
        204🥕
        205🥕
        ==================
        ```


</br>
</br>


### **`필요성`**

* `GCD 이전의 멀티 스레딩`

    [멀티 코어 프로세서](https://www.google.co.kr/search?q=%EB%A9%80%ED%8B%B0+%EC%BD%94%EC%96%B4+%ED%94%84%EB%A1%9C%EC%84%B8%EC%84%9C)에서는 프로그램의 동작을 멀티 프로세서에게 어떻게 잘 배분하는지가 중요하다.   
    GCD 이전에는 멀티 스레딩을 위해 Thread와 OperationQueue 등의 클래스를 사용했는데, 
    Thread는 복잡할 뿐만 아니라 임계 구역 (Critical Section) 등을 이용한 Lock을 관리하기 까다로웠다. 그리고 OperationQueue는 GCD에 비해 무겁고 [Boilerplate Code](https://www.google.co.kr/search?q=Boilerplate+Code)들이 많이 필요한 문제가 있었다.


* `GCD의 등장`

    그래서 Apple은 GCD를 내놓게된다. 

    GCD는 멀티 코어 프로세서 시스템에 대한 응용 프로그램 지원을 최적화하기 위해 Apple에서 개발한 기술로, **스레드 관리와 실행에 대한 책임을 애플리케이션 레벨에서 운영체제 레벨로 넘겨버린다.** GCD의 작업단위는 Block(Swift에서는 클로져)라고 불리며, DispatchQueue가 이 Block들을 관리한다.
    
    GCD는 각 애플리케이션에서 생성된 DispatchQueue를 읽는 멀티코어 실행엔진을 가지고 있는데, 이것이 Queue에 등록된 각 작업들을 꺼내서 스레드에 할당해준다. 그렇기 때문에 개발자는 내부 동작을 자세히 알 필요없이 Queue에 작업을 넘기기만 하면 되어서, Thread를 직접 생성하고 관리하는 것에 비해 관리도 용이하고 성능 면에서도 좋은 결과를 얻을 수 있다.

    이것이 GCD가 필요한 이유이다.