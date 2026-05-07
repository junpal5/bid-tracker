# bid-tracker 프로젝트 가이드

## 프로젝트 개요

- **파일**: `index.html` (단일 파일 대시보드 — 모든 CSS·HTML·JS 포함), `data.json`, `version-history.json`
- **저장소**: `junpal5/bid-tracker` (GitHub Pages로 배포)
- **사용자**: 비개발자 — 기술 용어 없이 한국어로 안내할 것

---

## 작업 워크플로우 (모든 작업 요청 시 반드시 준수)

### 1단계 — 계획 안내 (작업 시작 전 필수)
작업을 세부 단계로 나누어 **먼저 사용자에게 안내**한다. 작업을 시작하기 전에 항상 이 단계를 수행한다.

### 2단계 — 단계별 작업 수행
계획한 순서대로 작업을 진행한다.

### 3단계 — 변경 내용 요약 (한국어)
작업 완료 후 변경된 내용을 **한국어**로 간결하게 요약한다.
`version-history.json`의 `changes` 배열에 들어갈 항목 형태로 작성한다.

### 4단계 — 버전 선택지 제공
요약 후 아래 선택지를 사용자에게 제시한다. 현재 버전은 `version-history.json`의 `currentVersion`을 참조한다.

| 선택 | 버전 변화 | 적합한 경우 |
|------|-----------|-------------|
| 패치 | x.x.**+1** | 오탈자 수정, 데이터 업데이트, 사소한 버그 수정 |
| 마이너 | x.**+1**.0 | 새 기능 추가, UI 개선, 기존 기능 변경 |
| 메이저 | **+1**.0.0 | 전체 구조 변경, 대규모 리디자인 |
| 버전 유지 | 변경 없음 | 임시 수정 또는 테스트 |

### 5단계 — 자동 Push
사용자가 버전을 선택하면:
1. `version-history.json` 업데이트 (`currentVersion` + `history` 배열 맨 앞에 새 항목 추가)
2. 변경된 파일 전체(`index.html`, `version-history.json` 등) commit
3. **`main` 브랜치에 직접 push** (PR 없이 바로 반영)

```bash
git add <변경된 파일들>
git commit -m "dashboard: <작업 요약>"
git push origin main
```

> Push 실패 시: 원격에 새 커밋이 있을 수 있으므로 `git pull origin main --rebase` 후 재시도한다.

---

## 파일별 주의사항

### index.html
- 2700줄 이상의 단일 파일. CSS·JS 모두 인라인 포함.
- **다크모드 구현 방식**: `<style id="glassmorphism-override">` 태그를 `.disabled` 토글하는 방식. `disabled=true` → 다크, `disabled=false` → 라이트.
- 편집 시 Read 도구로 전체 파일을 읽지 말고, grep/offset으로 필요한 부분만 읽는다.

### version-history.json
- `currentVersion`: 현재 버전 문자열
- `history`: 최신 버전이 배열 **맨 앞**에 위치
- 날짜 형식: `"2026-05-07T00:00:00.000Z"` (ISO 8601)

---

## Push 인증 설정

git remote URL에 PAT가 포함되어 있어 별도 인증 없이 push 가능하다.
PAT가 만료되거나 갱신된 경우 아래 명령으로 업데이트한다:

```bash
git remote set-url origin https://junpal5:<NEW_TOKEN>@github.com/junpal5/bid-tracker.git
```

---

## 금지 사항

- PR(Pull Request) 생성 금지 — main에 직접 push한다.
- 불필요한 브랜치 생성 금지.
- 사용자에게 git 명령어를 직접 실행하도록 요청하지 말 것 (Claude가 대신 실행).
- 영어 기술 용어를 설명 없이 사용하지 말 것.
