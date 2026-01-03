# GitHub 리포지토리 생성 가이드

## 방법 1: 웹 인터페이스 사용 (권장)

### 1. GitHub 리포지토리 생성

1. https://github.com/new 접속
2. 다음 정보 입력:
   - **Repository name**: `introjo-roster`
   - **Description**: `원료생산팀 설비운영 근무 스케줄 관리 시스템`
   - **Visibility**: Public 또는 Private 선택
   - **Initialize this repository**: 체크 해제 (이미 로컬에 코드가 있음)
3. **"Create repository"** 클릭

### 2. 로컬 코드 푸시

터미널에서 다음 명령 실행:

```bash
cd "/Users/gimdohun/Side Project/Introjo"

# 원격 리포지토리 추가 (username을 본인의 GitHub 유저명으로 변경)
git remote add origin https://github.com/username/introjo-roster.git

# 코드 푸시
git push -u origin master
```

> **주의**: `username`을 본인의 GitHub 유저명으로 변경하세요!

## 방법 2: GitHub CLI 사용 (빠름)

GitHub CLI가 설치되어 있다면:

```bash
cd "/Users/gimdohun/Side Project/Introjo"

# 리포지토리 생성 및 푸시 (한 번에)
gh repo create introjo-roster --public --source=. --description="원료생산팀 설비운영 근무 스케줄 관리 시스템" --push
```

GitHub CLI 설치 (설치되지 않은 경우):
```bash
brew install gh
gh auth login
```

## 푸시 완료 확인

1. https://github.com/username/introjo-roster 접속
2. 파일 목록 확인:
   - index.html
   - manager.html
   - migrate-config.html
   - config-structure.md
   - README.md
   - .gitignore

## 다음 단계

GitHub 푸시 완료 후:
1. `DEPLOYMENT.md` 파일을 참고하여 Cloudflare Pages 배포
2. 배포 완료 후 `migrate-config.html` 실행
