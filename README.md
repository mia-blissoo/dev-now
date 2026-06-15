# DEV NOW

BLISSOO 개발팀(JISOO APP 담당) 내부 프로젝트 현황판.
C레벨·타팀이 개발팀 현재 진행 상황과 실험(POC) 결과물을 별도 보고 없이 바로 확인하는 모바일 웹.

## 진입

- 본문: https://dev-now-mia-5388s-projects.vercel.app/dev-launchpad.html
- 상태: Private (Vercel Authentication, 로그인 필요)

## 구성

| 파일 | 내용 |
|---|---|
| `dev-launchpad.html` | 단일 파일 앱 (HTML/CSS/Vanilla JS, ~80KB, 1500줄) |
| `vercel.json` | Vercel 배포 설정 (cleanUrls, `/` → `/dev-launchpad.html` rewrite) |
| `docs/prd-v1.md` | V1 PRD (이미 배포된 기능 명세) |
| `docs/prd-v2.html` | **V2 PRD (지금 진행 중)** — Google OAuth, 권한 모델, 슬랙 자동 연동, 별표 등 |

## 탭 구조

- **DEV NOW**: JISOO APP 개발 진행 현황. 간트차트(주축) + 카드 토글
- **POC**: 개발팀이 본 제품에 들어가기 전 빠르게 검증한 실험 결과물 (Proof of Concept)
- **Daily Enter**: K-엔터 뉴스·이슈 모니터링 (외부 링크 `k-ent-pulse.vercel.app`)

## 데이터 구조 (V1)

코드 상단 두 객체로만 구동. 데이터만 갈아끼우면 화면 갱신.

```js
PRODUCTS = [{ id, name, color }]
TASKS = [{
  name, product, pjt, dev, note, deadline, start, end,
  lifecycle: 'active' | 'on_hold' | 'cancelled',
  phases: [{ name, status, start, end, type?: 'milestone' }],
}]
POCS = [{ name, desc, tag, link, created, updated, assignees?, images? }]
```

저장: 브라우저 `localStorage` (V1) → V2에서 DB로.

## V2 변경 요약 (PRD 참조)

- **인증**: Google OAuth + 가입 승인 워크플로우 (뷰어/편집자/관리자)
- **랜딩**: 비로그인 시 confidential 강한 블러
- **상세**: 카드 클릭 → 바텀시트 대신 모달 큰 창
- **데이터 소스**: 텍스트 콘텐츠는 슬랙 List(F0ASC4WB3V4) 자동 연동 (매일 daily sync, 슬랙이 source of truth)
- **별표**: 카드별 "부회장님 확인 필요" 토글 (연보라, 편집권한자만)

상세는 `docs/prd-v2.html` 참조.

## 기술 스택

- 프레임워크 없음. 순수 HTML/CSS/Vanilla JS 단일 파일
- 외부 라이브러리: Pretendard 폰트, SortableJS 1.15.2 (드래그앤드롭) — 둘 다 CDN
- 빌드 없음 (Vercel이 정적 파일 그대로 서빙)
- 호스팅: Vercel (Hobby 플랜)
