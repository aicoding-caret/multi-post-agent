# 태스크-003: blogger-api-연동-및-oauth-인증 (blogger-api-and-oauth-integration)

**id:** 003
**태스크-명칭:** blogger-api-연동-및-oauth-인증-구현
**상태:** to-do
**담당자:** ai-어시스턴트 (Caret)
**관련-문서:**
* `docs/development-guide.md` (섹션 3. 백엔드 설계 (FastAPI) - `app/services/blogger_service.py`, `app/routers/post_router.py` 부분)
* `docs/google-api-references.md` (Blogger API 설정 및 OAuth 2.0 클라이언트 ID 관련)
* `docs/requirement.md` (섹션 2.2. 주요 기능 - Blogger API 연동)
* `app/core/config.py` (Google Client ID, Secret 접근)

## 1. 목표 (goal)

Google Blogger API를 사용하여 콘텐츠를 포스팅할 수 있도록 OAuth 2.0 인증 흐름을 구현하고, 획득한 인증 정보를 파일에 저장하여 재사용할 수 있도록 하며, 관련 서비스 및 라우터 로직을 작성합니다.

## 2. 체크리스트 (checklist)

- [ ] `app/services/blogger_service.py` 파일 생성 및 기본 구조 작성.
- [ ] Google API Python 클라이언트 라이브러리를 사용하여 Blogger API 서비스 객체를 생성하는 함수 구현.
    * `googleapiclient.discovery.build` 사용.
- [ ] OAuth 2.0 인증 흐름 시작 로직 구현:
    * `google_auth_oauthlib.flow.Flow` 인스턴스 생성 (client_id, client_secret, scopes, redirect_uri 사용).
    * `authorization_url()`을 사용하여 인증 URL 생성.
- [ ] `app/routers/post_router.py`에 `/auth/google` GET 엔드포인트 추가:
    * `blogger_service`의 인증 URL 생성 함수 호출.
    * 사용자를 Google 인증 페이지로 리디렉션 (`RedirectResponse`).
- [ ] `app/routers/post_router.py`에 `/auth/google/callback` GET 엔드포인트 추가:
    * Google로부터 받은 인증 코드를 사용하여 액세스 토큰 요청 및 저장 로직 구현 (`flow.fetch_token(code=...)`).
    * 획득한 인증 정보(credentials)를 JSON 파일로 안전하게 저장 (예: `.credentials/default_user_oauth.json`, `.gitignore` 처리 포함).
    * 인증 성공 시: "Blogger 인증이 완료되었습니다." 메시지를 메인 페이지로 전달하며 리디렉션. 인증 실패 시: "Blogger 인증에 실패했습니다." 메시지를 메인 페이지로 전달하며 리디렉션.
- [ ] `app/services/blogger_service.py`에 새 글을 포스팅하는 함수 구현:
    * `service.posts().insert(blogId=..., body=..., isDraft=False).execute()`
    * `body` 매개변수에는 포스트 제목과 HTML 형식의 콘텐츠가 포함되어야 함.
- [ ] `.gitignore` 파일에 인증 정보 저장 경로(예: `.credentials/`) 추가.
- [ ] `app/services/blogger_service.py`에 인증 정보 파일 저장(`save_credentials_to_file`) 및 로드(`load_credentials_from_file`) 함수 구현.
- [ ] `app/services/blogger_service.py`의 `get_blogger_service` 함수에서 파일로부터 인증 정보 로드 및 토큰 갱신 시 파일 재저장 로직 추가.

## 3. 진행-상황-및-특이사항 (progress-and-notes)

* (여기에 진행하면서 발생하는 특이사항이나 결정 사항 등을 기록합니다.)
* OAuth 콜백 URI는 Google Cloud Console에 등록된 것과 정확히 일치해야 합니다.
* Blogger API 사용을 위한 스코프(scope)를 정확히 설정해야 합니다 (예: `https://www.googleapis.com/auth/blogger`).
* 인증 정보 파일 저장 시, 파일 경로 및 `.gitignore` 설정을 주의해야 합니다.

## 4. 다음-태스크 (next-task)

* [태스크-004: 기본-프론트엔드-ui-구현](./004-basic-frontend-ui.md)
