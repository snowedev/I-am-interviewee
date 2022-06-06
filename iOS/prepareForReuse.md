# prepareForReuse에 대해서 설명하시오.

## Answer

#### prepareForReuse와 cell 재사용 문제

우리가 컬렉션 뷰, 테이블 뷰를 사용 할 때 셀을 재사용하는데, 그때 cell에 표시되어야 하는 내용이 잘못되어 나오거나 버벅이는 듯한 현상을 보이는 문제가 발생한 적이 있을것이다.  
이 현상을 이해하기 위해서는 cell이 어떻게 재사용되는지 알고 있어야 한다. 그리고 그 현상을 해결할 수 있는 시점이 `prepareForReuse` 호출 시점이다. 
`prepareForReuse` 자체의 정의는 간단하다. 말 그대로 재사용에 대한 준비를 하는 역할을 수행한다.  
> 왜 재사용을 하는지에 대해서는 메모리 효율성을 중점으로 생각해보면 좋을것이다.


#### Cell은 어떤 과정으로 재사용 될까

cell을 재사용을 위해 우리는 아래와 같이 tableView, collectionView에서 `dequeueReusableCell(withIdentifier: )`를 사용한다.

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        guard let cell = tableView.dequeueReusableCell(withIdentifier: TestCell.identifier, for: indexPath) as? TestCell else { 
            fatalError("Cell Not Found")
        }

        // ...

        return cell
    }
```
재사용에 대한 과정을 알아보기 위해 우리가 사용하는 `dequeueReusableCell(withIdentifier: )` 내부를 들여다보려한다.

`dequeueReusableCell(withIdentifier: )` 의 이름을 천천히 살펴보면 `dequeue`, `Reuse`, `Cell` 이렇게 세가지로 나눠볼 수 있다.  
테이블 뷰나 컬렉션 뷰 모두 셀을 재사용(Reuse)하는데, 셀들의 재사용 구조는 큐(queue)로 되어있다. 그래서 `dequeue`라는 단어가 등장한다.

<p align="center"><img src="https://user-images.githubusercontent.com/35067611/105666256-37833200-5f1c-11eb-8493-4d5549e7236f.png"></p>  

위 사진은 우리가 알아볼 과정을 시각적으로 나타낸 사진이다. 사진 속 시뮬레이터를 살펴보면 사용자에게 표시되는 셀 들이 있고 스크롤이 되어 화면에서 보이지 않는 셀들이 있다.  
**스크린에 표시되고 있지 않는 cell들은 queue로 들어간다.**(사진에서는 Task 2 셀을 예로 들고있다.) **이렇게 queue로 들어간 cell들을 dequeue하여 reuse되는 과정이 우리가 흔히 사용했던   `cellForRowAt` 메서드에서 `dequeueReusableCell(withIdentifier: )` 을 통해 이루어지는 것이다.** 

위와 같은 과정을 알고나면 왜 cell 재사용 문제가 발생하는지 유추할 수 있을 것이다.

스크린에 표시되고 있지 않은 cell들이 queue로 들어간다고 했다. 따라서 그걸 그대로 dequeue하여 재사용할 경우 흔히들 한번쯤 겪는 재사용 문제가 발생하는 것이다.  
(위 사진에서는 Task 2 cell이 체크된 상태로 queue에 들어갔고, 이를 dequeue하여 재사용을 할 경우 체크된 상태의 cell이 나올 것이다.)  

**그래서 우리는 문제를 해결하기 위해 `prepareForReuse`를 사용한다.**

#### prepareForReuse

<p align="center"><img src="https://user-images.githubusercontent.com/35067611/105666257-381bc880-5f1c-11eb-852f-024dbc043269.png"></p>  

위 사진에 명시된 순서로 cell이 재사용되는데 `cellForRowAt`이 호출 되기 전 `prepareForReuse`가 호출된다. 따라서 우리는 prepareForReuse내부에 cell이 재사용되기 위한 초기 설정을 코딩 해준다.

```swift
override func prepareForReuse() {
    super.prepareForReuse()
    // ...
}
```

한 가지 주의해야 할 점은 어떤 상태를 유지해야하는 cell이 있는 경우이다.  

만약 cell자체에 인터랙션이 없고 그냥 표시되는 용도라면 문제가 없을 것이다. 하지만 예시처럼 cell을 터치했을 때 체크표시가 되는 경우, prepareForReuse에서 label의 text와 체크표시에 대한 초기화를 시켜준다면 이미 터치되어 체크표시가 되어있어야하는 cell은 스크롤로 인해 화면에서 사라졌다가 다시 표시되는 경우 체크표시가 사라져있을 것이다. 따라서 우리는 위와 같은 부분을 주의하면서 코드를 작성해야한다. 언급한 예시에서는 prepareForReuse 이후 호출되는 cellForRowAt에서 데이터소스를 체크하여 해결할 수 있을 것이다.







