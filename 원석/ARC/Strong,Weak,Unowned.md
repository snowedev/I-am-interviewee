# Strong 과 Weak 참조 방식에 대해 설명하시오.

## 참고한 좋은 글



## Answer

### Strong
* 해당 인스턴스에 대한 소유권을 갖는다.
* 자신이 참조하는 인스턴스의 RC를 증가시킨다.
* 인스턴스 생성시 & 참조시 retain, 참조 종료시 release
* weak, unouwned로 선언하지 않았다면 default로 strong


### Weak
* 해당 인스턴스에 대한 소유권을 가지지 않는다. **주소값**만 가진다.
* 그래서 RC도 증가하지 않고 release도 일어나지 않는다.
* Optional 타입으로 nil이 될 수 있다.
> weak을 통해 강한순환참조를 해결할 수 있다!!


## 더 나아가기

### Unowned
* weak이랑 동일하지만 절대 nil이 될 수 없다는 점이 다름(Optional이 아니란 소리!)
* 안전하지 않음(비추)