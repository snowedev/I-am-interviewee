# 프로퍼티에 대해 설명하시오

## 참고한 좋은 글
* [[Swift]프로퍼티](https://jinshine.github.io/2018/05/22/Swift/6.프로퍼티(Property)/)
* [Swift의 프로퍼티에 대한 이해](https://soooprmx.com/swift-property/)
* [The Swift Language Guide - Properties](https://jusung.gitbook.io/the-swift-language-guide/language-guide/10-properties)
* [Properties - Stored Property(저장 프로퍼티)](https://zeddios.tistory.com/243)
* [Properties - Computed Property(연산 프로퍼티)](https://zeddios.tistory.com/245)
* [Properties - Property Observers(프로퍼티 옵저버)](https://zeddios.tistory.com/247)

## Answer

Property(프로퍼티): "재산", "소유물", "속성"

프로퍼티는 클래스나 구조체, 혹은 열거형의 객체 인스턴스가 그 내부에 가지고 있는, 객체의 상태에 관한 정보이다.

이러한 프로퍼티의 종류에는 크게 5가지가 있다.

1. [Stored Property(저장 프로퍼티)](#stored-property)
2. [Lazy Stored Property(지연 저장 프로퍼티)](#lazy-stored-property)
3. [Compute Property(연산 프로퍼티)](#compute-property)
4. [Property Observer(프로퍼티 옵저버)](#property-obserber)
5. [Type Property(타입 프로퍼티)](#type-property)


<br>

### `Stored Property`
가장 단순한 개념의 프로퍼티로써 클래스 또는 구조체의 인스턴스와 연관된 값을 저장하는 프로퍼티.

> 구조체는 저장 프로퍼티의 옵셔널 여부와 상관없이 저장 프로퍼티를 모두 포함하는 이니셜라이저가 자동으로 생성됨
>
> 클래스의 저장 프로퍼티는 옵셔널이 아닌 이상, 프로퍼티 기본 값을 지정해주거나 사용자정의 이니셜라이저를 통해 반드시 초기화 해주어야 함

* Struct(구조체)
    ```Swift
    struct MyPoint {
    var x: Int  // 저장 프로퍼티
    var y: Int  // 저장 프로퍼티
    }

    //구조체는 기본적으로 저장 프로퍼티를 매개변수로 가지는 이니셜라이져가 있다.
    let point: MyPoint = MyPoint(x: 10, y: 5)
    ```
    중요한 것은 구조체의 내부 변수가 MyPoint처럼 전부 let으로 선언된게 아니더라도 **인스턴스를 let으로 선언하게 되면 Value Type인 구조체의 특성상 내부 저장 프로퍼티들도 let으로 선언 된 것 같이 된다.**

    ```Swift
    let point: MyPoint = MyPoint(x: 10, y: 5)

    point.x = 0 // error: cannot assign to property: 'x' is a 'let' constant
    point.y = 0 // error: cannot assign to property: 'x' is a 'let' constant
    ```
* Class(클래스)
    ```swift
    class Position {
    var point: MyPoint  // 저장 프로퍼티(변수)
    let name: String    // 저장 프로퍼티(상수)

    // 저장 프로퍼티들에게 초기값이 없다면, 클래스는 init이 반드시 필요하다.
    init(name: String, currentPoint: MyPoint) {
            self.name = name
            self.point = currentPoint
        }
    }

    //클래스는 사용자정의 이니셜라이저를 호출해야만 한다.
    //그렇지 않으면 프로퍼티 초깃값을 할당할 수 없기 때문에 인스턴스 생성이 불가능 하다.
    let position: Position = Position(name: "동글이", currentPoint: point)
    ```
    
    **클래스는 구조체와 다르게 Reference Type이므로 인스턴스를 let으로 선언하여도 원본 값에 접근하기 때문에 var로 선언된 저장 프로퍼티가 var의 특성을 유지한다.**

    ```Swift
    let position: Position = Position(name: "동글이", currentPoint: point)

    position.point = MyPoint(x: 0, y: 0)
    position.name = "변경할래" // error: cannot assign to property: 'name' is a 'let' constant
    ```

<br>

### `Lazy Stored Property`
Lazy Stored Property(지연 저장 프로퍼티)는 초기화 시점에 초기값을 할당하는 것이 아니라, 해당 프로퍼티가 처음 **사용** 되는 시점으로 초기화를 미루는 프로퍼티이다. 

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

### `Compute Property`
특성 상태에 따른 값을 연산하는 프로퍼티. 인스턴스의 내/외부의 값을 연산하여 적절한 값을 return하는 `getter`의 역할이나 은닉화(private)된 내부의 프로퍼티 값을 간접적으로 설정하는 `setter`의 역할을 수행할 수도 있다.

굳이 메서드가 아닌 연산 프로퍼티를 사용하는 이유는 메서드로 구현시 getter 와 stter의 역할을 하는 메서드를 따로 따로 구현해주어야 하기 때문이다.
> setter만 구현하는 것은 불가하니 주의할 것


* 메서드 사용
    ```swift
    struct MyPoint {
        var x: Int
        var y: Int
        
        // 대칭점을 구하는 메서드 - 접근자(getter)
        func oppsitePoint() -> MyPoint {
            return MyPoint(x: -x, y: -y)
        }
        // setter
        mutating func setOppositePoint(_ opposite: MyPoint) {
            x = -opposite.x
            y = -opposite.y
        }
    }

    var position: MyPoint = MyPoint(x: 10, y: 20)

    // 현재 좌표 10, 15
    print(position)

    // 대칭 좌표 -10, -15
    print(position.oppsitePoint())

    // 좌표를 (-15, -10)으로 설정할래
    position.setOppositePoint(MyPoint(x: 15, y: 10))

    print(position) // -15, -10
    ```


* 연산 프로퍼티 사용
    ```swift
    struct MyPoint {
        var x: Int // 저장 프로퍼티
        var y: Int // 저장 프로퍼티
        
        // 연산 프로퍼티
        var oppsitionPoint: MyPoint {
            get {
                return MyPoint(x: x, y: y)
            }
            set (someParam) {
                x = -someParam.x
                y = -someParam.y
            }
        }
    }

    var position: MyPoint = MyPoint(x: 10, y: 20)
    print(position) // MyPoint(x: 10, y: 20)
    position.oppsitionPoint = MyPoint(x: 5, y: 10)
    print(position.oppsitionPoint) // MyPoint(x: -5, y: -10)
    ```
* 정리
    1. 클래스, 구조체, 열거형에 사용된다.

    2. var로 선언해야한다.
    
    3. 클래스, 구조체, 열거형에 값을 저장할 저장프로퍼티가 하나 있어야한다.==> 연산프로퍼티 자기 자신을 리턴하거나 값을 지정할 수 없다.
    
    4. get, set을 동시에 구현 가능하며, get만 구현하는 것도 가능(이 때는 get 키워드 생략 가능). 하지만 set만 구현하는 것은 안된다!
    
    5. set의 파라미터를 생략할 수 있으며(코드에서 `someParam`) 생략했을 시, newValue라는 키워드를 사용한다. 

<br>

### `Property Observer`

옵저버 프로퍼티를 사용하면 값이 변경됨에 따라 적절한 액션을 취할 수 있다. 프로퍼티의 값이 새로 할당될 때 마다, 혹은 변경되는 **값이 현재 값과 같더라도 호출된다.**
> **오직 저장 프로퍼티에만 적용할 수 있으며, lazy 프로퍼티에는 지정할 수 없다.**

* 정리
    - willSet: 
        - 호출 시기: `값이 저장되기 직전에 호출`
        - 전달되는 전달 인자: `프로퍼티가 변경될 값`
        - default 매개변수: `newValue`
    - didSet
        - 호출 시기: `새로운 값이 저장된 직후에 호출`
        - 전달되는 전달 인자: `프로퍼티가 변경되기 전의 값`
        - default 매개변수: `oldValue`

* 예제
    ```swift
    class StepCounter {

        //totalSteps는 "저장 프로퍼티"입니다!!
        var totalSteps: Int = 0 {

            // willSet 전달되는 전달 인자: "프로퍼티가 변경될 값"
            // newTotalSteps: 200 => 300
        willSet(newTotalSteps) {

                print("totalSteps을 \(newTotalSteps)로 설정하려고 합니다")
            }

            // didSet의 전달되는 전달 인자: "프로퍼티가 변경되기 전의 값"
            // oldTotalSteps: 0 => 200
            didSet(oldTotalSteps) {

                if totalSteps > oldTotalSteps  {

                    print("\(totalSteps - oldTotalSteps)걸음이 추가되었습니다.")
                }

            }
        }
    }

    let stepcounter = StepCounter()
    stepcounter.totalSteps = 200
    stepcounter.totalSteps = 300

    /**
    totalSteps을 200로 설정하려고 합니다
    200걸음이 추가되었습니다.
    totalSteps을 300로 설정하려고 합니다
    100걸음이 추가되었습니다.
    */
    ```

* 지역변수와 전역변수  
    프로퍼티를 계산하고 관찰하기 위해서 위에서 설명한 프로퍼티 옵저버 기능은 전역변수와 지역변수에서도 사용할 수 있다.
    * 전역변수: 함수, 메소드, 클로저 또는 type context `외부`에 정의되는 변수
    * 지역변수: 함수, 메소드 또는 closure context `내에서` 정의되는 변수
    * 전역상수(global constant)와 전역변수(global variable)은 항상 게으르게 연산된다. == 즉, 필요할 때 초기화.
    * 전역상수(global constant)와 전역변수(global variable)은 연산 프로퍼티와는 다르게 lazy키워드가 필요없다.
    * 지역상수(local constant)와 지역변수(local variable)은 게으르게 연산되지 않는다. 


<br>

### `Type Property`
이제까지 알아본 프로퍼티 개념은 모두 타입을 정의하고 그 타입의 인스턴스가 생성되었을때 사용될 수 있는 **인스턴스 프로퍼티** 이다. 

인스턴스 프로퍼티는 인스턴스를 새로 생성할 때 마다 초깃값에 해당하는 값이 프로퍼티의 값이 되고, 각 인스턴스마다 다른 값을 지닐 수 있게 해준다.

이와 다르게, 각각의 인스턴스가 아닌 타입 자체에 속하게 되는 프로퍼티를 타입 프로퍼티라고 한다.

* 정리
    1. 프로퍼티를 "타입 자체"에 연결할 수 있는데, 그게 타입 프로퍼티이다.

    2. 타입프로퍼티에는,  저장 타입 프로퍼티와 연산 타입 프로퍼티가 있다.
    
    3. 저장 타입 프로퍼티는 상수/변수 일 수 있으며((let / var로 선언이 가능), **무조건 기본값을 줘야한다.** 그리고 처음 엑세스 할 때는 초기화를 게으르게한다! 하지만 lazy키워드는 필요없다.
    
    4. 연산 타입 프로퍼티는 무조건 변수(var)로 선언되어야한다.

    5. 클래스 타입에 대한 연산 타입 프로퍼티(Computed type property)의 경우, "class" 키워드를 사용하여 서브클래스가 슈퍼클래스의 구현을 재정의(override)할 수 있다.

* 예제 1
    ```swift
    class Aclass {
        // 저장 타입 프로퍼티 => 무조건 초기화를 해주어야 함
        static var typeProperty: Int = 0
        
        // 연산 타입 프로퍼티 => 무조건 var로만 선언되어야 함
        static var typeComputedProperty: Int {
            get {
                return typeProperty
            }
            
            set {
                typeProperty = newValue
            }
        }
    }

    // 인스턴스 생성이 없이도 접근 가능
    Aclass.typeProperty = 123

    print(Aclass.typeProperty)  // 200
    print(Aclass.typeComputedProperty)  // 200
    ```

* 예제 2  
    > 일반 저장 프로퍼티는 클래스/구조체에서만 되지만  **저장 타입 프로퍼티는 열거형에서도 가능**
    ```swift
    enum SomeEnumeration {

    static var storedTypeProperty = "Some value."

    static var computedTypeProperty: Int {
           return 6
        }
    }
    ```

* 예제 3
    > 클래스 타입에 대한 연산 타입 프로퍼티(Computed type property)의 경우, "class" 키워드를 사용하여 서브클래스가 슈퍼클래스의 구현을 재정의(override)할 수 있습니다
    ``` swift
    class SomeClass {

        static var computedTypeProperty: Int {
            return 27
        }

        // class 키워드를 사용하여 서브 클래스에서의 재정의 허가
        class var overrideableComputedTypeProperty: Int {
            return 107
        }
    }

    class ChildSomeClass : SomeClass{
        override static var computedTypeProperty: Int {
            return 27 //error! cannot override static var
        }

        override static var overrideableComputedTypeProperty: Int{
            return 2222
        }
    }
    ```