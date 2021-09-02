# Intrinsic Size에 대해서 설명하시오.	


## 같이 보면 좋은 답변
* [hugging, resistance에 대해서 설명하시오.](./hugging,resistance.md)

## 참고한 좋은 글
* [intrinsicContentSize](https://developer.apple.com/documentation/uikit/uiview/1622600-intrinsiccontentsize)

## Answer


* ### intrinsicContentSize?
    "컨텐츠 고유 사이즈"이다. UILabel, UIButton, UIImageView등의 객체를 만들 때, 직접 사이즈를 적용하지 않아도 저마다의 크기가 지정되어 나오는 것이 바로 이 프로퍼티 때문이다. Label은 text에 맞게 이 값이 자동으로 조절되고 ImageView도 마찬가지이다.       
    그러나 순수 UIView는 넣고 오토 레이아웃을 중앙으로만 잡으면 오류가 발생한다. 콘테이너 뷰 역할을 하는 뷰들은 고유 사이즈가 없다. UILabel, UIButton, UIImageView와 달리 고유 사이즈를 모르기 때문에 오류가 발생하게 된다.

    * `UIView` : intrinsicContentSize 없음
    * `Slider` : width 만 intrinsicContentSize 가짐
    * `Label`, `Button`, `Switch`, `TextField` : height, width intrinsicContentSize 모두 가짐
    * TextView, ImageView : Content 에 따라 값이 달라짐


<br>

* ### intrinsicContentSize와 frame의 차이점?

    * `intrinsicContentSize`는 get-only 프로퍼티라 수정이안되고 `frame`은 수정이가능하다.

    * `frame`영역을 수정했다고해서 `intrinsicContentSize`의 값이 달라지지않는다. 서로 의존하고 있지않기때문에 `intrinsicContentSize`를 통해 뷰의 본래 사이즈를 알 수 있다.