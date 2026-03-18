# BP_CLAUDE_MD — CLAUDE.md 작성 가이드

> CLAUDE.md는 AI 에이전트가 프로젝트에 진입할 때 가장 먼저 읽는 "행동 규칙서"입니다.
> 이 파일이 잘 작성되면 AI가 프로젝트 컨텍스트를 빠르게 파악하고, 일관된 품질로 작업합니다.

## 왜 CLAUDE.md가 필요한가

- AI는 매 세션마다 프로젝트 맥락을 잃음 → CLAUDE.md가 "기억"을 대체
- 코드 컨벤션, 빌드 방법, 금지 사항을 명시하지 않으면 AI가 매번 다른 스타일로 작업
- 정규 문서(GOAL, DEVLOG, ARCHITECTURE 등)의 위치를 알려줘야 AI가 스스로 탐색 가능

## CLAUDE.md 템플릿

```markdown
# {프로젝트명} — Repository Knowledge Harness

## Canonical Documents (정규 문서)

아래 문서를 반드시 숙지한 뒤 작업을 시작할 것:

| 문서 | 경로 | 설명 |
|------|------|------|
| GOAL | GOAL.md | 목표, 요구사항(REQ-*), 성공 기준(SC-*) |
| DEVLOG | DEVLOG.md | 실험 로그, 버그 로그, 개선 로그, 교훈 |
| WORKFLOWS | WORKFLOWS.md | 단계별 실행 절차 |
| ARCHITECTURE | docs_canonical/ARCHITECTURE.md | 시스템 아키텍처, 설계 결정 |
| STYLEGUIDE | docs_canonical/STYLEGUIDE.md | 코드 컨벤션, 커밋 메시지 형식 |
| TESTING | docs_canonical/TESTING.md | 테스트 전략, CLI Gate |
| REPO_MAP | docs_canonical/REPO_MAP.md | 디렉토리 구조, 데이터 경로 |

## Execution Rules (실행 규칙)

1. **작업 전**: GOAL.md의 REQ/SC 확인
2. **매 단계 후**: CLI Gate 통과 (`tsc --noEmit && npm run build`)
3. **커밋 시**: DEVLOG.md에 EXP-{NNN} 기록 + 버전 업 + git commit
4. **실패 시**: 원인 분석 → 재시도 (같은 실수 반복 금지)
5. **단계 전환**: 모든 SC PASS 확인 후 진행

## Version Management (버전 관리)

- 빌드마다 `package.json` 버전 업데이트 (MAJOR.MINOR.PATCH)
- DEVLOG.md "개선 로그" 섹션에 변경 내역 기록
- 커밋 메시지에 버전 포함: `v0.2.0: {변경 요약}`
- 버전 기록은 누적 (최신 우선, 과거 기록 삭제 금지)

## Constraints (제약 조건)

- console.log 프로덕션 코드 금지
- @ts-ignore 사용 금지
- any 타입 사용 금지
- node_modules 커밋 금지
- 평문 시크릿 코드 내 포함 금지
- {프로젝트별 추가 제약}
```

## 작성 팁

### 1. 구체적으로 쓸 것
```
# Bad
"코드를 깔끔하게 작성하세요"

# Good
"함수명은 camelCase, 컴포넌트는 PascalCase, 파일명은 PascalCase.tsx"
```

### 2. "왜"를 포함할 것
```
# Bad
"npm config set strict-ssl false 설정 필요"

# Good
"LG 사내망 Somansa Root CA로 인해 npm install 시 cert 에러 발생
→ npm config set strict-ssl false 필요"
```

### 3. 정규 문서 경로를 명시할 것
AI는 파일 위치를 모르면 직접 탐색해야 하므로 시간 낭비. 경로를 테이블로 정리.

### 4. CLI Gate를 반드시 포함할 것
AI가 "빌드 성공했겠지" 하고 넘어가는 것을 방지. 매 단계마다 자동 검증 강제.

## TCC 프로젝트 실제 사례

TCC의 CLAUDE.md는 다음을 명시하여 29회 실험 100% 성공률을 달성:
- 정규 문서 4종의 정확한 경로
- 5단계 실행 규칙 (특히 CLI Gate 강제)
- 버전 관리 원칙 (빌드 = 버전 업 = DEVLOG 원자적 수행)
- 단계 전환 조건 (SC 전체 PASS 필수)
