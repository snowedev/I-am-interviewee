# hugging, resistance에 대해서 설명하시오.

## 참고한 좋은 글
* [[Swift] Hugging, Resistance](https://nsios.tistory.com/98)
* [[AutoLayout] Hugging priority와 Compression Resistance priority 비교](https://eunjin3786.tistory.com/43)

## Answer

오토레이아웃에서 사용되는 값으로서, Content Priority 속성에 `hugging`, `resistance` 두 종류가 있다. 얘네는 Priority를 설정해 줄 수 있는데 기본으로 제공해주는 `IntrinsicContentSize`을 이용해서 오토레이아웃을 잡고 싶을 경우 사용할 수 있다.


<img width="251" alt="image" src="https://user-images.githubusercontent.com/42789819/131532779-3c322746-80ff-44e9-9037-7e91792bcfc0.png">

> Size Inspector 하단부에 보면 이렇게 설정할 수 있게 되어있다.



* ### intrinsicContentSize?

    "컨텐츠 고유 사이즈"이다. UILabel, UIButton, UIImageView등의 객체를 만들 때, 직접 사이즈를 적용하지 않아도 저마다의 크기가 지정되어 나오는 것이 바로 이 프로퍼티 때문이다. Label은 text에 맞게 이 값이 자동으로 조절되고 ImageView도 마찬가지이다.       
    그러나 순수 UIView는 넣고 오토 레이아웃을 중앙으로만 잡으면 오류가 발생한다. 콘테이너 뷰 역할을 하는 뷰들은 고유 사이즈가 없다. UILabel, UIButton, UIImageView와 달리 고유 사이즈를 모르기 때문에 오류가 발생하게 된다.

* ### intrinsicContentSize와 frame의 차이점?

    * `intrinsicContentSize`는 get-only 프로퍼티라 수정이안되고 `frame`은 수정이가능하다.

    * `frame`영역을 수정했다고해서 `intrinsicContentSize`의 값이 달라지지않는다. 서로 의존하고 있지않기때문에 `intrinsicContentSize`를 통해 뷰의 본래 사이즈를 알 수 있다.


<br>

* ### Content Hugging?

    `허깅 = 감싸다 = 안으로 줄어들다`, 상대적으로 작아지려고 하는 세기를 의미한다.  
    컨텐츠 고유 사이즈(`IntrinsicContentSize`) 보다 뷰가 커지지 않도록 제한하는 우선도이다. 다른 제약사항보다 우선도가 높으면 뷰가 컨텐츠 사이즈보다 커지지 않는다. 
    > 남은 여백을 둘 중 어떤 Label로 채울거니? 크기가 늘어나게하지 않을 Label의 우선순위를 더 높게 지정하렴  
    > 우선순위가 높으면 크기 유지, 우선순위가 낮으면 크기가 늘어남

    <br>

    `["LEFT" label]["RIGHT" label]` 을 leading, trailing 0을 주면 아래와 같아진다.   
    그리고 오류가 horizontal hugging을 우선순위를 조정하라는 오류가 난다.  
    <img width="70%" alt="image" src="https://user-images.githubusercontent.com/42789819/131533799-4c9b96ed-71da-4e1a-9ae6-226d278ff162.png">

    여기서 `hugging priority`가 `파란색` > `주황색`이 되면 둘 중 더 낮은 주황색이 나머지 공간을 채우게 된다.

    <img width="70%" alt="image" src="https://user-images.githubusercontent.com/42789819/131535968-c5e1e3c3-6877-49a2-ba03-475db6641fa4.png">


<br>

* ### Compression Resistance?

    `resistance = 저항하다 = 바깥으로 늘어나다`, 상대적으로 커지려고 하는 세기를 의미한다. 
    컨텐츠 고유 사이즈(`IntrinsicContentSize`)보다 뷰가 작아지지 않도록 제한한다. 다른 제약사항보다 우선도가 높으면 뷰가 컨텐츠사이즈 보다 작아지지 않는다.
    > 공간이 좁은데 누가 줄어들래? 크기가 줄어들게 않을 Label의 우선순위를 더 높게 지정하렴
    > 우선순위가 높으면 크기 유지, 우선순위가 낮으면 크기가 작아짐

    <br>

    마찬가지로 아까처럼 오토를 잡고 이번엔 라벨의 너비가(내용이) 화면을 넘어가도록 내용을 많이 작성하였다. 
    그랬더니 이번엔 horizontaol compression 우선순위를 조정하라는 오류가 난다.
    <img width="100%" alt="image" src="https://user-images.githubusercontent.com/42789819/131536404-a07be47c-456e-4583-8c22-34abc667cddb.png">

    여기서 `compression priority`가 `파란색` > `주황색`이 되면 둘 중 더 낮은 주황색이 파란색이 필요한 공간을 양보하며 줄어들게된다.

    <img width="70%" alt="image" src="https://user-images.githubusercontent.com/42789819/131538181-ccaa5cf8-40bc-4159-8ec2-c6bbb7ee5e5a.png">