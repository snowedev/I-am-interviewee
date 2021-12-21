# ARC란 무엇인지 설명하시오.


<br>

## 도움을 준 좋은 사이트

* [The Swift Language Guide-ARC](https://jusung.gitbook.io/the-swift-language-guide/language-guide/23-automatic-reference-counting)
* [[Swift] ARC 뿌시기](https://sujinnaljin.medium.com/ios-arc-뿌시기-9b3e5dc23814)

## Answer

#### [**블로그에 정리한 내용**](https://snowedev.github.io/2021/07/12/iOS/ios24/)

ARC란 Automatic Reference Counting의 준말로써, 자동 참조 카운트라는 의미입니다. 

Swift에서는 **앱의 메모리 사용을 관리하기 위해서 ARC**를 사용하는데, 자동으로 참조 횟수를 관리하기 때문에 ARC가 알아서 더 이상 사용하지 않는 인스턴스를 메모리에서 해지합니다**(Run time에 계속 실행되는 것이 아니라 Complie time(build할 때)에 실행).**

때문에 개발자가 메모리 관리에 신경 쓸 필요가 없다는 장점이 있습니다.

> [참조 횟수(Reference Counting)는 클래스 타입의 인스턴스에만 적용되며, 값 타입인 구조체/열거형 등에는 적용되지 않습니다.](https://jusung.gitbook.io/the-swift-language-guide/language-guide/23-automatic-reference-counting)

WWDC2011에서 처음 등장하였으며, Obj-C는 ARC의 사용 여부를 **선택**할 수 있지만(사용안하면 MRC사용) Swift에서는 **필수**입니다.

ARC는 메모리 관리를 위해서 사용된다고 말했는데, Data, Heap, Stack, Code 이렇게 4가지 가상 메모리 영역 중 **Heap 영역**과 관련되어 있습니다. Heap 영역은 class, closure 등 **참조형 자료(레퍼런스 타입)**들이 머무는 공간이자, **개발자가 동적으로 할당하는 메모리 공간**이기 때문에 관리가 필요하기 때문입니다.

따라서 Retain Count는 Heap영역에 참조형 자료들이 얼마나 참조되고 있는지에 따라 카운팅되고 이에 따라 메모리를 할당 및 제거하게 됩니다. 이러한 일련의 역할을 수동적으로 했던게 MRC이고, 이후 등장한 ARC는 이것들을 자동으로 관리해준다고 생각하시면 될 것 같습니다.

## 더 나아가기

ARC는 사용 가능성이 있는 인스턴스의 메모리를 해제하는 일이 없도록 하기 위해 얼마나 많은 프로퍼티, 상수, 혹은 변수가 그 인스턴스를 참조하고 있는지 추적한다. 그렇기 때문에 ARC에서는 해당 인스턴스에 대한 참조가 최소 하나라도 존재한다면 그 인스턴스를 메모리에서 해지 하지 않는다!

## 생각해보기 

**Run time에 계속 실행되는 것이 아니라 Complie time(build할 때)에 실행된다고 하였는데 그럼 어떻게 동적으로 실행되는 것들의 reference count를 세고 메모리 관리를 할 수 있을까?**

Answer: [Retain Count가 되는 방식](./Reference-count-in-ARC.md)  


**ARC 와 GC의 차이점?**

Answer: [타 언어의 GC와 Swift ARC의 차이](