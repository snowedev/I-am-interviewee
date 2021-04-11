# ARC란 무엇인지 설명하시오.


<br>

## 도움을 준 좋은 사이트
* [The Swift Language Guide-ARC](https://jusung.gitbook.io/the-swift-language-guide/language-guide/23-automatic-reference-counting)



## Answer

ARC란 Automatic Reference Counting의 준말로써, 자동 참조 카운트라는 의미입니다. 

Swift에서는 앱의 메모리 사용을 관리하기 위해서 ARC를 사용하는데, 자동으로 참조 횟수를 관리하기 때문에 ARC가 알아서 더 이상 사용하지 않는 인스턴스를 메모리에서 해지합니다(Run time에 계속 실행되는 것이 아니라 Complie time(build할 때)에 실행).

때문에 개발자가 메모리 관리에 신경 쓸 필요가 없다는 장점이 있습니다.
> 참조 횟수(Reference Counting)는 클래스 타입의 인스턴스에만 적용되며, 값 타입인 구조체/열거형 등에는 적용되지 않습니다.


## 더 나아가기

ARC는 사용 가능성이 있는 인스턴스의 메모리를 해제하는 일이 없도록 하기 위해 얼마나 많은 프로퍼티, 상수, 혹은 변수가 그 인스턴스를 참조하고 있는지 추적한다. 그렇기 때문에 ARC에서는 해당 인스턴스에 대한 참조가 최소 하나라도 존재한다면 그 인스턴스를 메모리에서 해지 하지 않는다!

## 생각해보기 

**Run time에 계속 실행되는 것이 아니라 Complie time(build할 때)에 실행된다고 하였는데 그럼 어떻게 동적으로 실행되는 것들의 reference count를 세고 메모리 관리를 할 수 있을까?**