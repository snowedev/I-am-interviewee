# Notification 동작방식 및 활용방안

## 도움을 준 좋은 블로그
* [이동건의 이유있는 코드-NotificationCenter](https://baked-corn.tistory.com/42)


## Answer
<img width=50% src="https://t1.daumcdn.net/cfile/tistory/99E985335A12E50F1F">  


* 동작방식  
"특정 객체가 NotificationCenter에 등록된 Event를 발생시키면, NotificationCennter에 등록된 Observer들 중 해당 Event를 담당 중인 Observer가 그 Event에 대한 행동을 취하는 것(#selector)이 NotificationCenter가 동작하는 방식입니다."  

* 활용방안  
 "동작방식에서 알 수 있듯이 특정 Event의 실행을 감지할 수 있기 때문에, 특정 Event의 실행에 따라 동작해야하는 것 또는 동시적으로 여러 View에서 동작해야하는 것 등을 처리할 때에 활용할 수 있습니다."


---

## 연습  
> POST라는 버튼을 누르면 파란색 박스는 주황색으로, 차콜색은 초록색으로 변하는 예제입니다.

<img width="1198" alt="image" src="https://user-images.githubusercontent.com/42789819/111866311-ed647d00-89af-11eb-9708-dd14d02bf23e.png">

### POST

<img width="724" alt="image" src="https://user-images.githubusercontent.com/42789819/111866373-619f2080-89b0-11eb-8679-ed38fd068f3a.png">

* POST라는 버튼을 누르면 NotificationCenter에 PostButton이라는 Event 등록하고 이를 발생시킨다 라는 의미입니다.
* > object:매개변수를 통해 Event를 발생시킬 때 특정 객체를 같이 넘길 수 있습니다. 현재는 아무것도 넘기지 않을 것이기 때문에 nil로 설정하였습니다.

### Observer

<img width="734" alt="image" src="https://user-images.githubusercontent.com/42789819/111866436-bd69a980-89b0-11eb-9809-c138665a030d.png">

<img width="737" alt="image" src="https://user-images.githubusercontent.com/42789819/111866441-cb1f2f00-89b0-11eb-85d8-cfb9caaf1b99.png">

* 위의 두 뷰에서는 PostButton이라는 Event가 발생할 때 이를 캐치하고 어떤 행동을 취하는 Observer를 Add해줍니다.

* Observer가 취하는 행동은 #selector에 지정해준 changeSecondColor,changeThirdColor입니다.
    * changeSecondColor는 박스의 색을 orange로 변경해줍니다.
    * changeThirdColor는 박스의 색을 green으로 변경해줍니다.


### Result
<img width=30% src=https://user-images.githubusercontent.com/42789819/111866787-4c77c100-89b3-11eb-8af4-ddc50f6f64df.gif>