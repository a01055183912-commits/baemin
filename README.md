# AI로 만드는 우리 가게 홍보 글쓰기 — 우리 가게 요청 문장 도우미

사장님이 가게 정보와 이번에 필요한 글을 입력하면, ChatGPT나 Claude에 붙여 넣을 요청 문장을 즉시 조합해주는 웹앱입니다.

이 앱은 AI API를 호출하지 않습니다. 입력값과 템플릿을 조합해 요청 문장을 만들 뿐이며, 사장님이 그 문장을 직접 복사해 원하는 AI 서비스에 붙여 넣습니다. 서버 저장·로그인·결제·자동 게시 기능이 없고, 브라우저 저장(localStorage 등)도 사용하지 않습니다. 새로고침하거나 창을 닫으면 입력이 사라집니다.

## 로컬 개발

```bash
npm install
npm run dev
```

`http://localhost:5173` 에서 확인합니다.

## 프로덕션 빌드 확인

```bash
npm run build
npm start
```

`npm start`는 `dist/`를 정적으로 서빙하는 최소 Express 서버(`server.js`)를 `PORT` 환경변수(기본 8080)로 실행합니다.

## Railway 배포

이 저장소는 Railway의 Nixpacks 빌더를 그대로 사용할 수 있도록 구성되어 있습니다 (`railway.json`).

1. Railway에서 새 프로젝트를 만들고 이 GitHub 저장소를 연결합니다.
2. 빌드 명령: `npm install && npm run build` (자동 적용)
3. 시작 명령: `npm start` (자동 적용, `server.js`가 `PORT` 환경변수를 사용)
4. 배포 후 Railway가 제공하는 도메인으로 접속해 확인합니다.

별도의 환경변수, 데이터베이스, 외부 API 키가 필요하지 않습니다.

## 구현 범위와 경계

- React 단일 파일 컴포넌트: `src/App.jsx` (UI, 기본 데이터, 템플릿 조합 함수, CSS 포함)
- AI API 호출, 키, 로그인, 결제, 자동 게시 없음
- 사용자 입력을 외부로 전송하지 않음 (외부 폰트·분석 도구 미사용)
- React state로만 입력 상태 유지 (localStorage/sessionStorage/IndexedDB/쿠키 미사용)
- `<form>` 태그 대신 버튼 onClick으로 처리

## 배포 전 검증 기록

- `npm run build` 성공, `npm start`(Express, `PORT` 환경변수) 정상 응답 확인
- Playwright로 360px 화면에서 골든 패스 직접 조작 검증: 소개서 4칸 입력 → 메뉴 설명 요청 생성 → 전체 복사 성공 → 요청 기록 저장 → 받은 글 고치기(담백하게) → 원문 속 금지 표현 감지
- 소개서 필드 개인정보 패턴(전화번호 등) 감지·해제, 소개서 백업 파일 다운로드→복원 라운드트립, 개인정보 포함/버전 불일치 백업 파일 거부 확인
- 리뷰 답변 선택 시 인스타그램 옵션 제외, 리뷰 미입력 시 요청 생성 버튼 비활성화 확인
- 미검증: 실제 Railway 계정 연결·배포, 30개 요청 예시 전체 개별 클릭, 크로스 브라우저, 실제 ChatGPT/Claude 응답 품질
