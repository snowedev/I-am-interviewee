# App Bundle의 구조와 역할에 대해 설명하시오.

## 참고한 좋은 글
* [iOS 앱 번들 구조와 샌드박스 방식](https://sihyungyou.github.io/iOS-app-bundle/)
* [Package, Bundle, NSBundle](https://nshipster.co.kr/bundles-and-packages/)


## Answer

`Application Bundle`: 개발자가 생성하는 가장 일반적인 유형의 번들


<br>

### 번들이 뭘까?
### Bundle 과 Package

* **Package - Finder가 사용자에게 단일 파일로 보여주는 디렉토리**  

    `Package`는 macOS에서 디렉토리를 추상화하는 기본적인 방법 중 하나이다. `Package`는 디렉토리이지만, `Finder`에서는 이를 단일한 파일로 인식한다. 그래서 일반적인 디렉토리를 `Finder`에서 더블 클릭하여 열면 디렉토리 안으로 이동하지만, `Package`는 파일이 실행된다.

    <img width=50% alt="image" src="https://user-images.githubusercontent.com/42789819/131257318-59644b8e-d6f6-4281-9c12-96b022e87022.png">
    
    `Finder`에서 응용 프로그램 탭으로 이동한 후 아무 앱 아이콘을 오른쪽 클릭으로 확인하면 놀랍게도 패키지 내용 보기 옵션이 존재한다. 이는 `App`도 `Package`의 종류 중 하나임을 의미한다. MacOS에서는 하나의 Application을 `.app`이라는 확장자가 붙는 `Package`로 취급한다. 시스템(`Finder`)은 `.app`, `.bundle`, `.framework`, `.plugin` 와 같은 확장자를 가진 디렉토리를 `Package`로 인식한다.

<br>

* **Bundle - 실행 가능한 코드와 코드에 의해 사용되는 리소스를 가진 디렉토리**  

    `Bundle`은 `Package`처럼 디렉토리의 한 종류를 지칭하지만, 실행 가능한 코드와 리소스를 포함하는 디렉토리이다. 많은 `Bundle`이 `Package`이기도 하기 때문에 `Bundle`과 `Package` 개념을 서로 혼용해서 쓰는 경우가 많다. `application` 같은 경우는 `Finder`에서 사용자에게 단일한 파일로 노출되는 `Package`이면서, 실행 코드와 리소스를 포함하여 `Bundle`이기도 한 대표적인 예이다.
    > Bundle은 사용자에게 보여지는 이름과 실제 Bundle이 사용하는 File System의 이름을 따로 관리한다. 예를 들어, Finder에서 이름을 변경하는 것은 Display Name만 변경하는 것이다.


<br>

앞서 말했듯 앱은 패키지이면서 번들이기도 한 대표적인 예이다.   
이를 토대로 App Bundle에 대해 좀 더 자세히 설명하면, App Bundle은 `샌드박스 구조`에서 Bundle Container로 앱의 `실행 코드와 리소스를` 하나의 공간에 그룹화하여 저장한 `디렉토리`를 말한다.

여기서 `샌드박스`란, **보호된 영역 내에서만 프로그램을 동작시키는 것**으로 어린아이를 보호하기 위해 이 샌드박스 내에서만 놀도록 하는 의미에서 유래된 `보안 모델`이다. 
> [참고링크 1번](https://sihyungyou.github.io/iOS-app-bundle/)을 참고하면 더 자세한 설명이 있다.  

만약 샌드박스로 앱을 감싸지 않는다면 앱은 자신을 실행하는 사용자의 모든 권한을 갖고, 사용자가 접근할 수 있는 모든 리소스에 동일하게 접근이 가능하다. 이러한 경우, 앱이나 연결된 프레임워크에 보안적 결함이 있을 때 이를 악용할 수 있으며 사용자가 수행할 수 있는 모든 작업을 수행할 수 있게 되는 문제가 생긴다. 이러한 보안 문제를 해결하기 위해 iOS는 앱이 모두 SandBox화 되어있다.


<br>

앱 내부에 번들은 다음과 같이 선언되어 있다.
번들 내에 접근해서 직접 수정은 불가하고, 안에 리소스들을 사용하는것이 허락된 상태이다.
```swift
open class Bundle : NSObject {
    /* Methods for creating or retrieving bundle instances. */
    open class var main: Bundle { get }
}

// Usage
Bundle.main
```

만약 Xcode 프로젝트에서 어떤 외부 리소스를 추가하려고 하면 아래와 같은 화면이 뜨게 되는데

<img width = 70% src=https://user-images.githubusercontent.com/42789819/131258421-443d47d5-b13a-465b-a5a6-ac2c1e800f36.png>

여기서 확인을 누르면 앱 전역에서 다음과 같은 형태로 리소스에 접근할 수 있다. 이미지의 경우 아래와 같이 사용할 수 있으며, 그 외 다른 메서드들은 [공식문서](https://developer.apple.com/documentation/foundation/bundle) 하단을 참고하면 된다.

```swift
Bundle.main.url(forResource: "myImageName", withExtension: "jpg")
```
> 이미지 같은 경우에는 Xcode에서 특별하게 취급하여, Bundle 사용을 하지 않아도 바로 이미지 리터럴을 사용하거나 UIImage 형태로 접근할 수 있다.


<br>

앱 번들(메인 번들)에서 사용 가능한 리소스들은 아래와 같다.

* Asset
* 외부에서 추가한 리소스(폰트, 이미지, 사운드 파일 등)
* Storyboard
* plist (InfoPlist 포함 다른것들 - 역시 프로젝트폴더로 집어넣으니까)
* 메인 nib 파일


추가적으로, `Bundle`을 이용하면 소프트웨어를 이전할 때 단순히 `Bundle`만 옮기면 되기 때문에 편리함을 제공한다.