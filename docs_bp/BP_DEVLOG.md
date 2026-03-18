# BP_DEVLOG — DEVLOG 관리 체계

> DEVLOG.md는 프로젝트의 "블랙박스"입니다.
> 모든 실험, 버그, 개선, 교훈이 누적되어 AI가 과거 맥락을 파악하고 같은 실수를 반복하지 않습니다.

## DEVLOG.md 템플릿

```markdown
# {프로젝트명} — Development Log

## ID 체계

| 접두어 | 의미 | 형식 | 예시 |
|--------|------|------|------|
| EXP | 실험 (커밋 단위) | EXP-{NNN} | EXP-001 |
| S | 단계 | S{Phase}.{Step} | S1.3 |
| REQ | 기능 요구사항 | REQ-{Domain}-{NNN} | REQ-AUTH-001 |
| NFR | 비기능 요구사항 | NFR-{NNN} | NFR-001 |
| SC | 성공 기준 | SC-{NN} | SC-05 |
| BUG | 결함 | BUG-{NNN} | BUG-003 |

---

## Traceability Matrix (추적 매트릭스)

| Step | 설명 | Requirement | SC | Status |
|------|------|-------------|-----|--------|
| S1.1 | {단계 설명} | REQ-XXX-001 | SC-01 | ✅ |
| S1.2 | ... | ... | ... | ⬜ |

---

## Experiment Log (실험 로그)

### EXP-001 · S1.1 — {실험 제목}
- **날짜**: YYYY-MM-DD
- **요구사항**: REQ-XXX-001
- **변경 파일**: {파일 목록}
- **CLI Gate**: ✅ tsc OK · ✅ build OK
- **결과**: ✅ PASS / ❌ FAIL
- **커밋**: `v0.1.0: {메시지}`

---

## Cumulative Stats (누적 통계)

| 항목 | 값 |
|------|-----|
| 총 실험 | {N} |
| 성공 커밋 | {N} |
| 리셋 | {N} |
| 성공률 | {N}% |

---

## Bug Log (버그 로그)

### BUG-001 — {버그 제목}
- **발견**: EXP-{NNN}
- **현상**: {사용자가 보고한 현상}
- **원인**: {근본 원인}
- **수정**: {수정 내용 + 파일}
- **상태**: ✅ RESOLVED / 🔴 OPEN

---

## 개선 로그 (Improvement Log)

> 버전별 UI/UX, 성능, 품질 개선 사항을 기록한다.
> 빌드+형상관리 시 버전을 올리고, 해당 버전의 모든 개선 내역을 이 섹션에 기록한다.

### v{X.Y.Z} — YYYY-MM-DD ({변경 요약})

| # | 개선 항목 | 변경 파일 | 변경 내용 |
|---|-----------|-----------|-----------|
| 1 | {항목} | {파일} | {내용} |

---

## Lessons Learned (교훈)

1. {교훈 내용} → {해결 방법}
2. ...
```

## DEVLOG 작성 규칙

### 1. 원자적 기록
- 1 커밋 = 1 EXP = 1 DEVLOG 엔트리
- 커밋하기 전에 DEVLOG에 먼저 기록

### 2. 실패도 기록
```markdown
### EXP-007 · S2.3 — AudioContext 동시 사용 시도
- **결과**: ❌ FAIL
- **원인**: MediaRecorder + AudioContext가 같은 stream 사용 시 충돌
- **교훈**: stream.clone()으로 분리 필요
```

### 3. 버그 로그에 "근본 원인" 필수
```markdown
# Bad
- 원인: 코드 오류

# Good
- 원인: Electron에서 git rev-parse HEAD는 빈 리포에서 실패.
  HEAD가 아직 존재하지 않기 때문. git rev-parse --git-dir로 대체 필요.
```

### 4. 교훈은 "상황 → 해결" 형식
```markdown
# Bad
"npm install이 안 됨"

# Good
"LG Somansa Root CA 환경에서 npm install 시 CERT_NOT_TRUSTED 에러 발생
→ npm config set strict-ssl false로 해결"
```

### 5. 개선 로그는 테이블 형식
```markdown
| # | 개선 항목 | 변경 파일 | 변경 내용 |
|---|-----------|-----------|-----------|
| 1 | DPI 블러 수정 | main.ts, index.html | GPU rasterization 활성화 + font smoothing CSS |
```

## 개선 로그 vs 실험 로그

| | 실험 로그 | 개선 로그 |
|-|----------|----------|
| 단위 | 커밋 1개 | 버전 1개 (여러 커밋 포함 가능) |
| 목적 | 추적성 (REQ ↔ SC ↔ 코드) | 변경 이력 (무엇이 바뀌었나) |
| 대상 | 개발 단계의 정형화된 실험 | UI/UX 개선, 버그픽스, 리팩토링 등 |
| 형식 | 상세 (CLI Gate 결과 포함) | 테이블 요약 |

## TCC 프로젝트 실적

- DEVLOG.md 63KB, 29개 실험 기록
- 8개 버그 전부 근본 원인 분석 + 해결
- 12개 교훈 축적 → 이후 세션에서 같은 실수 0회
- v0.2.0 ~ v1.0.0까지 20+개 버전의 개선 이력 완전 보존
