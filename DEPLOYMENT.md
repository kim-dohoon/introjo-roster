# Cloudflare Pages 배포 가이드

## 🚀 Cloudflare Pages 배포 단계

### 1. Cloudflare Pages 접속
1. https://dash.cloudflare.com/ 로그인
2. 왼쪽 메뉴에서 **"Pages"** 클릭
3. **"Create a project"** 버튼 클릭

### 2. GitHub 연동
1. **"Connect to Git"** 선택
2. GitHub 계정 연결 (처음 사용 시 승인 필요)
3. 리포지토리 목록에서 **`introjo-roster`** 선택
4. **"Begin setup"** 클릭

### 3. 빌드 설정

다음 설정을 입력합니다:

| 항목 | 값 |
|------|-----|
| **Project name** | `introjo-roster` (또는 원하는 이름) |
| **Production branch** | `master` (또는 `main`) |
| **Build command** | (비워두기 - 정적 HTML이므로 빌드 불필요) |
| **Build output directory** | `/` (루트 디렉토리) |

4. **"Save and Deploy"** 클릭

### 4. 배포 완료 확인

- 배포가 완료되면 URL이 생성됩니다: `https://introjo-roster.pages.dev`
- 또는 커스텀 도메인 설정 가능

## 📋 배포 후 필수 작업

### 1. 초기 데이터 마이그레이션

배포된 사이트에서 **한 번만** 실행:

```
https://introjo-roster.pages.dev/migrate-config.html
```

1. 위 URL 접속
2. **"🚀 마이그레이션 시작"** 버튼 클릭
3. 로그에서 "✅ 모든 설정 데이터 마이그레이션 완료!" 확인
4. 완료 후 이 페이지는 다시 접속하지 않아도 됨

### 2. 동작 확인

**사용자 앱 테스트:**
```
https://introjo-roster.pages.dev/
```
- [LIVE] 배지 표시 확인
- 공휴일이 빨간색으로 표시되는지 확인
- 팀원 이름이 자동으로 채워지는지 확인

**관리자 앱 테스트:**
```
https://introjo-roster.pages.dev/manager.html
```
- 팀 관리 탭에서 구성원 추가/수정/삭제
- 변경 후 index.html에 즉시 반영되는지 확인

## 🔐 보안 관련

### manager.html 접근 제한

**옵션 1: Cloudflare Access 사용 (권장)**

1. Cloudflare Dashboard → **Zero Trust** → **Access** → **Applications**
2. **"Add an application"** 클릭
3. Self-hosted 선택
4. Application settings:
   - **Application name**: `Manager Panel`
   - **Session Duration**: `24 hours`
   - **Application domain**: `introjo-roster.pages.dev/manager.html`
5. Policy 설정:
   - Action: `Allow`
   - Include: `Emails ending in @your-company.com`
6. **Save**

**옵션 2: 간단한 방법**

manager.html을 로컬에서만 사용하고 배포하지 않기:
1. GitHub 리포지토리에서 manager.html 삭제
2. 로컬 PC에서만 manager.html 보관
3. 배포 시에는 index.html과 migrate-config.html만 포함

## 🔄 업데이트 방법

코드 변경 후 자동으로 배포됩니다:

```bash
git add .
git commit -m "Update: 변경 내용"
git push
```

Cloudflare Pages가 자동으로 감지하고 재배포합니다 (약 1-2분 소요).

## 📊 커스텀 도메인 설정 (선택사항)

Cloudflare Pages에서 자체 도메인 연결:

1. Pages 프로젝트 → **Custom domains** 탭
2. **"Set up a custom domain"** 클릭
3. 도메인 입력 (예: `roster.yourcompany.com`)
4. DNS 레코드 자동 설정 (Cloudflare에서 도메인 관리 시)

## ⚙️ Firestore 보안 규칙 설정 (권장)

Firebase Console → Firestore Database → Rules:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Config 읽기는 누구나, 쓰기는 인증된 사용자만
    match /artifacts/production-roster-2026/public/config/{document=**} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    
    // Roster 데이터는 인증된 사용자만
    match /artifacts/production-roster-2026/public/data/{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

## 🆘 문제 해결

### 배포 실패
- Cloudflare Pages → Deployments에서 로그 확인
- Build command와 Build output directory가 비어있는지 확인

### [LIVE] 배지가 안 나타남
- Firebase 설정 확인 (Firestore, Authentication 활성화)
- 브라우저 콘솔(F12)에서 에러 메시지 확인

### 데이터가 로드되지 않음
- migrate-config.html을 실행했는지 확인
- Firebase Console에서 config 컬렉션이 생성되었는지 확인

## 📞 지원

문제 발생 시:
1. 브라우저 콘솔(F12) 에러 확인
2. Cloudflare Pages 배포 로그 확인
3. Firebase Console에서 Firestore 데이터 확인
