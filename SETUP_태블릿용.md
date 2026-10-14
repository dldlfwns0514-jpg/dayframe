# Dayframe 설정 순서 (태블릿 초보자용)

가능하면 태블릿에서 브라우저의 '데스크톱 사이트' 기능을 켜면 설정 화면이 편할 수 있습니다. 이 프로젝트는 파일만으로 온라인에 배포되지 않으므로 아래 설정이 필요합니다.

## 1. Supabase 프로젝트 만들기
1. https://supabase.com 에서 계정을 만들고 새 프로젝트를 생성합니다.
2. 데이터베이스 비밀번호는 안전한 곳에 보관합니다.
3. 프로젝트의 SQL Editor를 엽니다.
4. `supabase/migrations/001_tasks.sql` 파일의 전체 내용을 복사해 SQL Editor에 붙여넣고 실행합니다.
5. Project Settings → API에서 Project URL과 publishable key를 확인합니다. secret/service_role 키는 웹페이지에 넣지 않습니다.

## 2. 사이트 연결 설정
1. `public/index.html`을 편집할 수 있는 도구에서 엽니다.
2. 파일 아래쪽 `SUPABASE_URL_HERE`와 `SUPABASE_PUBLISHABLE_KEY_HERE`를 프로젝트 값으로 바꿉니다.
3. 이 파일은 로컬에서 미리 볼 수 있지만, 다른 기기에서도 쓰려면 HTTPS 웹 호스팅에 배포해야 합니다.
4. Supabase Auth 설정에서 이메일 로그인을 활성화하고 필요하면 이메일 확인 설정을 마칩니다.

## 3. AI 서버 함수
`supabase/functions/dayframe-ai/index.ts`를 Supabase Edge Function `dayframe-ai`로 배포합니다.
서버 함수의 Secrets에 `OPENAI_API_KEY`, `SUPABASE_URL`, `SUPABASE_PUBLISHABLE_KEY`를 추가합니다. 프로젝트의 publishable key를 사용하고 secret/service_role 키는 AI 함수에 넣지 않습니다. OpenAI API 키는 사이트 HTML에 절대 넣지 않습니다.
Supabase 대시보드에서 함수 배포 기능을 사용할 수 있습니다. CLI 배포는 컴퓨터 환경이 더 편하므로, 태블릿만 쓸 경우 대시보드의 편집/배포 지원 여부를 먼저 확인하세요.

## 4. ChatGPT 앱 연동
1. Supabase Edge Function `dayframe-gpt`를 배포합니다.
2. 서버 Secrets에 `DAYFRAME_GPT_API_KEY`, `SUPABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY`, `DAYFRAME_OWNER_USER_ID`를 설정합니다. `DAYFRAME_OWNER_USER_ID`는 사이트에서 가입한 사용자 ID이며, `SUPABASE_SERVICE_ROLE_KEY`는 서버에만 보관해야 합니다.
3. `DAYFRAME_GPT_API_KEY`에는 임의의 길고 무작위인 비밀 값을 사용합니다.
4. 함수 URL은 `https://YOUR_PROJECT_REF.supabase.co/functions/v1/dayframe-gpt` 형식입니다.
5. 커스텀 GPT 편집 화면에서 Actions를 추가하고 `openapi-gpt-actions.yaml` 내용을 붙여넣습니다.
6. 서버 주소의 `YOUR_PROJECT_REF`를 실제 프로젝트 ID로 바꿉니다.
7. Authentication을 API Key 방식으로 설정하고 헤더 이름을 `x-api-key`로 지정한 뒤 같은 `DAYFRAME_GPT_API_KEY` 값을 넣습니다.
8. GPT Actions는 기능과 계정/워크스페이스 설정에 따라 이용 가능 여부가 달라질 수 있습니다.

## 5. 테스트 순서
- 사이트에서 가입/로그인
- 일정 하나 추가 → 새로고침 → 일정이 남는지 확인
- AI 채팅으로 “내 일정을 보여 줘” 또는 “내일 오후 7시에 수학 공부 1시간 추가해 줘” 테스트
- 커스텀 GPT에서 “오늘 일정을 보여 줘” 테스트

## 6. 아직 별도로 필요한 것
- 안정적인 공개 호스팅 주소
- 푸시 알림/예약 알림 서비스
- GPT Actions를 통한 인증과 개인 데이터 접근 테스트
- 백업/복구와 삭제 정책

ChatGPT 앱의 일반 알림이 이 사이트의 모든 일정 알림을 자동으로 대신해 주는 것은 아닙니다. 실제 시간 알림은 별도의 알림 시스템을 연결해야 합니다.
