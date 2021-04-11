# ARC 대신 Manual Reference Count 방식으로 구현할 때 꼭 사용해야 하는 메서드들을 쓰고 역할을 설명하시오.


## 참고한 사이트
* [메모리관리-MRC/MRR](https://babbab2.tistory.com/28)
* [retain-apple.developer](https://developer.apple.com/documentation/objectivec/1418956-nsobject/1571946-retain)
* [release-apple.developer](https://developer.apple.com/documentation/objectivec/1418956-nsobject/1571957-release)
* [ARC뿌시기-naljin](https://sujinnaljin.medium.com/ios-arc-뿌시기-9b3e5dc23814)


## 사전지식
* ARC: **Automatic** Reference Counting
* MRC: **Manual** Reference Counting


## Anwser

MRC(Manual Reference Counting)를 사용할 때에는 retain, release 메서드를 꼭 사용해주어야 한다.
> MRC를 retain/release 개념으로 생각해서 MRR(Memory Retain Release)라고도 부름

* retain: retain count(=reference count)증가를 통해 현재 Scope에서 객체가 유지되는 것을 보장
    * ```obj-c
      [test retain];
      ```
    * 인스턴스 생성 시에는 자동으로 count +1이 되지만 그 이후부터 인스턴스의 주소값을 할당받은 경우에는 retain메서드를 통해 수동적으로 RC를 올려주는 작업을 해주어야 함

* release: retain count(=reference count)를 감소시킴. retain 후에 필요 없을 때 release함
    * ```obj-c
      [test2 release];
      ```
    * ARC에서 했던것처럼 test2를 더이상 사용하지 않아서 nil을 대입하더라도 자동으로 RC가 0으로 해지되지 않음. release 메서드를 호출해야 RC값을 감소시킬 수 있음


## 더 깊게 알아보기


### `retain`

```obj-c
// test라는 인스턴스 생성
TestClass *test [[TestClass alloc] init];

/* 
test.retainCount == 1 
*/
```
새로 인스턴스를 생성할 경우 RC가 자동으로 +1

<img width=50% src="https://user-images.githubusercontent.com/42789819/114310064-63c64c00-9b24-11eb-94c8-053b01d2b4a1.png">



```obj-c
TestClass *test [[TestClass alloc] init];
TestClass *test2 = test;

// test.retainCount == 1 
```
수동으로 retain을 해주지 않았기 때문에 아직 RC가 증가되지 않는다.


```obj-c
TestClass *test [[TestClass alloc] init];
TestClass *test2 = [test retain];
 
//test.retainCount == 2
```
이렇게 retain 을 수동으로 해주어야 retain count가 증가한다.

<img width=50% src="https://user-images.githubusercontent.com/42789819/114310065-65900f80-9b24-11eb-8a41-f698f87aff55.png">



### `release`

test2를 더이상 사용하지 않을거야 ARC처럼 nil을 대입해볼까?

```obj-c
TestClass *test [[TestClass alloc] init];
TestClass *test2 = [test retain];
test2 = nil;
 
//test.retainCount == 2
```
안된다. 수동으로 release를 해주지 않으면 RC는 내려가지 않는다.

```obj-c
TestClass *test [[TestClass alloc] init];
TestClass *test2 = [test retain];
[test2 release]; // test.retainCount == 1
test2 = nil; // test2.retainCount == 0
```
[test2 release];를 해줌으로써 test의 RC는 1감소하고 참조하고 있는 인스턴스가 없는 test2는 그제서야 nil을 대입하여 0임을 명시해 줄 수 있다.
