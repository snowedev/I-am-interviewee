# TableView의 동작 방식과 화면에 Cell을 출력하기 위해 최소한 구현해야 하는 DataSource 메서드를 설명하시오


## 참고한 좋은 글
* [iOS) 테이블뷰 (UITableView) 기초 - (2)](https://woonhyeong.tistory.com/6)


## 답변

* ### **TableView 동작 방식**
    TableView는 "프로토타입 셀"을 통해 셀들을 구성하게 된다. 따라서 테이블 뷰를 구현하기 위해서는 어떠한 셀들을 사용하고 어떻게 배치 할 것인지 구현해주어야 하는데 이를 위해 TableView의 DataSource와 Delegate를 채택하고, 구현한 객체에게 TableView가 이러한 권한을 위임해주어야 한다.
    > 프로토타입 셀이란 개발자가 임의로 지정하여 테이블뷰 안에서 사용할 셀들의 설계도


<br>

* ### **동작 순서**

    cell 생성시

    * `awakeFromNib()` 로 셀을 만들어준다.
        * `tableView(_:willDisplay:forRowAt:)` 이 만들어지는 셀마다 실행된다.
    
    * 화면에 display되는 셀이 만들어진 후, `tableView(_:prefetchRowsAt:indexPath:)` 가 호출된다.
        * prefetch는 화면상에는 안보이지만 앞으로 보여질 셀들을 미리 준비해주는 역할을한다.
    
    ---
    
    스크롤 시

    * 스크롤이 일어나며 보여지는 셀에 `cellForRowAt`, `willDisplay`가 발생하고 `prefetchRowsAt`이 작동하면서 prefetch queue에 있는 셀들을 조회한다.

    * 그 후에 `tableView(_:didEndDisplaying:forRowAt:)`가 호출되면서 display되던 셀이 사라진다.
    
    * 이후부터는 셀이 만들어질 때 `awakeFromNib()`이 아닌 `prepareForReuse()`가 호출
        * prepareForReuse()에서 셀이 재사용될 때 셀을 초기화해주는 작업을 해주곤 한다.


 <br>   

* ### **최소한 필요한 필수 메서드**
    ```swift
    @required 
    // 특정 위치에 표시할 셀을 요청하는 메서드
    func tableView(UITableView, cellForRowAt: IndexPath) 
    
    // 각 섹션에 표시할 행의 개수를 묻는 메서드
    func tableView(UITableView, numberOfRowsInSection: Int)
    ```


<br>

## 더 나아가기

### 필수구현을 제외한 TableView 메서드

* UITableViewDelegate Methods - `MVC중 Controller부분`
    ```swift
    // 지정된 행이 선택되었음을 알리는 메서드
    func tableView(UITableView, didSelectRowAt: IndexPath)

    // 지정된 행의 선택이 해제되었음을 알리는 메서드
    func tableView(UITableView, didDeselectRowAt: IndexPath)

    // 특정 위치 행의 높이를 묻는 메서드
    func tableView(UITableView, heightForRowAt: IndexPath)

    // 특정 위치 행의 들여쓰기 수준을 묻는 메서드
    func tableView(UITableView, indentationLevelForRowAt: IndexPath)

    // 특정 섹션의 헤더뷰 또는 푸터뷰를 요청하는 메서드
    func tableView(UITableView, viewForHeaderInSection: Int)
    func tableView(UITableView, viewForFooterInSection: Int)

    // 특정 섹션의 헤더뷰 또는 푸터뷰의 높이를 물어보는 메서드
    func tableView(UITableView, heightForHeaderInSection: Int)
    func tableView(UITableView, heightForFooterInSection: Int)

    // 테이블뷰가 편집모드에 들어갔음을 알리는 메서드
    func tableView(UITableView, willBeginEditingRowAt: IndexPath)

    // 테이블뷰가 편집모드에서 빠져나왔음을 알리는 메서드
    func tableView(UITableView, didEndEditingRowAt: IndexPath?)
    ```
* UITableViewDataSource - `MVC중 Model부분`    
    ```swift 
    @optional
    // 테이블뷰의 총 섹션 개수를 묻는 메서드
    func numberOfSections(in: UITableView)
    
    // 특정 섹션의 헤더 혹은 푸터 타이틀을 묻는 메서드
    func tableView(UITableView, titleForHeaderInSection: Int)
    func tableView(UITableView, titleForFooterInSection: Int)
    
    // 특정 위치의 행을 삭제 또는 추가 요청하는 메서드
    func tableView(UITableView, commit: UITableViewCellEditingStyle, forRowAt: IndexPath)
    
    // 특정 위치의 행이 편집 가능한지 묻는 메서드
    func tableView(UITableView, canEditRowAt: IndexPath)

    // 특정 위치의 행을 재정렬 할 수 있는지 묻는 메서드
    func tableView(UITableView, canMoveRowAt: IndexPath)
    
    // 특정 위치의 행을 다른 위치로 옮기는 메서드
    func tableView(UITableView, moveRowAt: IndexPath, to: IndexPath)
    ```