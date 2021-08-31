# 오토레이아웃을 코드로 작성하는 방법은 무엇인가? (3가지)

## 알아야할 것
스토리보드로 오토를 잡을때는 translatesAutoresizingMaskIntoConstraints 속성이 자동으로 false로 반영된다. 
하지만 코드로 짤때는 그렇지 않기 때문에 직접 translatesAutoresizingMaskIntoConstraints = false를 해 주어야 한다(SnapKit사용시에는 자동으로 되는걸로 알고있음).


## Answer

1. `NSLayoutConstaint`
    
    속성값들이 좀 길지고 직관적인편이나 코드가 길어지게 되면 가독성이 떨어지는 편이다.
    
    ```swift
    NSLayoutConstraint(item: alertView,
                attribute: .leading,
                relatedBy: .equal,
                toItem: messageLabel,
                attribute: .leading,
                multiplier: 1.0,
                constant: 8).isActive = true
    ```

<br>

2. `NSLayoutAnchor` - 제일 많이 쓰이는 것 같다.  
    > 사용성을 위해 SnapKit 라이브러리를 함께 활용하는편이다.

    원래는 `NSLayoutConstaint`를 사용하여 오토레이아웃을 작성했다. 그러나 앞서 기술했듯 가독성 측면에서 단점이 있었기때문에 새롭게 Apple에서 내준 방식이다. 코드가 마찬가지로 길어지긴하나 그래도 보다 가독성이 좋아졌다.

    ```swift
    NSLayoutConstraint.activate([
        button.centerXAnchor.constraint(equalTo: view.centerXAnchor),
        ...
        ])
    ```

<br>

3. `Visual Format Language(VFL)`

    이건 처음 듣는 건데 간단하게 말하면 기호 및 문자열로 레이아웃 관계를 시각적인 표현으로 표시하는 거라고 한다. 뷰는 대괄호로 표시되고 뷰간의 연결은 하이픈(또는 뷰들을 떨어뜨리는 숫자에 의해 두개의 분리된 하이픈)을 사용한다. `NSLayoutConstraint`를 이용해서 생성한다.

    사용이 익숙해지면 편리하다고 하는데 사용을 위해 `VFL`만의문법을 익혀야하기 때문에 협업시에 도입하기에 단점 존재하는 것 같다.

    ```swift
    let views = ["myView": myView]
    let formatString = "|-[myView]-|"
    let constraints = NSLayoutConstraint.constraintsWithVisualFormat(formatString, 
        options: .AlignAllTop, 
        metrics: nil, 
        views: views)

    NSLayoutConstraint.activateConstraints(constraints)
    ```