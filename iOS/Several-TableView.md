# 하나의 View Controller 코드에서 여러 TableView Controller 역할을 해야 할 경우 어떻게 구분해서 구현해야 하는지 설명하시오.

## 답변

### `방법 1`
### UITableViewDelegate, UITableViewDataSource내 메소드에서 if문 또는 switch문 이용
> ==는 값만을 비교, ===는 두 인스턴스의 포인터의 참조값을 비교하기 때문에 더 정확하다

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    if tableView === firstTV{
        //...
    } else if tableView === secondTV {
        //...
    }
}
```
```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    switch tableView {
      case firstTV:
      	//...
      case secondTV:
      	//...
      default:
      	//...
    }
}
```


<br>

### `방법 2`
### UITableViewDelegate, UITableViewDataSource를 채택한 TableView 클래스를 각각 생성하여 적용
> 해당 테이블 뷰와 컬렉션뷰의 델리게이트와 데이터소스를 해당 뷰컨트롤러가 아닌 해당 객체에 위임하여 사용할 수 있다.

```swift
class ViewController: UIViewController {
    var firstTV: UITableView!
    var secondTV: UITableView!
    let firstDataSource = FirstTableController()
    let secondDataSource = SecondTableController()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // ...
        firstTV.dataSource = FirstTableView()
        firstTV.delegate = FirstTableView()
        secondTV.dataSource = SecondTableView()
        secondTV.delegate = SecondTableView()
    }
        // ...
}

class FirstTableView: NSObject, UITableViewDelegate, UITableViewDataSource {
    
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 10
    }
 
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = UITableViewCell(style: .default, reuseIdentifier: nil)
        // ...
        return cell
    }
}

class SecondTableView: NSObject, UITableViewDelegate, UITableViewDataSource {
        // ...
    }
}
```