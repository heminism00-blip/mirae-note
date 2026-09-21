# 미래노트 — MASTER PROMPT (빌드 명세)

> 재현·개선용 마스터 명세. 새 기능/수정 시 함께 갱신.

## 목적
투자 관련 영상·기사·뉴스를 담고 **요점·인사이트·실제 적용안**으로 구조화.
"적용 상태"로 자료 우선순위를 인식하고, 나중에 적용을 챙길 수 있게 한다.

## 기술 구조
- `index.html` 하나에 HTML+CSS(inline)+JS(vanilla). 외부 의존성 없음.
- 데이터: 브라우저 `localStorage` (키 `miraenote:v1`).
- **제미나이 API**(무료 키): 링크 넣고 자동정리 → generativelanguage.googleapis.com generateContent에 유튜브 URL(file_data.file_uri) + JSON 강제(responseMimeType). 키는 localStorage(miraenote:gk)에 저장(코드에 하드코딩 금지), 모델 GEMINI_MODEL, 맞춤맥락 MY_CONTEXT.
- 링크 메타(제목/썸네일): 유튜브 ID → `img.youtube.com/vi/ID/hqdefault.jpg`(썸네일),
  제목은 `noembed.com/embed?url=...`(CORS 가능). 실패 시 수동 입력.
- PWA: manifest.json + icon.png. 색 테마 = teal `#0d9488`.

## 데이터 모델 (노트)
`{ id, url, videoId?, title, thumb, cats:[], importance(0-3), status('apply'|'review'|'ref'),
   summary, insight, action, createdAt }`

## 화면
- 새 노트 폼: 링크+불러오기, 썸네일 미리보기, 제목, 카테고리(멀티칩), 중요도(별 3),
  3줄요점/인사이트/적용안, 적용상태(세그)
- 필터: 상태(전체/검토/적용/참고) + 분야(카테고리) + 검색
- 목록 카드: 썸네일+상태배지+별+제목+태그, 탭하면 상세(요점/인사이트/적용안)+열기/수정/삭제
- 정렬: 상태 우선순위(검토>적용>참고) → 중요도 → 최신

## 카테고리(기본)
반도체 · 금리 · 환율 · ETF · 심리 · 기타 (배열 CATS로 관리, 추가 쉬움)

## 로드맵
- 1단계(현재): 수동 정리 + 자동 제목/썸네일.
- 2단계(서버 필요): 키워드 기반 영상·뉴스 자동 수집 → 선별 → 메일/문자 발송.
  YouTube Data API + 뉴스 API + 발송 서비스(SendGrid/문자) + 상시 서버(비용) 필요.

## 공통 빌드 규칙
1. 앱 이름 + 아이콘(icon.png) + manifest → 홈 화면 앱화.
2. 버전업마다 CHANGELOG에 [요청·변경파일·변경내용] 추가, 커밋 메시지에 버전.
3. 파일명 index.html 고정.
