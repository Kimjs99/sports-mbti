# Changelog

All notable changes to this project will be documented in this file.

## [v0.1.0] - 2026-03-05

### ✨ Features
- 모든 페이지 하단에 저작권 문구 및 버전 표시 추가 (`8a8a9e2`)
- PNG/PDF 다운로드 개선 — 그래프 포함 및 폰트 렌더링 수정 (`0dcdeac`)
  - `cardRef` 범위를 결과 카드 + 레이더 차트 모두 포함하도록 확장
  - `document.fonts.ready` 대기로 한글 폰트 깨짐 해결
  - `onclone` 콜백에서 SVG 크기 명시로 그래프 캡처 수정
  - `allowTaint: true` 추가

### 🔧 Chores
- 초기 파일 업로드 및 정리 (`3fd9a40`, `a14be0e`, `15c6a24`, `41c1970`, `b22ba08`, `c2aef13`)
