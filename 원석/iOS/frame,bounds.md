# Bounds와 Frame의 차이점


## 필요한 사전 지식
* [CGPoint, CGSize, CGRect](https://zeddios.tistory.com/201)

> CGRect는 "사각형"으로 그려지며, origin과 size를 가진다. 즉, [x좌표, y좌표, width(너비), height(높이)]를 가진다.    


## 도움을 준 좋은 블로그
* [ZeddiOS-Frame과 Bounds의 차이(1/2)](https://zeddios.tistory.com/203)
* [ZeddiOS-Frame과 Bounds의 차이(2/2)](https://zeddios.tistory.com/231)


## Answer


```swift
//MARK: -frame
//SupeView(상위뷰)의 좌표시스템 안에서 View의 위치와 크기를 나타냄
open var frame: CGRect

//MARK: -bounds
//View의 위치와 크기를 자신만의 좌표시스템 안에서 나타냄
open var bounds: CGRect
```

### frame
**SupeView(상위뷰)의 좌표시스템** 안에서 View의 위치와 크기를 나타냄

<img width="654" alt="image" src="https://user-images.githubusercontent.com/42789819/111126763-6762c380-85b6-11eb-9078-285704d7e888.png">

log를 보면 하늘색 sub view의 frame은 (x:30, y:191) 이고, 보라색 sub sub view의 frame은 (x: 70, y: 101)임을 알 수 있다.  

이 수치는 앞서 말한 frame의 정의이기도 한 `SuperView를 기준으로 자신의 위치와 크기를 나타냄` 때문이다. 따라서 다음과 같이 생각해야 한다.  


<img width="200" alt="image" src="https://user-images.githubusercontent.com/42789819/111127385-20290280-85b7-11eb-8249-5d0062ae798b.png">

* 하늘색의 SuperView는 노란색
    * 노란색의 origin을 기준으로 얼마만큼 떨어진 곳에 위치해 있는지 표시 
* 보라색의 SuperView는 하늘색
    * 하늘색의 origin을 기준으로 얼마만큼 떨어진 곳에 위치해 있는지 표시



## bounds
View의 위치와 크기를 **자신만의 좌표시스템** 안에서 나타냄

<img width="654" alt="image" src="https://user-images.githubusercontent.com/42789819/111127822-962d6980-85b7-11eb-9c4f-4ca4a977d0cf.png">

상위뷰와 아무런 상관이 없으며, 오직 자신이 기준.  


Bounds는 자신만의 좌표계 (0,0)를 기준으로 위치 (x, y) 및 크기 (너비, 높이)로 표현되는 사각형이다.  
그래서 Bounds의 origin은 default로 (0,0).  


무슨 소리인지 알기 위해!

<img width="400" alt="image" src="https://user-images.githubusercontent.com/42789819/111129867-ec9ba780-85b9-11eb-902e-83c56c5c6351.png">


왼쪽의 화면에서 하늘색의 bounds를 

```swift
subView.bounds.origin.x = 50
subView.bounds.origin.y = 60
```
이렇게 주었더니 우측과 같은 결과가 나온다. 분명 하늘색의 위치를 조정해주었는데, 그것도 우측 아래로 이동시켰는데 왜 보라색이, 그것도 왼쪽 위로 움직였을까?  


Bounds는 상위뷰 안에서의 좌표가 아닌 **"자신만의 좌표시스템"** 을 가진다고 하였다. 따라서 Bounds를 변경하는 것은 해당 위치에서 View를 다시그리라는 의미가 된다.  


Bounds는 상위뷰와 아무런 관련이 없으므로, subView는 움직이지 않는 것 처럼 보이고 그 안에있던 sub-sub-View가 움직이는 것 처럼 보이는 것이다!


Bounds는 좀 더 자세한 설명이 있어야 이해가 빠르기 때문에 이 글을 참조하면 더 좋을 것 같다.


본 글의 출처: [ZeddiOS-Frame과 Bounds의 차이](https://zeddios.tistory.com/203)