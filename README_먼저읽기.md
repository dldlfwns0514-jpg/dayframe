# Dayframe — 설치 준비 패키지

이 폴더는 Dayframe을 클라우드 + AI + ChatGPT 연동으로 확장하기 위한 시작 프로젝트입니다.

## 들어 있는 것
- `public/index.html`: Supabase에 연결할 수 있는 기본 일정 화면
- `supabase/migrations/001_tasks.sql`: 일정 테이블 및 사용자별 접근 규칙
- `supabase/functions/dayframe-ai/index.ts`: 사이트의 AI 일정 비서 서버 함수
- `supabase/functions/dayframe-gpt/index.ts`: 커스텀 GPT Actions용 API 서버 함수
- `openapi-gpt-actions.yaml`: GPT 편집 화면에 넣을 API 설명서
- `SETUP_태블릿용.md`: 설정 순서

## 먼저 알아둘 점
이 패키지는 설정을 마친 뒤 배포할 수 있는 프로젝트 초안입니다. 압축을 푼 것만으로 온라인 서비스가 자동으로 만들어지는 것은 아닙니다. Supabase 프로젝트, OpenAI API 키, 배포 및 GPT Actions 설정이 필요합니다.

현재 프런트엔드는 Supabase 이메일 로그인과 일정 CRUD를 지원하는 기본 버전입니다. AI 채팅은 `dayframe-ai` 함수가 배포되고 API 키가 등록된 뒤 사용할 수 있습니다. ChatGPT 앱 연동은 별도의 커스텀 GPT에 Actions를 등록해야 합니다.

## 보안
- OpenAI API 키를 `public/index.html`에 넣지 마세요.
- Supabase `service_role`/secret key를 웹페이지에 넣지 마세요.
- GPT Actions에는 `dayframe-gpt` 전용 공유 키를 설정하세요.
- 이 버전은 개인 프로젝트용 시작점입니다. 공개 서비스로 배포하기 전에는 로그인, 키 보관, 요청 제한, 데이터 삭제/백업 등을 검토하세요.
