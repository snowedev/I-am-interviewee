# Class 와 Struct의 차이


## Answer

```swift
// Struct
struct Resolution {
    var width = 0
    var height = 0
}
// Class
class VideoMode {
    var resolution = Resolution()
    var interlaced = false
    var frameRate = 0.0
    var name: String?
}
```

```swift
// Instance
let someResolution = Resolution()
let someVideoMode = VideoMode()
```


### struct
* **call by value (할당 또는 파라미터 전달시 value copy가 일어남)**
* stack memory 영역에 할당 (속도가 빠름)
* scope based lifetime: 컴파일타임에 compiler가 언제 메모리를 할당/해제할지 정확히 알고있음
* data locality: CPU 캐시 히트율이 높음
* 상속 불가능 (protocol은 사용 가능)
* NSData로 serialize 불가능
* Codable 프로토콜을 이용하여 손쉬운 JSON <-> struct 변환 가능 (Swift 4 이상)
* 항상 새로운 변수로 copy가 일어나기때문에 multi-thread 환경에서 공유변수로 인해 문제를 일으킬 확률이 적음


### class
* **call by reference (할당 또는 파라메터 전달시 객체를 가리키고있는 메모리 주소값만 복사됨)**
* heap memory 영역에 할당 (속도가 느림)
* 런타임에 직접 alloc하며 reference counting을 통해 dealloc이 필요
* memory fragmentation 등의 overhead가 존재
* 상속 가능
* NSData serialize 가능
* Codable 사용 불가능
* 런타임에 타입 캐스팅을 통해서 클래스 인스턴스에 따라 여러 동작이 가능
* deinitializer 존재

> 참고: class안에 struct 변수를 property로 정의하는것은 가능하며, 반대로 struct의  property중 하나로 class 인스턴스 변수를 갖고있는 것도 가능하다.    
> 이 경우 해당 struct 변수의 copy가 일어날때 class 인스턴스의 주소값만 복사된다.


### **핵심** 

`call by value` VS `call by reference`

구조체(struct)와 열거형(enum)은 `call by value`로서, 할당 또는 파라미터 전달 시 value copy가 일어난다.

하지만 Class는 `call by reference`로서, 할당 또는 파라미터 전달 시 value copy 대신에 객체를 가리키고 있는 주소값만 복사되어 참조가 이루어진다.

우리가 사용하는 배열, 딕셔너리, 셋과 같은 컬렉션 타입은 구조체로서 구현되어 있다.