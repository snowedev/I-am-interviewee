# 앱의 콘텐츠나 데이터 자체를 저장/보관하는 특별한 객체를 무엇이라고 하는가?



## 참고한 좋은 글

* [NSUserDefaults - 공식문서](https://developer.apple.com/documentation/foundation/nsuserdefaults) == [UserDefaults - 공식문서](https://developer.apple.com/documentation/foundation/userdefaults/)
* [CoreData - 공식문서](https://developer.apple.com/documentation/coredata)
* [CoreData(1)](https://zeddios.tistory.com/987)



## 답변

공부를 위해 찾아보니 UserDefaults외에도 CoreData도 함께 많이 나오는 것 같다. 그래서 CoreData와도 조금 비교를 통해 차이점을 알아보고자 한다.

### CoreData

- 단일 기기에서 data를 유지 혹은 캐싱하거나, CloudKit을 사용하여 여러 기기에 data를 동기화 하는 것 
- 넓은 의미로는 앱의 모델 계층이며, 객체 그래프를 관리하는 **Framework** 이다.
- CoreData의 데이터모델 편집기를 통해서 사용자는 사용자 데이터 타입과 관계를 정의할 수 있다. 그리고  각각의 클래스 정의를 생성한다. 그러고나면 CoreData는 런타임에 객체 인스턴스를 관리하여 [다음의 기능](https://developer.apple.com/documentation/coredata)을 제공할 수 있다.
- 기능 중에는 Persistance 기능이 있는데 이를 통해서 앱이 삭제되더라도 남는 데이터의 영구적인 저장이 가능하다
  - 이는 SQLite에서도 제공이 되는 기능이라서 CoreData를 데이터베이스로 생각할 수도 있다.
  - 하지만 Persistance는 CoreData의 기능 중 하나 일 뿐 **CoreData == Database가 아니다.**



여기까지가 CoreData에 대한 간단한 설명입니다. 이제 UserDefaults에 대해 알아보겠습니다.



### UserDefaults

```swift
class UserDefaults: NSObject
@interface NSUserDefaults : NSObject
```

* NSUserDefaults(UserDefaults)는 **Class**이다.
* 사용자의 defaults database에 key-value 쌍으로 데이터의 저장을 돕는 **인터페이스**이다.
* **앱이 꺼져도 지속되지만, 앱 삭제시에는 CoreData와 다르게 함께 사라지는 데이터이다.**
* NSUserDefaults 클래스는 default system과 상호작용할 수 있는 프로그래밍 인터페이스를 제공한다. default system은 사용자의 선호도에 맞추어 앱이 자신의 행동을 사용자화할 수 있도록한다. 예를들어, 사용자가 선호하는 측정 단위 또는 미디어 속도를 명시하도록 할 수 있다. 앱들은 이러한 기본 값들을 사용자의 default database에 있는 파라미터들의 집합에 할당하여 저장한다. 파라미터들은 일반적으로 앱 시작시의 기본 상태 또는 기본적으로 동작하는 방식을 결정되는데에 사용되기 때문에 default라고 한다.
* 런타임에 사용자의 default database에서 값을 읽기 위해 NSUserDefaults 객체를 사용할 수 있는데, 매번 default database에 접근하는 것은 비효율적이므로 NSUserDefaults는 데이터들을 캐싱해놓는다.

> 종합적으로 NSUserDefaults는 Class이며, Key-Value 쌍으로 디바이스에 데이터 저장하는 것을 돕는 인터페이스이다.
>
> 일반적으로 앱 실행시 초기 셋팅이나 기본동작에 사용되는 기본값들을 저장한다.

#### 어디서 사용할까?

문서에서처럼 앱의 기본적인 셋팅에 필요한 값, 혹은 사용자가 선호도에 맞추어 설정한 값들이 다음 앱 실행시에도 유지가 되어야 할 때 사용할 수 있다(검색 필터 정보 / 온보딩을 보았는지 등). 서버에 올리지 않는 데이터이지만 지속적으로 참조해야하는 값들을 저장한다고 생각해도 될 것 같다. 

<img width=49% alt="image" src="https://user-images.githubusercontent.com/42789819/145992906-26413b6e-df1d-436d-adc4-b365ce47a949.png"> <img width=49% alt="image" src="https://user-images.githubusercontent.com/42789819/145993126-ec8bcd33-cdcf-4b11-9385-b0118f58cb77.png">

```swift
UserDefaults.standard.set(value, forKey: "CustomKey") // 인터페이스를 통해 key-value 쌍 저장
UserDefaults.standard.value(forKey: "CustomKey") // 저장했던 key값으로 value 참조
// 올바르지 않은 키 값 접근시 nil값 반환
```

