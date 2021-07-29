# Observer Pattern에 대해 설명하시오

## 참고하면 좋은 글
* [Observer Pattern(Swift)](https://linsaeng.tistory.com/6?category=753322)'
* [Observer Pattern in Swift](https://duwjdtn11.tistory.com/547)


## Answer

### `Observer`: 관찰자.

Observer Pattern은 해당 프로퍼티가 변경 되는걸 관찰하고 있다가 변경 되는 시점에서 update가 수행 되게 되는 형태를 말한다. Delegate Pattern의 1:1 관계와 다르게, 객체간의 1:N 의존 관계를 구성하는 패턴이다.  


특정 값이 변경이 발생 할 때 다른 연쇄적으로 그 값을 참조 하고 하고 있는 값들이 자동 적으로 변경이 이루어 져야 될 때 사용 하면 유용하다.


따라서, NotificationCenter와 존재 이유가 비슷하다(자세한 건 하단에서).  


### `Observer Pattern with protocols`

* Observerable 한 패턴을 위해 프로토콜을 통한 접근 방식 정의
    ```swift
    // MARK: -Observerable Protocol
    protocol Observer {
        var id: Int { get }
        func update()
    }
    ```

* Subject 클래스: 객체에게 업데이트를 알리는 역할
    > 옵저버를 attach하면, 이를 ObserverArray에 넣는다.  
    >
    > 다른 객체에게 notify하게 되면 이 ObserverArray를 순회하며 Observer Protocol 내 update() 함수를 호출한다.
    ```swift
    class Subject {
        private var observerArray = [Observer]()
        private var _number = Int()
        
        // number변수가 초기화되면 notify
        var number: Int {
            set {
                _number = newValue
                notify()
            }
            get {
                return _number
            }
        }
        
        // 옵저버 등록
        func attachObserver(observer: Observer) {
            observerArray.append(observer)
        }
        
        // 등록된 옵저버들에게 notify
        private func notify() {
            for observer in observerArray {
                observer.update()
            }
        }
    }
    ```
    * 프로코톨 채택
    ```swift
    // 2진수 변환
    class BinaryObserver: Observer {
        private var subject = Subject()
        var id = Int()
        
        init(subject: Subject, id: Int) {
            self.subject = subject
            self.subject.attachObserver(observer: self) // 옵저버 등록
            self.id = id
        }
        
        func update() {
            print("\(subject.number)의 2진수 변환: \(String(subject.number, radix:2))")
        }
    }

    // 16진수 변환
    class HexObserver: Observer {
        private var subject = Subject()
        var id = Int()
        
        init(subject: Subject, id: Int) {
            self.subject = subject
            self.subject.attachObserver(observer: self) // 옵저버 등록
            self.id = id
        }
        
        func update() {
            print("\(subject.number)의 16진수 변환: \(String(subject.number, radix:16))")
        }
    }


    // 8진수 변환
    class OctaObserver: Observer {
        private var subject = Subject()
        var id = Int()
        
        init(subject: Subject, id: Int) {
            self.subject = subject
            self.subject.attachObserver(observer: self) // 옵저버 등록
            self.id = id
        }
        
        func update() {
            print("\(subject.number)의 8진수 변환: \(String(subject.number, radix:8))")
        }
    }
    ```
    * notify
    ```swift
    let subject = Subject()

    let binary = BinaryObserver(subject: subject, id: 1)
    let octa = OctaObserver(subject: subject, id: 2)
    let hex = HexObserver(subject: subject, id: 3)

    subject.number = 15 // notify
    subject.number = 2 // notify
    ```

    * output
    ```xml
    15의 2진수 변환: 1111
    15의 8진수 변환: 17
    15의 16진수 변환: f

    2의 2진수 변환: 10
    2의 8진수 변환: 2
    2의 16진수 변환: 2
    ```

## More
Observer Pattern은 observer를 등록하고 notification을 post한다는 데에 있어서 NotificationCenter와 그 동작 방식이 비슷하다. 


그럼 뭐가 다를까? NotificationCenter는 Observer Pattern의 개념을 구현한 것이다. 


따라서 NotificationCenter는 기존 Observer Pattern에서 observer를 등록하고 notification을 주는 역할만 빼서 추상화 레벨을 올린 구현체라는 차이점이 있다.

