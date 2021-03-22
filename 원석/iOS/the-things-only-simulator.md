# 실제 디바이스가 없을 경우 개발 환경에서 할 수 있는 것과 없는 것을 설명하시오.


## 참고하면 좋은 사이트
> 이 문서를 참조하면 시뮬레이터 환경에서 할 수 없는 것들을 자세히 알 수 있다.
[TestingontheiOSSimulator](https://developer.apple.com/library/archive/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator/TestingontheiOSSimulator.html) 


## Answer

위 공식 문서를 요약하자면 다음과 같다.    


시뮬레이터는 Mac 기반에서 돌아가기 때문에 컴퓨터의 자원을 활용한다.  이 때, 컴퓨터의 자원이라 함은 CPU, memory 그리고 network connection 등이 될 수 있다. 그렇기 때문에 시뮬레이터 상에서 테스트되는 앱들은 실제 디바이스에서 구동 될 때 보다 빠르고, 부드럽게 동작할 수 있으며, 시뮬레이터에서 테스트 했을 때 보여주는 퍼포먼스 또한 정확한 앱의 성능을 나타내지 않는다는 것을 유의해야 한다.

대부분은 시뮬레이터에서도 테스트가 가능하지만 하드웨어적인 변화가 필요한 요소들 및 몇몇 API와 Framework등은 시뮬레이터에서 테스트가 불가하다.  


그 목록은 아래와 같다.

### Hardware Differences
> 대부분은 테스트가 가능하지만 하드웨어적인 요소가 필요한 것들은 테스트 불가

* Motion support (accelerometer and gyroscope) are unsupported.
* Audio and video input (camera and microphone) are unsupported.
* Proximity sensor(근접센서)
* Barometer(기압계)
* Ambient light sensor(주변광 센서)


### API Differences
> Simulator APIs don’t have all the features that are available on a device.  
> For example, the APIs don’t offer:

* Receiving and sending Apple push notifications
* Privacy alerts for access to Photos, Contacts, Calendar, and Reminders
* The UIBackgroundModes key
* Handoff support

> In addition, Simulator doesn’t support the following frameworks:

* External Accessory
* IOSurface
* Media Player
* Message UI
* In UIKit, the UIVideoEditorController class


### + 
* Simulator does not include backward compatibility with all versions of iOS and watchOS(시뮬레이터는 모든 iOS, watchOS의 버전들의 하위호환을 포함하지 않는다).