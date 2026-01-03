# Firebase Config 컬렉션 구조

## 컬렉션 경로
```
artifacts/production-roster-2026/public/config/
```

## 문서 구조

### 1. holidays
**문서 ID**: `holidays`

공휴일 데이터를 연도별로 저장합니다.

```json
{
  "2025": {
    "2025-01-01": "신정",
    "2025-01-28": "설날",
    "2025-01-29": "설날",
    "2025-01-30": "설날",
    "2025-03-01": "삼일절",
    "2025-03-03": "대체공휴일",
    "2025-05-05": "어린이날",
    "2025-05-06": "대체공휴일",
    "2025-06-06": "현충일",
    "2025-08-15": "광복절",
    "2025-10-03": "개천절",
    "2025-10-05": "추석",
    "2025-10-06": "추석",
    "2025-10-07": "추석",
    "2025-10-08": "대체공휴일",
    "2025-10-09": "한글날",
    "2025-12-25": "성탄절"
  },
  "2026": {
    "2026-01-01": "신정",
    "2026-02-16": "설날",
    ...
  }
}
```

### 2. teamMembers
**문서 ID**: `teamMembers`

각 팀의 구성원 명단을 배열로 저장합니다.

```json
{
  "A": ["박영남", "홍성민", "김정국", "김도훈2", "이민정", "조명주", ""],
  "B": ["오영석", "이동웅", "김윤수", "주정호", "박소희", "조미형", "송보란"],
  "C": ["정인철", "김도영", "이은혁", "최용", "전수진", "김지혜2", ""],
  "DAY_ONLY": ["신화동", "임현규", "김민솔", ""]
}
```

### 3. shiftTypes
**문서 ID**: `shiftTypes`

근무 유형별 UI 스타일 정의를 저장합니다.

```json
{
  "DAY": {
    "label": "주간",
    "color": "bg-amber-50 text-amber-700",
    "border": "border-amber-200",
    "badge": "bg-amber-100 text-amber-800 ring-1 ring-amber-200"
  },
  "NIGHT": {
    "label": "야간",
    "color": "bg-indigo-50 text-indigo-700",
    "border": "border-indigo-200",
    "badge": "bg-indigo-100 text-indigo-800 ring-1 ring-indigo-200"
  },
  "JU_HYU": {
    "label": "무휴",
    "color": "bg-slate-50 text-slate-500",
    "border": "border-slate-200",
    "badge": "bg-slate-100 text-slate-600 ring-1 ring-slate-200"
  },
  "MU_HYU": {
    "label": "주휴",
    "color": "bg-emerald-50 text-emerald-700",
    "border": "border-emerald-200",
    "badge": "bg-emerald-100 text-emerald-800 ring-1 ring-emerald-200"
  },
  "YU_HYU": {
    "label": "유휴",
    "color": "bg-red-50 text-red-700",
    "border": "border-red-200",
    "badge": "bg-red-100 text-red-800 ring-1 ring-red-200"
  }
}
```

### 4. attendanceOptions
**문서 ID**: `attendanceOptions`

출근 유형 옵션 목록을 저장합니다.

```json
{
  "options": [
    { "value": "", "label": "정상", "color": "text-slate-500" },
    { "value": "연차", "label": "연차", "color": "text-red-500 font-bold" },
    { "value": "반차", "label": "반차", "color": "text-blue-500 font-bold" },
    { "value": "조퇴", "label": "조퇴", "color": "text-yellow-600 font-bold" },
    { "value": "지각", "label": "지각", "color": "text-purple-500 font-bold" },
    { "value": "교육", "label": "교육", "color": "text-green-600 font-bold" },
    { "value": "훈련", "label": "훈련", "color": "text-indigo-600 font-bold" },
    { "value": "특근", "label": "특근", "color": "text-pink-600 font-bold" },
    { "value": "기타", "label": "기타", "color": "text-gray-600 font-bold" }
  ]
}
```

### 5. systemSettings
**문서 ID**: `systemSettings`

시스템 전반의 설정값을 저장합니다.

```json
{
  "baseDate": "2026-01-01",
  "shiftWorkHours": 10.25,
  "dayWorkHours": 8.0,
  "version": "1.0.0",
  "lastUpdated": "2026-01-03T21:51:00+09:00"
}
```

## 데이터 접근 패턴

### 읽기 (index.html)
```javascript
const configRef = doc(db, 'artifacts', appId, 'public', 'config', 'holidays');
const snapshot = await getDoc(configRef);
const holidays = snapshot.data();
```

### 실시간 리스닝 (index.html, manager.html)
```javascript
const configRef = doc(db, 'artifacts', appId, 'public', 'config', 'teamMembers');
onSnapshot(configRef, (snapshot) => {
  window.TEAM_MEMBERS = snapshot.data();
  renderCalendar();
});
```

### 쓰기 (manager.html)
```javascript
const configRef = doc(db, 'artifacts', appId, 'public', 'config', 'teamMembers');
await setDoc(configRef, updatedTeamMembers);
```
