# 일주일 운동 루틴 (운동 관리 앱)

요일별 헬스 루틴을 체크하고, 프로필(체중 등) 기준으로 소모 칼로리를 집계하며, 달력에서 하루 목표 달성 여부를 관리하는 단일 HTML 웹앱입니다. 서버 없이 동작하며 기록은 브라우저(localStorage)에 저장됩니다.

## 파일 구성

| 파일 | 설명 |
|---|---|
| `index.html` | 앱 본체 (HTML + CSS + JS 모두 포함) |
| `img/*.jpg` | 운동 카드 기구 사진 (20장) |
| `manifest.json` | 홈 화면에 추가 시 앱처럼 열리게 하는 설정 |
| `.nojekyll` | GitHub Pages가 파일을 가공하지 않도록 하는 표시 |

## GitHub Pages로 배포

1. GitHub에서 새 저장소 생성 (예: `workout-routine`, Public)
2. 이 폴더의 파일을 모두 업로드 (아래 두 방법 중 하나)
3. 저장소 **Settings → Pages → Build and deployment**
   - Source: `Deploy from a branch`
   - Branch: `main` / `/ (root)` → Save
4. 1~2분 후 `https://<계정명>.github.io/<저장소명>/` 에서 접속

### 방법 A. 웹 브라우저에서 업로드 (git 설치 불필요)

저장소 페이지 → `Add file` → `Upload files` → 폴더 안 파일 전체를 드래그 → `Commit changes`.
`img` 폴더는 폴더째로 드래그하면 구조가 유지됩니다. (`.nojekyll`, `.gitignore` 같은 숨김 파일은 탐색기에서 "숨긴 항목 표시"를 켜야 보입니다.)

### 방법 B. Git 명령어

```bash
cd "C:\Users\e102423\Downloads\운동관리앱_v01"
git init
git add .
git commit -m "운동 관리 앱 초안 v0.1"
git branch -M main
git remote add origin https://github.com/<계정명>/<저장소명>.git
git push -u origin main
```

이후 수정 사항 반영:

```bash
git add .
git commit -m "변경 내용"
git push
```

## 폰에서 앱처럼 쓰기

배포 주소를 폰 브라우저로 열고 **홈 화면에 추가**(iOS Safari: 공유 → 홈 화면에 추가 / Android Chrome: 메뉴 → 홈 화면에 추가)하면 전체화면 앱처럼 열립니다.

## 데이터 저장 구조 (localStorage 키 `gym-routine-v1`)

```json
{
  "profile": { "name": "", "sex": "m", "age": 35, "height": 175, "weight": 78, "fat": 20, "muscle": 33, "waist": 84 },
  "log": { "2026-09-17": { "thu1": { "done": true, "min": 22, "at": 1758000000000 } } },
  "weekly": [ { "date": "2026-09-14", "weight": 78, "waist": 84, "pain": 2 } ]
}
```

기록은 기기별로 따로 저장되므로, 다른 기기로 옮길 때는 프로필 탭의 **내보내기 → 가져오기**를 사용하세요. 나중에 Flask/SQLite 백엔드로 확장할 때 이 구조를 그대로 테이블로 옮기면 됩니다.

## 칼로리 계산 기준

- 유산소: `MET × 체중(kg) × 시간(h)`
- 근력: 세트당 (회당 3초 + 세트 간 휴식 60초) 시간에 부하별 MET 3.5~6 적용
- 딥스 머신(보조식)은 `체중 − 보조 중량`을 부하로 계산

어림값이므로 절대치보다는 추세 비교용으로 보세요.
