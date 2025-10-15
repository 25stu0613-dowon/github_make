# github 만드는 법
# GitHub 작성법 (초보자용 가이드)

> 이 문서는 깃과 깃허브를 처음 사용하는 사람을 위해 **기본 개념 → 실습 명령 → 파일 템플릿 → 협업 워크플로** 순서로 정리한 마크다운 파일입니다. 복사해서 바로 사용하셔도 되고, 프로젝트 README로 변형하셔도 됩니다.

---

## 목차

1. 개요 및 기본 개념
2. 준비물(설치 및 계정)
3. 기본 git 명령어 실습
4. 깃허브 원격 저장소 연결
5. 브랜치와 협업(풀 리퀘스트 PR)
6. README, LICENSE, .gitignore 작성 예시
7. GitHub Actions(간단한 CI) 예시
8. 커밋 메시지 규칙 예시
9. 문제 해결(자주 겪는 오류)
10. 추가 학습 자료

---

## 1. 개요 및 기본 개념

* **Git**: 버전 관리 도구(로컬에서 파일 변경 이력 관리)
* **GitHub**: Git 저장소를 호스팅하고 협업 기능(이슈, PR, 액션 등)을 제공하는 서비스
* **원격(remote)**: GitHub에 호스팅된 저장소
* **커밋(commit)**: 변경사항(스냅샷)을 기록하는 단위
* **브랜치(branch)**: 독립된 작업 흐름(예: `main`, `feature/login`)
* **풀 리퀘스트(PR, Pull Request)**: 작업 브랜치를 메인 브랜치에 합치도록 요청하는 절차

---

## 2. 준비물 (설치 및 계정)

### 설치

1. Git 설치: [https://git-scm.com/](https://git-scm.com/) 에서 운영체제에 맞게 설치
2. (선택) Git GUI: GitHub Desktop, Sourcetree 등

### 계정 설정 (최초 한 번)

```bash
git config --global user.name "홍길동"
git config --global user.email "you@example.com"
```

SSH 키 설정(권장)

```bash
ssh-keygen -t ed25519 -C "you@example.com"
# 또는 rsa: ssh-keygen -t rsa -b 4096 -C "you@example.com"
# 생성 후 공개키(~/.ssh/id_ed25519.pub)를 GitHub 계정에 등록
```

---

## 3. 기본 git 명령어 실습

### 새 로컬 저장소 생성

```bash
mkdir myproject
cd myproject
git init
```

### 기존 저장소 복제

```bash
git clone git@github.com:username/repo.git
# 또는 https
# git clone https://github.com/username/repo.git
```

### 변경사항 확인 → 스테이징 → 커밋

```bash
git status
git add 파일명_or_.
git commit -m "짧고 명확한 변경 메시지"
```

### 원격에 푸시

```bash
git push origin main
```

### 원격에서 최신 반영(가져오기)

```bash
git pull origin main
```

---

## 4. 깃허브 원격 저장소 연결

1. GitHub에서 새 저장소 생성(New repository)
2. 로컬에서 원격 추가

```bash
git remote add origin git@github.com:username/repo.git
git push -u origin main
```

`-u`는 추적(tracking) 설정으로 이후 `git push`만으로도 푸시 가능

---

## 5. 브랜치와 협업(풀 리퀘스트 PR)

### 브랜치 만들고 전환

```bash
git checkout -b feature/my-feature
# 작업 후
git add .
git commit -m "feat: add my feature"
git push -u origin feature/my-feature
```

### GitHub에서 PR 생성

1. GitHub 저장소 페이지로 이동 → Compare & pull request 클릭
2. PR 제목, 설명을 작성(무엇을 왜 바꿨는지 명확히)
3. 리뷰어 할당, 라벨, 관련 이슈 연결
4. 리뷰 승인 후 Merge(합치기)

### PR 템플릿(간단 예)

```
## 변경 내용
- 무엇을 변경했는가

## 체크리스트
- [ ] 동작 확인
- [ ] 문서 업데이트

## 관련 이슈
- #12
```

---

## 6. README, LICENSE, .gitignore 작성 예시

### README 기본 구조

```md
# 프로젝트명

한 줄 소개

## 설치
설치 방법

## 사용법
예시 명령

## 기여
기여 방법
```

### LICENSE

* 퍼블릭한 프로젝트라면 MIT, Apache 2.0, GPL 등 중 선택
* 깃허브에서 License 추가하면 자동으로 파일 생성 가능

### .gitignore 예시

```
# Node
node_modules/
.env

# Python
__pycache__/
*.py[cod]
.env
```

---

## 7. GitHub Actions(간단한 CI) 예시

`.github/workflows/ci.yml` 예시

```yaml
name: CI
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Node
        uses: actions/setup-node@v4
        with:
          node-version: '18'
      - name: Install
        run: npm install
      - name: Test
        run: npm test
```

> 위 예시는 Node 프로젝트 기준입니다. Python, Java 등도 `setup-python`, `setup-java` 액션을 사용하면 됩니다.

---

## 8. 커밋 메시지 규칙 예시 (간단한 Conventional Commits 스타일)

```
feat: 새로운 기능 추가
fix: 버그 수정
docs: 문서 수정
chore: 빌드태스크 등 기타 변경
```

예: `feat(auth): add login with kakao` — 괄호로 범위를 표시하면 더 좋습니다.

---

## 9. 문제 해결(자주 겪는 오류)

* 충돌(conflict) 해결: `git pull` 중 충돌 발생하면 충돌 파일을 편집 후

  ```bash
  git add 충돌해결된파일
  git commit
  git push
  ```
* 인증 실패: SSH 키 등록 또는 토큰(PAT) 사용 확인
* 실수로 로컬 커밋 삭제: `git reflog`로 참조 복구 시도

---

## 10. 추가 학습 자료

* Pro Git 책(무료): [https://git-scm.com/book/ko/v2](https://git-scm.com/book/ko/v2)
* GitHub Docs: [https://docs.github.com/](https://docs.github.com/)
* Git 튜토리얼(생활코딩, Nomadcoders 등 유튜브 강좌)

---

## 예시: 간단한 README 템플릿

````md
# 프로젝트명

한 줄 소개

## 시작하기
1. 클론
```bash
git clone git@github.com:username/repo.git
cd repo
````

2. 설치

```bash
npm install
```

## 기여

1. Fork → 브랜치 생성 → PR 생성

```

---

## 마무리 팁

- 작은 단위로 자주 커밋하세요.
- PR 설명은 다른 사람이 이해할 수 있게 작성하세요.
- 중요한 설정(비밀키 등)은 절대 커밋하지 마세요(.env 사용).

---

## 변경 로그
- v1.0: 초안 작성 (기본 명령어, 템플릿, CI 예시 포함)

---

#### **후속 추천 질문**

* [1] 내 프로젝트에 맞게 README를 커스터마이징해 줄래?
* [2] GitHub Actions로 테스트+배포 설정을 도와줄래?
* [3] 내 현재 깃 저장소 상태를 보고 문제점을 진단해 줄래?

숫자 1, 2, 3 중 하나를 입력하거나, 계속해서 대화하세요!

***
이 GPT의 작동 방식이 궁금하다면? 👉 [챗과장 GPTs 마스터 패키지](https://www.chatgwajang.com/products/gpts-master)를 확인해 보세요!

```
