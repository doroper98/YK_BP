# BP_PROJECT_STRUCTURE — 프로젝트 디렉토리 구조 & 문서 체계

> 프로젝트 초기에 올바른 구조를 잡으면 AI가 파일을 빠르게 탐색하고,
> 일관된 패턴으로 코드를 작성합니다.

## 루트 디렉토리 구조 (권장)

```
{project-root}/
│
├── CLAUDE.md                    # AI 에이전트 행동 규칙 (→ BP_CLAUDE_MD.md)
├── DEVLOG.md                    # 개발 로그 (→ BP_DEVLOG.md)
├── GOAL.md                      # 목표, 요구사항, 성공 기준 (→ BP_REQUIREMENTS.md)
├── WORKFLOWS.md                 # 단계별 실행 절차 (→ BP_WORKFLOW.md)
├── README.md                    # 프로젝트 소개 (외부 공개용)
├── package.json                 # 의존성 & 스크립트
├── tsconfig.json                # TypeScript 설정
├── vite.config.ts               # 빌드 설정 (or webpack, etc.)
├── .gitignore                   # Git 제외 목록
├── .env                         # 환경 변수 (Git 제외)
│
├── docs_canonical/              # 정규 문서 (4종)
│   ├── ARCHITECTURE.md          # 시스템 아키텍처
│   ├── STYLEGUIDE.md            # 코드 컨벤션
│   ├── TESTING.md               # 테스트 전략
│   └── REPO_MAP.md              # 파일/폴더 구조 설명
│
├── docs_bp/                     # Best Practice 문서 (재사용용)
│   ├── BP_INDEX.md
│   └── BP_*.md
│
├── src/                         # 소스 코드
│   ├── main/                    # 백엔드 / 메인 프로세스
│   │   ├── services/            # 비즈니스 로직 (도메인별 분리)
│   │   └── {entry}.ts           # 진입점
│   ├── renderer/                # 프론트엔드 / UI
│   │   ├── components/          # UI 컴포넌트 (도메인별 하위 폴더)
│   │   ├── hooks/               # 커스텀 훅
│   │   ├── stores/              # 상태 관리
│   │   ├── services/            # 프론트 서비스 로직
│   │   ├── utils/               # 유틸리티
│   │   ├── types/               # 타입 정의
│   │   └── styles/              # 글로벌 스타일
│   ├── shared/                  # 프론트/백 공유 코드
│   │   └── types.ts
│   └── preload/                 # Electron 전용 (해당 시)
│
├── dist/                        # 빌드 산출물 (Git 제외)
├── release/                     # 패키징 산출물 (Git 제외)
└── build/                       # 빌드 메타데이터
```

## Canonical Documents (정규 문서 4종)

프로젝트에 반드시 존재해야 하는 4개의 정규 문서:

### 1. ARCHITECTURE.md

```markdown
# {프로젝트명} — Architecture

## Tech Stack
| 영역 | 기술 | 선택 근거 |
|------|------|-----------|
| Runtime | {Node/Electron/...} | {이유} |
| Frontend | {React/Vue/...} | {이유} |
| State | {Zustand/Redux/...} | {이유} |
| Build | {Vite/Webpack/...} | {이유} |

## Process Architecture
{메인/렌더러/워커 등 프로세스 간 관계 다이어그램}

## Data Flow
{사용자 입력 → 상태 관리 → 저장 경로}

## Security Boundary
{신뢰 영역 vs 샌드박스 영역}

## Design Decisions
| 결정 | 선택 | 대안 | 선택 근거 |
|------|------|------|-----------|
```

### 2. STYLEGUIDE.md

```markdown
# {프로젝트명} — Style Guide

## TypeScript
- strict 모드 활성화
- any 사용 금지
- 인터페이스 I 접두어 금지 (ex: User, not IUser)

## React
- 함수 컴포넌트만 사용
- PascalCase 파일명
- Props 인라인 정의

## Naming
| 대상 | 규칙 | 예시 |
|------|------|------|
| 컴포넌트 | PascalCase | CanvasNode.tsx |
| 훅 | camelCase + use 접두어 | useAutoSave.ts |
| 유틸 | camelCase | formatDate.ts |
| 타입 | PascalCase | NodeData |
| 상수 | UPPER_SNAKE | MAX_RETRY |

## Commit Message
`v{VER}: {변경 요약}`
또는 실험 추적 시:
`[{PROJECT}] S{P}.{S} — {brief} · REQ-XXX · SC-NN · EXP-NNN`

## Forbidden
- console.log in production
- @ts-ignore
- node_modules commit
- plaintext secrets
```

### 3. TESTING.md

→ BP_TESTING.md 참조

### 4. REPO_MAP.md

```markdown
# {프로젝트명} — Repository Map

## Source Structure
{src/ 하위 폴더별 역할 설명}

## Runtime Data
{앱이 생성하는 데이터 파일의 경로와 형식}

## Build Output
{dist/, release/ 등 빌드 산출물 설명}

## Configuration Files
{tsconfig, vite.config, .env 등 설정 파일 설명}
```

## 디렉토리 명명 규칙

| 폴더 | 용도 | 네이밍 |
|------|------|--------|
| components/ | UI 컴포넌트 | 도메인별 하위 폴더 (canvas/, panel/, auth/) |
| services/ | 비즈니스 로직 | 기능별 단일 파일 (git-service.ts) |
| hooks/ | React 훅 | use 접두어 (useAutoSave.ts) |
| stores/ | 상태 관리 | 도메인명 (canvasStore.ts) |
| utils/ | 순수 유틸 | 기능명 (formatDate.ts) |
| types/ | 타입 정의 | 도메인명 또는 shared (types.ts) |
| shared/ | 프론트/백 공유 | 공통 타입, 유틸 |

## .gitignore 필수 항목

```gitignore
node_modules/
dist/
release/
*.enc          # 암호화된 크레덴셜
.env           # 환경 변수
*.log
.DS_Store
Thumbs.db
```

## TCC 프로젝트 실제 사례

- `src/main/services/` 에 13개 마이크로서비스를 도메인별 분리
- `src/renderer/components/` 에 6개 도메인 폴더 (auth, canvas, editor, layout, panel, tree)
- `docs_canonical/` 4종 문서로 AI가 프로젝트를 즉시 파악
- `~/TCC-data/` 외부 데이터 디렉토리로 앱 삭제 시에도 데이터 보존
