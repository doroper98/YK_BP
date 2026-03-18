# BP_VERSIONING — 버전 관리 & 릴리스 전략

> 빌드 = 버전 업 = DEVLOG 기록. 이 세 가지는 항상 원자적으로 수행합니다.

## Semantic Versioning

```
MAJOR.MINOR.PATCH
  │      │      │
  │      │      └── 버그 수정, 미세 조정
  │      └── 기능 추가, UI 개선
  └── 대규모 변경, 첫 정식 릴리스, 호환성 변경
```

### 버전 업 기준

| 변경 유형 | 버전 업 | 예시 |
|-----------|---------|------|
| 오타 수정, CSS 미세 조정 | PATCH | 0.4.5 → 0.4.6 |
| 새 기능 추가 | MINOR | 0.4.6 → 0.5.0 |
| UI 전면 개편 | MINOR | 0.3.7 → 0.4.0 |
| 첫 정식 릴리스 | MAJOR | 0.4.6 → 1.0.0 |
| 호환성 깨지는 변경 | MAJOR | 1.2.3 → 2.0.0 |

## 원자적 버전 관리 절차

하나의 버전 업에는 반드시 3가지가 동시에 수행:

```
1. package.json version 업데이트
2. DEVLOG.md 개선 로그 추가 (테이블 형식)
3. git commit -m "v{X.Y.Z}: {변경 요약}"
```

### 수행 순서
```bash
# Step 1: package.json 버전 수정
# Step 2: DEVLOG.md에 개선 로그 추가
# Step 3: 커밋
git add package.json DEVLOG.md {변경된 파일들}
git commit -m "v0.4.5: Remove DevTools auto-open, apply glassmorphism to all panels"
```

## 개선 로그 형식

```markdown
### v0.4.5 — 2026-03-17 (패널 테마 통일)

| # | 개선 항목 | 변경 파일 | 변경 내용 |
|---|-----------|-----------|-----------|
| 1 | DevTools 자동 열기 제거 | main.ts | openDevTools() 삭제, F12 토글만 유지 |
| 2 | GitPanel 라이트 테마 | GitPanel.tsx | 다크 → glassmorphism 라이트 |
| 3 | SearchPanel 라이트 테마 | SearchPanel.tsx | 다크 → glassmorphism 라이트 |
```

## 커밋 메시지 형식

### 일반 개발 (Phase 진행 중)
```
[{PROJECT}] S{P}.{S} — {brief} · REQ-XXX · SC-NN · EXP-NNN
```

### 버전 릴리스
```
v{X.Y.Z}: {1줄 변경 요약}
```

### 복합 변경
```
v{X.Y.Z}: {주요 변경} + {부수 변경}
```

예시:
```
v0.4.0: Full UI redesign — light theme, glassmorphism, Pretendard font
v0.4.4: Fix high-DPI blur + auto-resize meeting notes
v1.0.0: First official release — exe distribution
```

## Git 브랜치 전략 (소규모 프로젝트)

```
master ─── 모든 작업을 직접 커밋
            │
            ├── v0.1.0
            ├── v0.2.0
            ├── ...
            └── v1.0.0 ← exe 배포
```

소규모 AI 협업 프로젝트에서는 **master 단일 브랜치**로 충분합니다.
- AI가 매번 CLI Gate를 통과하므로 broken 커밋이 없음
- feature 브랜치는 팀 규모가 커질 때 도입

## 릴리스 절차

```
1. 모든 변경 사항 구현 완료
2. CLI Gate 최종 통과
3. package.json 버전 업
4. DEVLOG 개선 로그 기록
5. git commit + push
6. 패키징 (electron-builder, npm publish 등)
7. 산출물 확인 (exe 설치 테스트 등)
```

### Electron 패키징 설정 (electron-builder)
```json
{
  "build": {
    "appId": "com.{author}.{project}",
    "productName": "{Project Name}",
    "directories": { "output": "release" },
    "files": ["dist/**/*", "node_modules/**/*"],
    "win": {
      "target": [{ "target": "nsis", "arch": ["x64"] }]
    },
    "nsis": {
      "oneClick": false,
      "allowToChangeInstallationDirectory": true,
      "createDesktopShortcut": true,
      "createStartMenuShortcut": true,
      "shortcutName": "{앱 약칭}"
    },
    "asar": true
  }
}
```

### 빌드 스크립트
```json
{
  "scripts": {
    "pack": "npm run build && npm run build:electron && electron-builder --dir",
    "dist": "npm run build && npm run build:electron && electron-builder"
  }
}
```

## 버전 히스토리 보존 규칙

1. **절대 삭제 금지** — 과거 버전 기록은 절대 삭제하지 않음
2. **최신 우선** — 가장 최근 버전이 맨 위
3. **테이블 형식** — 일관된 형식으로 변경 파일과 내용 기록
4. **날짜 포함** — 각 버전에 날짜 기록

## TCC 프로젝트 실적

- v0.1.0 → v1.0.0까지 20+ 버전 누적 관리
- 모든 버전의 변경 이력이 DEVLOG에 완전 보존
- 원자적 버전 관리 (package.json + DEVLOG + commit 동시 수행) 100% 준수
- electron-builder로 Windows exe 인스톨러 자동 생성
