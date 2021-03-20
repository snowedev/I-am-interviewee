# iOS 기술 면접을 준비하는 레포
> [👨🏻‍💻👩🏻‍💻iOS 면접에 나올 질문들 총 정리](https://github.com/JeaSungLEE/iOSInterviewquestions)를 참고하여 공부하는 Repository입니다.  


<br>

## 참여자
| Interviewee | Answer | Github |
|:----------|:-----:|:-----:|
|<img width=100px src=https://user-images.githubusercontent.com/42789819/111863006-285cb580-899c-11eb-8977-3c251851fdca.png> | [제 답변은 다음과 같습니다](./원석)| [원석's Github](www.github.com/snowedev) |
|<img width=100px src=https://user-images.githubusercontent.com/42789819/111863005-2692f200-899c-11eb-893d-bf7a4d30024b.jpeg> | [제 답변은 다음과 같습니다](./수정)| [수정's Github](www.github.com/suzumsz) |


<br>

## Branch 관리

```
- Main
   ├── wonseok(각 Local Branch)
   └── sujeong
```


<br>

### **브랜치 만들기**

- 브랜치 만듦

```bash
git branch 참여자 이름
```

- 원격 저장소에 로컬 브랜치 push

```bash
git push --set-upstream origin 브랜치이름(참여자 이름)
```
```bash
git push -u origin 브랜치이름(참여자 이름)
```


<br>

### **브랜치 사용하기**
- 브랜치 전환

```bash
git checkout 본인브랜치
```

- 코드 변경 (현재 **참여자** 브랜치)

```bash
git add .
git commit -m "커밋 메세지" origin 본인브랜치
```

- 푸시 (현재 **참여자** 브랜치)

```bash
git push origin 본인브랜치
```


<br>

### **Merge**
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

- 충돌이 안났다면 push (현재 **main** 브랜치)

```bash
git push
```

- 다시 본인의 답변 작성을 위해 checkout

```bash
git checkout 본인브랜치
```
