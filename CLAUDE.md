# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 개요

스포츠 MBTI — 16문항 성격 검사로 동·하계 올림픽 20개 종목 중 적합한 스포츠를 추천하는 체육 수업용 도구.
배포 주소: https://Kimjs99.github.io/sports-mbti/

## 아키텍처

**단일 HTML 파일 앱** — 모든 로직이 `index.html` 하나에 담겨 있습니다. 빌드 도구, package.json, node_modules 없음.

- **런타임**: CDN(UMD)으로 로드하는 React 18 — 번들링 없음
- **언어**: JavaScript (JSX를 Babel CLI로 사전 컴파일한 결과물 — 원본 `.jsx` 파일은 저장소에 없음)
- **스타일**: CSS-in-JS (인라인 스타일) + `<style>` 태그 내 전역 CSS 애니메이션
- **차트**: SVG로 직접 구현한 레이더 차트 (별도 차트 라이브러리 없음)
- **다운로드**: CDN으로 로드한 html2canvas + jsPDF (PNG/PDF 내보내기)

### 앱 흐름 (4단계 화면, `useState`로 관리)

```
랜딩 → 인적사항 입력(학번/이름) → 16문항 퀴즈 → 결과(추천 종목 + 레이더 차트)
```

### `index.html` 스크립트 블록 섹션 구조

| 섹션 주석 | 내용 |
|-----------|------|
| `/* DATA */` | `SPORTS` 배열 — 20개 종목의 트레이트 프로필 |
| _(주석 없음)_ | `QUESTIONS` 배열 — 16개 문항 및 트레이트 가중치 |
| `/* CALC */` | `calcResult()` — 정규화 및 유클리드 거리 계산 |
| `/* STYLES */` | `S` 컴포넌트 — React로 주입하는 전역 CSS |
| `/* COMPONENTS */` | `Blob`, `Rings`, `Pgbar`, `Radar` 등 서브 컴포넌트 |
| _(APP 섹션)_ | 메인 `App` 컴포넌트 — 4개 phase별 화면 렌더링 |

### 점수 산출 알고리즘

16개 문항의 답변마다 6개 트레이트(`teamwork`, `explosive`, `endurance`, `precision`, `risk`, `winter`)에 가중치 점수를 누적합니다. 원점수를 0~10으로 정규화한 뒤, 20개 종목 각각의 트레이트 프로필과 유클리드 거리를 계산해 거리가 가장 짧은 종목을 최종 결과로 출력합니다.

## 로컬 실행

별도 설치 없이 바로 실행 가능합니다.

```bash
open index.html              # macOS
python -m http.server 8080   # 이후 http://localhost:8080 접속
```

## 파일 수정 시 주의사항

`index.html`의 스크립트 블록은 Babel로 컴파일된 JS입니다 (원본 JSX 소스 없음). 수정은 `index.html` 내 스크립트 블록을 직접 편집해야 합니다. 위 섹션 주석을 기준으로 위치를 파악하세요.

JSX 소스에서 재컴파일이 필요한 경우:

```bash
npx @babel/cli --presets @babel/preset-react,@babel/preset-env src.jsx -o out.js
# 출력된 JS를 index.html의 <script> 블록에 붙여넣기
```
