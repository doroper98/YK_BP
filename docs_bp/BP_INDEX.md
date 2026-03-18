# Best Practice Index — AI-Assisted Software Development

> 이 문서는 TCC 프로젝트에서 검증된 AI 협업 개발 방법론의 총 가이드입니다.
> 새 프로젝트 시작 시 이 디렉토리(`docs_bp/`)를 복사하고, AI에게 "BP_ 문서를 참고해서 프로젝트 구조를 수립해"라고 지시하세요.

## 문서 목록

| # | 문서 | 설명 | 적용 시점 |
|---|------|------|-----------|
| 1 | [BP_CLAUDE_MD.md](BP_CLAUDE_MD.md) | CLAUDE.md 작성 가이드 — AI 에이전트의 행동 규칙 정의 | 프로젝트 최초 생성 시 |
| 2 | [BP_PROJECT_STRUCTURE.md](BP_PROJECT_STRUCTURE.md) | 프로젝트 디렉토리 구조 & Canonical 문서 체계 | 프로젝트 최초 생성 시 |
| 3 | [BP_REQUIREMENTS.md](BP_REQUIREMENTS.md) | 요구사항 정의 & AI에게 전달하는 방법 | 기획 단계 |
| 4 | [BP_WORKFLOW.md](BP_WORKFLOW.md) | AI 협업 개발 워크플로우 (요구→구현→검증→배포) | 개발 전 과정 |
| 5 | [BP_DEVLOG.md](BP_DEVLOG.md) | DEVLOG 관리 체계 — 실험/버그/개선 로그 | 개발 전 과정 |
| 6 | [BP_TESTING.md](BP_TESTING.md) | 테스트 전략 — CLI Gate + Manual Verification | 매 구현 단계 |
| 7 | [BP_VERSIONING.md](BP_VERSIONING.md) | 버전 관리 & 릴리스 전략 | 빌드/배포 시 |

## 사용법

```
1. 새 프로젝트 루트에 docs_bp/ 폴더를 복사
2. AI에게 지시: "docs_bp/BP_INDEX.md를 읽고, 이 프로젝트에 맞게 구조를 수립해"
3. AI가 CLAUDE.md, DEVLOG.md, GOAL.md, WORKFLOWS.md, docs_canonical/ 등을 자동 생성
4. 개발 시작
```

## 핵심 원칙

1. **Traceability (추적성)** — 모든 변경은 요구사항 ID ↔ 실험 ID ↔ 커밋으로 추적 가능
2. **CLI Gate (자동 검증)** — 매 단계마다 `tsc --noEmit && npm run build` 통과 필수
3. **Atomic Version (원자적 버전)** — 빌드 = 버전 업 = DEVLOG 기록 (동시 수행)
4. **Lessons Learned (교훈 축적)** — 삽질/해결 내역을 DEVLOG에 기록하여 반복 방지
5. **Canonical Docs (정규 문서)** — ARCHITECTURE, STYLEGUIDE, TESTING, REPO_MAP 4종 유지
