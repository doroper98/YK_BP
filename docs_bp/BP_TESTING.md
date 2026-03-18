# BP_TESTING — 테스트 전략

> AI 협업 개발에서는 "자동 검증(CLI Gate)"과 "수동 검증(사람 피드백)"을 조합합니다.
> 유닛 테스트가 없어도 일관된 품질을 유지할 수 있는 실전 검증 체계입니다.

## 2-Layer 검증 체계

```
Layer 1: CLI Gate (자동, AI가 매번 실행)
    tsc --noEmit     → 타입 에러 감지
    npm run build    → 빌드 성공 확인
    npm run lint     → 코드 스타일 (warning OK, error FAIL)
    npm run test     → 유닛 테스트 (설정된 경우)

Layer 2: Manual Verification (수동, 사람이 실행)
    앱 실행 → UI 확인 → 스크린샷/피드백 → AI에게 전달
```

## Layer 1: CLI Gate

### 실행 시점
- **매 구현 단계 완료 후** (코드 1개 기능 작성 후)
- **커밋 전** (DEVLOG 기록 전 반드시)
- **버전 릴리스 전**

### 명령어
```bash
# 필수 (매 단계)
tsc --noEmit && npm run build

# 권장 (커밋 전)
tsc --noEmit && npm run build && npm run lint

# 전체 (릴리스 전)
tsc --noEmit && npm run build && npm run lint && npm run test
```

### 실패 시 처리
```
CLI Gate 실패
    ↓
[AI] 에러 메시지 분석
    ↓
[AI] 코드 수정
    ↓
[AI] CLI Gate 재실행
    ↓  (통과할 때까지 반복)
[AI] DEVLOG에 실패/재시도 기록
```

### package.json 스크립트 설정
```json
{
  "scripts": {
    "typecheck": "tsc --noEmit",
    "build": "tsc --noEmit && vite build",
    "lint": "eslint src/ --ext .ts,.tsx",
    "test": "jest",
    "gate": "npm run typecheck && npm run build && npm run lint"
  }
}
```

## Layer 2: Manual Verification

### 검증 흐름
```
1. [AI] 구현 완료 + CLI Gate 통과 알림
2. [사람] 앱 실행 (npm start 또는 exe)
3. [사람] 해당 기능 직접 테스트
4. [사람] 결과를 AI에게 피드백:
   - OK → "잘 되네" / "좋아"
   - NG → "여전히 안 돼" / "이건 다르게 해줘" / 스크린샷
5. [AI] NG인 경우 수정 → CLI Gate → 재검증 요청
```

### SC별 검증 체크리스트 작성법
```markdown
## Manual Verification Checklist

| SC | 검증 항목 | 검증 방법 | Pass/Fail |
|----|-----------|-----------|-----------|
| SC-01 | 앱 5초 내 실행 | 스톱워치 | |
| SC-02 | 로그인 성공 | 올바른 비밀번호 입력 | |
| SC-02 | 로그인 실패 | 틀린 비밀번호 입력 | |
| SC-02 | 계정 잠금 | 5회 연속 실패 | |
| SC-04 | 줌 25% | Ctrl+마우스 휠 | |
| SC-04 | 줌 400% | Ctrl+마우스 휠 | |
| SC-04 | 캔버스 팬 | 마우스 드래그 | |
```

## 스크린샷 피드백 패턴

사람이 AI에게 UI 피드백을 줄 때 가장 효과적인 방법:

### 패턴 1: 스크린샷 + 한 줄 설명
```
[스크린샷 첨부]
"이 버튼 위치가 시계를 가리고 있어"
```

### 패턴 2: 비교 피드백
```
"follow up 창의 테마는 좋은데, git 창은 어두워서 안 어울려"
```

### 패턴 3: 부정 피드백 (이전 수정 무효)
```
"여전히 해상도가 거지 같은데?"
→ AI: 이전 접근법 폐기, 다른 방법 시도
```

## 버그 발견 → 수정 → 검증 사이클

```
[사람] 버그 발견 → 현상 설명
    ↓
[AI] BUG-NNN 등록 (DEVLOG)
    ↓
[AI] 원인 분석
    ↓
[AI] 수정 구현
    ↓
[AI] CLI Gate ✅
    ↓
[사람] 재검증
    ↓
[AI] BUG-NNN → RESOLVED (DEVLOG)
```

## 테스트 없이도 품질을 유지하는 비결

1. **TypeScript strict 모드** — 타입 에러가 런타임 버그의 60%+ 차단
2. **CLI Gate 강제** — 빌드 안 되는 코드가 커밋되지 않음
3. **사람의 즉각 피드백** — 유닛 테스트보다 빠른 UI 검증
4. **DEVLOG 교훈 축적** — 같은 버그 재발 방지
5. **버전별 개선 로그** — 회귀 발생 시 변경 이력 추적 가능

## TCC 프로젝트 실적

- CLI Gate 100% 통과 (29/29 실험)
- 수동 검증으로 8개 버그 발견 → 전부 해결
- TypeScript strict 모드 + CLI Gate만으로 안정적 품질 유지
- 별도 테스트 프레임워크 없이 v1.0.0 릴리스 달성
