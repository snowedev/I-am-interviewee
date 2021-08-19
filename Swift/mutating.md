# mutating 키워드에 대해 설명하시오.

## 참고한 좋은 글
* [The Swift Programming Language-Methods](https://docs.swift.org/swift-book/LanguageGuide/Methods.html)

## 연관된 답변
* [Instance/Type Method](./Instance,Type-Method.md)

## Answer

`mutate`: 변화하다, 변화시키다.

무엇을 변화시키는거냐면 바로 value type이다. 기본적으로 value type(struct, enum)의 property들을 인스턴스 메서드에 의해 수정될 수 없다.

하지만 인스턴스 메서드 앞에 `mutating`이라는 키워드를 붙인다면 이야기가 달라진다.

`mutating`을 선언한 메서드는 메서드 내에서 property를 변경할 수 있고, 메서드가 종료될 때 변경한 모든 내용을 기존 value type property에 다시 기록한다.

* struct
    ```swift
    struct Point {
        var x = 0.0, y = 0.0
        mutating func moveBy(x deltaX: Double, y deltaY: Double) {
            x += deltaX
            y += deltaY
        }
    }
    var somePoint = Point(x: 1.0, y: 1.0)
    print("The point is now at (\(somePoint.x), \(somePoint.y))")
    // Prints "The point is now at (1.0, 1.0)"

    somePoint.moveBy(x: 2.0, y: 3.0)
    print("The point is now at (\(somePoint.x), \(somePoint.y))")
    // Prints "The point is now at (3.0, 4.0)"
    ```

* enum
    ```swift
    enum TriStateSwitch {
        case off, low, high

        mutating func next() {

            switch self {

            case .off:
                self = .low

            case .low:
                self = .high

            case .high:
                self = .off
            }
        }
    }
    var ovenLight = TriStateSwitch.low
    ovenLight.next() // ovenLight == .high
    ovenLight.next() // ovenLight == .off
    ```

하지만 let으로 선언된 인스턴스는 property가 변수이더라도 속성을 변경할 수 없기 때문에 mutating 메서드를 호출할 수 없다.

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
let somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0) // error: cannot use mutating member on immutable value: 'somePoint' is a 'let' constant 
```