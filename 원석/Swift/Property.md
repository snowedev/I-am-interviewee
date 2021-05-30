# 프로퍼티에 대해 설명하시오

## 참고한 좋은 글
* [[Swift]프로퍼티](https://jinshine.github.io/2018/05/22/Swift/6.프로퍼티(Property)/)
* [Swift의 프로퍼티에 대한 이해](https://soooprmx.com/swift-property/)

## Answer

Property(프로퍼티): "재산", "소유물", "속성"

프로퍼티는 클래스나 구조체, 혹은 열거형의 객체 인스턴스가 그 내부에 가지고 있는, 객체의 상태에 관한 정보이다.

이러한 프로퍼티의 종류에는 크게 5가지가 있다.

1. 저장 프로퍼티
2. 느긋한(lazy) 프로퍼티
3. 계산(연산) 프로퍼티
4. 타입 프로퍼티
5. 프로퍼티 옵저버


<br>

### 1. 저장 프로퍼티
가장 단순한 개념의 프로퍼티로써 클래스 또는 구조체의 인스턴스와 연관된 값을 저장하는 프로퍼티.

> 구조체는 저장 프로퍼티의 옵셔널 여부와 상관없이 저장 프로퍼티를 모두 포함하는 이니셜라이저가 자동으로 생성됨
>
> 클래스의 저장 프로퍼티는 옵셔널이 아닌 이상, 프로퍼티 기본 값을 지정해주거나 사용자정의 이니셜라이저를 통해 반드시 초기화 해주어야 함
```Swift
struct MyPoint {
  var x: Int  // 저장 프로퍼티
  var y: Int  // 저장 프로퍼티
}

//구초제는 기본적으로 저장 프로퍼티를 매개변수로 가지는 이니셜라이져가 있다.
let point: MyPoint = MyPoint(x: 10, y: 5)

class Position {
  var point: MyPoint  // 저장 프로퍼티(변수)
  let name: String    // 저장 프로퍼티(상수)

  init(name: String, currentPoint: MyPoint) {
      self.name = name
      self.point = currentPoint
  }
}

//클래스는 사용자정의 이니셜라이저를 호출해야만 한다.
//그렇지 않으면 프로퍼티 초깃값을 할당할 수 없기 때문에 인스턴스 생성이 불가능 하다.
let position: Position = Position(name: "동글이", currentPoint: point)
```


<br>

### 2. 느긋한(lazy) 프로퍼티
lazy 프로퍼티는 초기화 시점에 초기값을 할당하는 것이 아니라, 해당 프로퍼티가 처음 get 되는 시점으로 초기화를 미루는 프로퍼티이다. 

프로퍼티를 초기화하는데 많은 연산 혹은 메모리를 필요로 하거나, 네트워크 접속과 같이 시간이 오래걸리는 작업을 포함해야 한다면 해당 프로퍼티의 사용 여부가 불확실한 상황에서 무조건 함께 초기화하는 것은 좋지 않은 전략이다. 

보통 복잡한 클래스나 구조체를 구현할 때 많이 사용되곤 한다.

```Swift
struct MyPoint {
  var x: Int = 0
  var y: Int = 0
}

class Position {
  lazy var point: MyPoint = MyPoint()
  let name: String

  init(name: String) {
      self.name = name
  }
}
//Position이라는 객체를 생성됬지만 아직 MyPoint 객체에 생성되지 않은 상태.
let position: Position = Position(name: "동글이")

//이 코드를 통해 point프로퍼티로 처음 접근할 때 point 프로퍼티의 CoordinatePoint가 생성됩니다.
print(position.point)
```

이러한 특성을 이용하여 불필요한 공간 낭비 와 성능 저하를 해결할 수 있다.


<br>

### 3. 연산 프로퍼티
특성 상태에 따른 값을 연산하는 프로퍼티. 인스턴스의 내/외부의 값을 연산하여 적절한 값을 return하는 `getter`의 역할이나 은닉화(private)된 내부의 프로퍼티 값을 간접적으로 설정하는 `setter`의 역할을 수행할 수도 있다.

굳이 메서드가 아닌 연산 프로퍼티를 사용하는 이유는 메서드로 구현시 getter 와 stter의 역할을 하는 메서드를 따로 따로 구현해주어야 하기 때문이다.
> setter만 구현하는 것은 불가하니 주의할 것


`메서드 사용`
```swift
// 메서드 사용
struct MyPoint {
    var x: Int
    var y: Int
    
    // 대칭점을 구하는 메서드 - 접근자(getter)
    func oppsitePoint() -> MyPoint {
        return MyPoint(x: -x, y: -y)
    }
    
    mutating func setOppositePoint(_ opposite: MyPoint) {
        x = -opposite.x
        y = -opposite.y
    }
}

var position: MyPoint = MyPoint(x: 10, y: 20)

//현재 좌표 10, 15
print(position)

//대칭 좌표 -10, -15
print(position.oppsitePoint())

//대칭 좌표를 (15, 10)으로 설정
position.setOppositePoint(MyPoint(x: 15, y: 10))

print(position) // -15, -10
```


`연산 프로퍼티 사용`
```swift
// 연산 프로퍼티 사용
struct MyPoint {
    var x: Int
    var y: Int
    
    var oppsitionPoint: MyPoint {
        get {
            return MyPoint(x: 10, y: 20)
        }
        set (someParam) {
            x = -someParam.x
            y = -someParam.y
        }
    }
}
```

<br>

### 4. 프로퍼티 옵저버

옵저버 프로퍼티를 사용하면 값이 변경됨에 따라 적절한 액션을 취할 수 있다. `프로퍼티의 값이 새로 할당될 때 마다, 혹은 변경되는 값이 현재 값과 같더라도 호출된다.`
> 오직 저장 프로퍼티에만 적용할 수 있으며, lazy 프로퍼티에는 지정할 수 없다.

- willSet: 
    - 전달되는 전달 인자: `프로퍼티가 변경될 값`
    - default 매개변수: `newValue`
- didSet
    - 전달되는 전달 인자: `프로퍼티가 변경되기 전의 값`
    - default 매개변수: `oldValue`

```swift
class Account {
    var credit: Int = 0 {
        willSet {
            print("잔액이 \(credit)에서 \(newValue)원으로 변경될 예정입니다.")
        }
        
        didSet {
            print("잔액이 \(oldValue)에서 \(credit)으로 변경되었습니다.")
        }
    }
}

let myAccount: Account = Account()
myAccount.credit = 1000

//잔액이 0에서 1000원으로 변경될 예정입니다.
//잔액이 0에서 1000으로 변경되었습니다.
```

<br>

### 5. 타입 프로퍼티
이제까지 알아본 프로퍼티 개념은 모두 타입을 정의하고 그 타입의 인스턴스가 생성되었을때 사용될 수 있는 인스턴스 프로퍼티 이다. 

인스턴스 프로퍼티는 인스턴스를 새로 생성할 때 마다 초깃값에 해당하는 값이 프로퍼티의 값이 되고, 각 인스턴스마다 다른 값을 지닐 수 있게 해준다.

즉, 각각의 인스턴스가 아닌 타입 자체에 속하게 되는 프로퍼티를 타입 프로퍼티라고 한다.


```swift
class Aclass {
    // 저장 프로퍼티
    static var typeProperty: Int = 0
    
    var instanceProperty: Int = 0 {
        didSet {
            Aclass.typeProperty = instanceProperty + 100
        }
    }
    
    // 연산 타입 프로퍼티
    static var typeComputedProperty: Int {
        get {
            return typeProperty
        }
        
        set {
            typeProperty = newValue
        }
    }
}

Aclass.typeProperty = 123

let classInstance: Aclass = Aclass()
classInstance.instanceProperty = 100

print(Aclass.typeProperty)  // 200
print(Aclass.typeComputedProperty)  // 200
```