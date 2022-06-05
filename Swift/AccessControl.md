# 접근 제어자의 종류엔 어떤게 있는지 설명하시오

## Answer

접근 제어자의 범위라고 해야하나 접근제어가 부여될 수 있는 큰 범위로는 `module`, `source file`이 있다. 

#### Module, Source File

`module`: 하나의 framework를 의미한다. import 키워드로 추가되는 것들을 모듈이라고 할 수 있다.  
`source file`: module은 source file들로 구성되어있다. ~.swift 와 같은 파일들을 예로 들 수 있겠다.  


#### 접근제어자(Access Control)

접근 제어자의 종류에는 5가지가 있다.

* open, public  
프로젝트 내의 모든 module에서 접근 할 수 있다. 가장 개방적인 제어자이다.  

* internal(default)  
코드가 작성된 module에서만 접근할 수 있다. 우리가 별도로 명시하지 않는다면 internal 키워드가 붙게된다.  

* fileprivate  
코드가 작성된 source file 내에서만 접근할 수 있도록 제어한다. 같은 source file이라면 다른 클래스라도 접근할 수 있다.  

* private  
가장 폐쇄적인 접근 제어자이다. 코드가 작성된 객체 안에서만 접근이 가능하다.  
fileprivate와 다르게 private 키워드가 붙었다면 같은 source file이라도 서로 다른 클래스에 있는 경우 접근이 불가하다.  


#### open, public

둘의 차이점에 대해 알아보자.

`open` 키워드는  
* class 앞에만 붙을 수 있다.    
* 왜냐면 open 키워드가 붙었다는 것은 다른 모듈에서 subclassing이 가능하다는 의미이기 때문이다.  
* 모듈 A에서 만들어둔 class를 모듈 B에서 subclassing하여 superClass로 삼기 위해서는 모듈 A의 class가 open 키워드로 선언되어야 한다.  


#### 접근 제어자의 소소한 특이점

* setter는 getter보다 더 제한적인 접근 제어자를 가질 수 있다.  
    ```swift
    // get은 internal set은 private  
    private (set) var name: String
    ```

* protocol에서 위를 활용하려면 서로 규칙을 지켜주어야 한다.  

private (set)을 프로토콜에서 사용하려면 protocol 조건을 잘 맞추어주어야 합니다.  
따라서 우리가 protocol 구현체에서 set에 대해 private 키워드를 붙이고 싶다면 protocol에서 또한 { get set }이 아닌 { get }만 가능해야 합니다.  

```swift
protocol Game {
    var theShow: String { get }
}

struct Ps5: Game {
    private (set) theShow: String
}
```

* Unit Test를 할 때에는 `@testable` 키워드를 붙임으로써 public과 open이 아닌 entity들도 사용할 수 있게해줍니다.  
```swift
@testable import {target_name}
```