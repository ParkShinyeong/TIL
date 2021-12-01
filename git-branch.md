## 브랜치

독립적으로 어떤 작업을 진행하기 위한 개념

- 한 소스코드에서 **동시에 다양한 작업**을 할 수 있게 해준다.
- 소스코드의 **한 시점과 동일한 상태**를 만들고 브랜치를 넘나들며 작업을 수행할 수 있다.
- 각각의 브랜치에서 생긴 변화가 다른 브랜치에 **영향을 주지 않고, 독립적**으로 코딩을 진행할 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d8f11cbe-fcb0-4c9f-a52a-8911ec65e9a8/Untitled.png)

(이미지 출처: Git Beginner's Guide for Dummies)

이렇게 **다양한 브랜치**를 만들고 작업을 할 수 있다.

- **feature**: 기능을 추가할 때마다 따는 브랜치
- **dev, develope** : 베타 버전, 모든 개발 로그들이 쌓이는 곳, 새로운 기능들이 완성이 되고나서 머지되는 곳
- **release** : 한번 배포 시도해보는 곳
- **hotfix**: 급한 수정
- **master, main** : 항상 최신의 안정적인 프로그램, 안전함을 보장받은 곳

### 브랜치 종류

1. **통합 브랜치 (Integration Branch)**

- 배포될 소스 코드가 기록되는 브랜치
- Git repository를 생성하면 기본적으로 main 브랜치가 생긴다. ( 혹은 master)
- 해당 프로젝트의 모든 기능이 정상적으로 작동하는 상태의 소스 코드가 담겨 있다.

1. **피처 브랜치(Feature Branch) 혹은 토픽 브랜치**

- 기능 추가, 버그 수정과 같이 단위 작업을 위한 브랜치
- 통합 브랜치로부터 만들어내며, 피처 브랜치에서 하나의 작업이 완료되면 다시 통합 브랜치에서 병합하는 방식으로 진행된다.

## 브랜치 명령어 모음

**새로운 브랜치 생성**

```jsx
$ git branch 새_브랜치_이름
```

**브랜치 전환**

```jsx
$ git checkout 브랜치 이름
$ git switch 브랜치 이름
```

**새 브랜치 생성 후 그 브랜치로 전환**

```jsx
$ git checkout -b 새_브랜치_이름
$ git switch -c 새_브랜치_이름
```

**브랜치 목록 확인**

```jsx
$ git branch
```

**브랜치 목록과 각 브랜치 최근 커밋 확인**

```jsx
$ git branch -v
```

**브랜치 병합**

main 브랜치에 mywork 브랜치를 병합할 때. 먼저 main으로 이동한 뒤 병합해준다.

```jsx
$ git checkout main
$ git merge mywork
```

- **fast-forward 방식**
  만약 mywork와 merge 되기 전 main브랜치에 추가적인 커밋이 없으면 브랜치가 분기될 필요가 없다.
  별도의 커밋을 생성하지 않고, main이 가리키는 커밋을 mywork가 생성한 커밋으로 바꾼다.
- **merge commit 방식**
  main에 별도의 커밋이 있었다면, 각 브랜치가 줄기처럼 분기한 후, 병합할 것이다.

**브랜치 삭제**

```jsx
$ git branch -d 삭제할_브랜치_이름
$ git branch -D  //해당 명령어는 병합하지 않은 브랜치를 강제로 삭제한다.
```

**로그에 모든 브랜치를 그래프로 표현**

```jsx
$ git log --branches --graph --decorate
```

**아직 커밋하지 않은 작업을 스택에 임시 저장**

```jsx
$ git stash
```

❓ **merge vs rebase**

`git rebase main mywork`

rebase의 원리 ⇒ fast-forward 와 같다.

- **merge:** 변경 내용의 이력이 모두 남아있어 이력이 복잡해진다.
- **rebase**: branch base를 이동시킨다는 뜻으로, 브랜치 통합을 목적으로 하지만, 특정 시점으로 브랜치가 가리키는 곳을 변경하는 기능을 한다.
  - rebase는 커밋 이력을 모두 보존하지 않는다. 따라서 되도록 쓰지 않는 것이 좋다.

### 그 외

```jsx
rebase; // 커밋의 베이스를 다시 정하고 싶은 경우
squash; // 여러 개의 커밋 로그를 하나로 묶고싶은 경우
revert; // 커밋 여러 개의 변경 사항을 취소하고 싶은 경우
--amend; // 최근 커밋 메세지를 수정하고 싶은 경우
```

[더 알아보면 좋을 명령어!](https://dangitgit.com/ko)

[https://www.notion.so/codestates/Git-675908feaa8a4eacbe7977d87aa17ab9#b86caa14dc68459a89def8e8b8cdb636](https://www.notion.so/675908feaa8a4eacbe7977d87aa17ab9)

[https://www.notion.so/codestates/Git-Merge-9312bb14511e4643b0729ac8546d2ee3](https://www.notion.so/Git-Merge-9312bb14511e4643b0729ac8546d2ee3)

[https://backlog.com/git-tutorial/kr/stepup/stepup7_4.html](https://backlog.com/git-tutorial/kr/stepup/stepup7_4.html)
