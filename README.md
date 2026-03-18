# YK_ Design System

12개의 독립적인 CSS 스타일 가이드 모음. 프로젝트에 CSS 파일 하나만 import하면 폰트, 색상, 그림자, 컴포넌트가 자동 적용됩니다.

## 사용법

```html
<link rel="stylesheet" href="YK_neu-bootstrap.css">
<body class="yk-body">
  <div class="yk-card">Hello World</div>
</body>
```

## 테마 목록

### Neumorphism (5종)
| 파일 | 성격 | 배경 |
|------|------|------|
| `YK_neu-bootstrap.css` | 클래식 뉴모피즘, Bootstrap 느낌 | Light (#e0e5ec) |
| `YK_ui-neumorphism.css` | 다크 뉴모피즘, React 라이브러리 스타일 | Dark (#292d35) |
| `YK_neumorphism-io.css` | 크림색 초부드러운 뉴모피즘 | Light (#eff0f4) |
| `YK_soft-ui-2026.css` | 2026 최신 Soft UI, Samsung 영감 | Light (#eceef1) |
| `YK_claymorphism.css` | 장난감 같은 3D, 밝은 회색 기반 | Light (#f2f3f7) |

### Modern Trends (3종)
| 파일 | 성격 | 배경 |
|------|------|------|
| `YK_neo-brutalism.css` | 두꺼운 테두리, 하드 셰도우, 대담한 색 | Light (#fef6e4) |
| `YK_soft-brutalism.css` | 부드러운 브루탈리즘, 파스텔 어스톤 | Light (#f5f0e8) |
| `YK_bento-grid.css` | Apple 영감 모듈형, 깔끔 미니멀 | Light (#f8f9fa) |

### CLI / Retro (4종)
| 파일 | 성격 | 배경 |
|------|------|------|
| `YK_hacker-terminal.css` | CRT 녹색 터미널, 스캔라인 | Dark (#0a0a0a) |
| `YK_claude-cli.css` | Anthropic 터미널, 보라/청록 | Dark (#1a1625) |
| `YK_windows-98.css` | Windows 98 3D 베벨, 회색 표면 | Teal (#008080) |
| `YK_bootstra-386.css` | 1980년대 DOS, 파란 배경, 픽셀 폰트 | Blue (#0000aa) |

## 공통 컴포넌트

모든 테마에서 동일한 클래스명을 사용합니다:

- `.yk-body` — 페이지 기본 스타일
- `.yk-card` — 카드 컨테이너
- `.yk-btn` / `.yk-btn-primary` / `.yk-btn-danger` / `.yk-btn-success` — 버튼
- `.yk-badge` / `.yk-badge-danger` / `.yk-badge-success` — 배지
- `.yk-input` / `.yk-select` / `.yk-textarea` — 입력 폼
- `.yk-alert` / `.yk-alert-danger` / `.yk-alert-success` — 알림 박스
- `.yk-metric` / `.yk-metric-label` / `.yk-metric-value` — 메트릭 카드
- `.yk-panel` / `.yk-panel-title` / `.yk-dot` — 패널
- `.yk-grid-2` / `.yk-grid-3` / `.yk-grid-4` — 그리드 레이아웃
- `.yk-table` — 테이블

## CSS 변수

각 테마에서 오버라이드 가능한 변수:

```css
--yk-bg          /* 페이지 배경색 */
--yk-surface     /* 카드/컴포넌트 배경색 */
--yk-text        /* 기본 텍스트 색 */
--yk-primary     /* 주 액센트 색 */
--yk-danger      /* 위험/에러 색 */
--yk-success     /* 성공 색 */
--yk-warning     /* 경고 색 */
--yk-radius      /* 기본 border-radius */
--yk-font        /* 기본 폰트 패밀리 */
```

## 데모

`YK_demo.html`을 브라우저에서 열면 12개 테마를 실시간으로 전환하며 비교할 수 있습니다.

## 라이선스

MIT
