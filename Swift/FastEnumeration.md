# Fast Enumeration 이란 무엇인지 설명하시오.

## Refer
* [apple - fast enumeration](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ObjectiveC/Chapters/ocFastEnumeration.html)
* [apple - NSFastEnumeration](https://developer.apple.com/documentation/foundation/nsfastenumeration/)

## Answer

### 정의
이름만 들으면 굉장히 생소한 Fast Enumertaion은 이미 자주 사용중일 가능성이 높다.   
> Objective-C에서는 Fase enumeration, Swift에서는 NSFastEnumeration 프로토콜을 찾아보면 된다.

Fast enumeration의 정의는 다음과 같다.

Fast enumeration is a language feature that allows you to efficiently and safely enumerate over the contents of a collection using a concise syntax.
> Fast enumeration은 간결한 구문을 사용하여 컬렉션의 내용을 효율적이고 안전하게 열거할 수 있는 언어 기능입니다. 

* 인스턴스가 다른 객체의 집합체에 접근을 제공하는 클래스면 `NSFastEnumeration` protocol 적용이 가능하다. 
* Foundation framework에 있는 collection classes들(`NSArray`, `NSDictionary`, `NSSet`)을 예로 들 수 있다.


### 문법
```objective-c
for ( Type newVariable in expression ) { statements }

Type existingItem;
for ( existingItem in expression ) { statements }
```
* 두 표현식 모두 `NSFastEnumeration` 프로토콜을 따르는 객체를 산출한다.
* 반복 포인터 (iterating variable : `newVariable`, `existingItem`) 는 오브젝트의 항목 소진으로 루프가 끝나면 `nil`을, 루프가 일찍 종료되면 가장 마지막에 가리킨 아이템이 남아 있게된다.


### 장점
- 열거에 있어서 `NSEnumerator` 같은 것들을 직접 사용하는 것보다 더 효율적이다.
- 문법이 간단하다.
- 안전하다.
    - 열거 중에 집합체를 수정하면 exception 발생한다. 열거체를 변형에 폐쇄적으로 만들기 때문에 안전하다.
- 일반 `for`문처럼 동작하기 때문에 `break`나 `continue` 를 사용할 수 있다.
