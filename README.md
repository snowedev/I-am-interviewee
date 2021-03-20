# iOS 기술 면접을 준비하는 레포
> [👨🏻‍💻👩🏻‍💻iOS 면접에 나올 질문들 총 정리](https://github.com/JeaSungLEE/iOSInterviewquestions)를 참고하여 공부하는 Repository입니다.  


<br>

## 참여자
| Interviewee | Answer | Github |
|:----------|:-----:|:-----:|
|<img width=100px src=https://user-images.githubusercontent.com/42789819/111863006-285cb580-899c-11eb-8977-3c251851fdca.png> | [이동](./원석)| [원석's Github](www.github.com/snowedev) |
|<img width=100px src=https://user-images.githubusercontent.com/42789819/111863005-2692f200-899c-11eb-893d-bf7a4d30024b.jpeg> | [이동](./수정)| [수정's Github](www.github.com/suzumsz) |


<br>

## Branch 관리
- main 브랜치

 메인(main): 메인 브랜치

<br>

```
- Main
   ├── wonseok(각 Local Branch)
   └── sujeong
```

<br>

**브랜치 만들기**

- 브랜치 만듦

```bash
git branch 기능(or 뷰)이름
```

- 원격 저장소에 로컬 브랜치 push

```bash
git push --set-upstream origin 브랜치이름(뷰이름)
```
```bash
git push -u origin 브랜치이름(뷰이름)
```


**브랜치 사용하기**
- 브랜치 전환

```bash
git checkout 뷰이름
```

- 코드 변경 (현재 **참여자 이름** 브랜치)

```bash
git add .
git commit -m "커밋 메세지" origin 뷰이름
```

- 푸시 (현재 **참여자 이름** 브랜치)

```bash
git push origin 뷰이름 브랜치
```


**Merge**
- 현재 브랜치에서 할 일 다 했으면 **main** 브랜치로 전환

```bash
git checkout main
```

> main전환 후, pull받을게 있다면 pull받기
> - main pull: git pull or git pull origin main
> - main push: git push or git push origin main

- 머지 (현재 **main** 브랜치)

```bash
git merge 참여자브랜치
```