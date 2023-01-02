# defer란 무엇인지 설명하시오.

## 답변
<br>
함수 안에서 작성 위치와 상관없이 함수가 종료되기 전에 사용되는 클로저 입니다.<br>
defer를 영어 그대로 해석하여 함수 종료 직전까 실행을 미루다 라고 생각하시면 됩니다.


## 사용법
함수 안에서 어디서든 defer를 구현하면 됩니다.
```swift
func testDefer() {
  print("#1")
  
  defer { print("#2") }
  
  print("#3")
}
```
일반적인 실행 순서에 의하면<br>
```
#1
#2
#3
```
이 맞는 순서겠지만 #2은 defer 클로저를 사용했으므로 위 함수의 실행 결과는<br>
```
#1
#3
#2
```
의 순서로 출력됩니다.<br>

## 주의점
1. defer를 읽기 전에 함수가 종료되면 defer는 실행되지 않는다.
```swift
func testDefer() {
  print("#1")
  return
  
  defer { print("#2") }
  
  print("#3")
}
```
위와같이 defer가 읽히기도 전에 종료된 경우 defer는 실행되지 않습니다. 따라서 위 함수의 출력은
```
#1
```
이 됩니다.<br>

2.defer의 실행 순서는 읽은 순서의 역순이다.<br>
stack에 담겨져있다가 호출이 되는 것 같습니다.
```swift
func testDefer() {
  defer { print("#1") }
  defer { print("#2") }
  defer { print("#3") }
}
```
위 함수의 경우 다음과 같이 출력됩니다.
```
#3
#2
#1
```
3. defer는 중첩이 가능하며 바깥쪽 부터 실행된다.
```swift
func testDefer() {
    defer {
        defer {
            defer {
                print("defer #3")
            }
            print("defer #2")
        }
        print("defer #1")
    }
}
```
위 함수의 출력 결과는 다음과 같습니다.
```
#1
#2
#3
```

## 그렇다면 언제 사용하는가??
함수 종료 전 변수나 상수를 처리용으로 사용합니다.<br>
예를들어 NSLock 이용시
```swift
let myLock: NSLock = .init()

func fetchData() {
    myLock.lock()
    
    defer { myLock.unlock() }
    
    if data == nil { return }
    
    //이후 작업 ~.~
```

## 참고한 블로그
https://babbab2.tistory.com/80


