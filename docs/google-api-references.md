# 구글 API 가이드

본 문서는 학습에 앞서 구글 API를 얻고 사용하는 방법에 관한 문서입니다.

## 1. 구글 계정 신규 생성

* **URL:** [https://accounts.google.com/](https://accounts.google.com/)
* **필요 정보:**
    * 계정 이름
    * 계정 이메일
    * 복구 이메일 혹은 전화번호

## 2. Gemini API Key 발급

1. **API Key 발급 페이지 접속:**
    * [https://aistudio.google.com/apikey](https://aistudio.google.com/apikey)
2. **API Key 생성 및 복사:**
    * "API 키 만들기"를 통해 Key를 생성하고 복사합니다. (이 정보는 Caret의 Gemini API 설정 등에 사용됩니다.)

### 2.1. Google Cloud 무료 크레딧 (선택 사항)

* Google Cloud Console에서 "$300 무료 크레딧" 제공 페이지를 확인합니다.
    * **필요 정보:** 개인 신용카드, 주소 등
    * **확인 사항:** "바로 결제로 이동"하는 경우 크레딧이 제공되지 않을 수 있습니다. "$300(약 43만원)의 무료 크레딧으로 90일간 Google Cloud를 체험해 보세요."와 같은 문구가 표시되어야 합니다.

> **주의사항:** Google AI 스튜디오 API Key 페이지에서 일반 계정 활성화, 결제 계정 연결, 그리고 Tier 1으로의 변경까지 모두 완료되었는지 확인해야 무료 크레딧 사용이 가능합니다.

## 3. Blogger 가입 및 API 설정

### 3.1. Blogger 가입
1. [Blogger 웹사이트](https://www.blogger.com)에 접속하여 Google 계정으로 가입하고 새 블로그를 생성합니다.

### 3.2. Google Cloud Console에서 Blogger API 활성화
1. [Google Cloud Console](https://console.cloud.google.com/)에 접속합니다.
2. 프로젝트를 선택하거나 새 프로젝트를 생성합니다.
3. "API 및 서비스" > "라이브러리"로 이동합니다.
4. "Blogger API"를 검색하고 "사용 설정"합니다.
  https://console.cloud.google.com/marketplace/product/google/blogger.googleapis.com?q=search&referrer=search&inv=1&invt=AbyD4w&project=gen-lang-client-0557988161


### 3.3. OAuth 2.0 클라이언트 ID 설정
1. "API 및 서비스" > "사용자 인증 정보"로 이동합니다.
2. "+ 사용자 인증 정보 만들기" > "OAuth 클라이언트 ID"를 선택합니다.
3. **애플리케이션 유형**을 "웹 애플리케이션"으로 선택합니다.
4. **승인된 JavaScript 원본** 설정:
    * 로컬 개발 환경에서 테스트하기 위해 웹 애플리케이션이 실행되는 주소를 추가합니다.
    * 예시: `http://localhost:3000` (스크린샷 참고)
        * URI 1: `http://localhost:3000`
5. **승인된 리디렉션 URI** 설정:
    * OAuth 2.0 인증 후 Google이 사용자를 리디렉션할 애플리케이션의 경로를 추가합니다.
    * 이 URI는 FastAPI 애플리케이션에서 OAuth 콜백을 처리할 엔드포인트 경로와 일치해야 합니다.
    * 예시: `http://localhost:3000/oauth` (마스터께서 설정하신 값으로 업데이트 필요)
    * **참고:** `docs/development-guide.md`의 `app/routers/post_router.py` 설계 시 `/auth/google/callback` 경로를 사용할 예정입니다. 실제 구현 시 이 경로와 일치시키거나, Google Cloud Console의 리디렉션 URI 설정을 `http://localhost:3000/auth/google/callback`으로 변경해야 할 수 있습니다.
6. "만들기"를 클릭하면 클라이언트 ID와 클라이언트 보안 비밀이 생성됩니다.
    * 이 정보는 프로젝트 루트의 `.env` 파일에 `GOOGLE_CLIENT_ID`와 `GOOGLE_CLIENT_SECRET`으로 안전하게 보관해야 합니다.
7. "블로그" 만들기
   https://www.blogger.com/about/ 에서 만들기를 클릭하여 블로그를 만듭니다.

> **참고:** Google Cloud Console의 설정 변경사항이 시스템에 완전히 적용되기까지 몇 분에서 몇 시간까지 소요될 수 있습니다.

### 3.4. 블로그 ID 확인 방법

Blogger API를 사용하여 특정 블로그에 글을 게시하거나 정보를 가져오려면 해당 블로그의 고유 ID가 필요합니다.

1. **Blogger 대시보드 접속:**
    * [https://www.blogger.com](https://www.blogger.com) 에 접속하여 Google 계정으로 로그인합니다.
2. **해당 블로그 선택:**
    * ID를 확인하고자 하는 블로그를 선택하여 해당 블로그의 관리 페이지로 이동합니다. (예: 글 목록, 설정 등)
3. **URL에서 블로그 ID 확인:**
    * 브라우저의 주소창을 확인합니다. URL은 일반적으로 다음과 같은 형식 중 하나로 표시됩니다:
        * `https://www.blogger.com/blog/posts/{블로그_ID}`
        * `https://www.blogger.com/blogger.g?blogID={블로그_ID}#allposts`
        * `https://www.blogger.com/u/{숫자}/blog/page/edit/{블로그_ID}/{페이지_ID}`
        * `https://www.blogger.com/u/{숫자}/blog/settings/{블로그_ID}`
    * 위 형식에서 `{블로그_ID}` 부분에 해당하는 긴 숫자 문자열이 해당 블로그의 고유 ID입니다.
    * 예시: 마스터께서 알려주신 `https://www.blogger.com/u/5/blog/settings/7130095174209823806` 의 경우, 블로그 ID는 `7130095174209823806` 입니다.

이 블로그 ID는 API 요청 시 `blogId` 매개변수 값으로 사용됩니다.

## 5. 획득한 인증 정보 및 Blog ID를 `.env` 파일에 설정

위 과정들을 통해 얻은 Google Cloud 프로젝트의 **클라이언트 ID**, **클라이언트 시크릿**, 그리고 포스팅할 **Blogger의 Blog ID**는 실제 애플리케이션 코드에서 안전하게 사용하기 위해 환경 변수로 관리하는 것이 좋습니다.

1.  **`.env` 파일 준비:**
    *   프로젝트 루트 디렉토리에 `.env.template` 파일이 있다면, 이를 복사하여 같은 위치에 `.env` 파일을 생성합니다. (예: 터미널에서 `copy .env.template .env` 또는 `cp .env.template .env`)
    *   만약 `.env.template` 파일이 없다면, 직접 `.env` 파일을 생성합니다.
2.  **`.env` 파일에 정보 입력:**
    *   생성하거나 준비된 `.env` 파일을 열고, 다음과 같은 형식으로 앞에서 획득한 정보들을 입력합니다. (LLM API 키는 이 문서에서 다루지 않지만, 프로젝트에서 필요시 함께 관리합니다.)

        ```dotenv
        # .env 파일 예시
        LLM_API_KEY="여러분의_LLM_API_키_여기에_입력 (선택 사항, OpenAI 등 사용 시)"
        GOOGLE_CLIENT_ID="여기에_여러분의_클라이언트_ID를_입력하세요"
        GOOGLE_CLIENT_SECRET="여기에_여러분의_클라이언트_시크릿을_입력하세요"
        BLOG_ID="여기에_여러분의_Blogger_Blog_ID를_입력하세요"
        ```
    *   **`GOOGLE_CLIENT_ID`**: "OAuth 2.0 클라이언트 ID 설정" 단계에서 확인한 클라이언트 ID입니다.
    *   **`GOOGLE_CLIENT_SECRET`**: "OAuth 2.0 클라이언트 ID 설정" 단계에서 확인한 클라이언트 보안 비밀입니다.
    *   **`BLOG_ID`**: "블로그 ID 확인 방법" 섹션을 참고하여 확인한 여러분의 Blogger 블로그 ID입니다.
3.  **보안:**
    *   `.env` 파일은 민감한 정보를 담고 있으므로, 절대로 Git 저장소에 직접 커밋하거나 외부에 노출되지 않도록 주의해야 합니다. 프로젝트의 `.gitignore` 파일에 `.env`가 포함되어 있는지 확인하세요.

이렇게 `.env` 파일에 설정된 값들은 애플리케이션 실행 시 `app/core/config.py`와 같은 설정 파일을 통해 안전하게 로드되어 사용됩니다.
