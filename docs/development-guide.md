# 개발 가이드: AI 콘텐츠 변환 및 Blogger 포스팅 에이전트

## 1. 개요

본 문서는 `docs/requirement.md`에 정의된 "AI 콘텐츠 변환 및 Blogger 포스팅 에이전트" 웹 애플리케이션 개발을 위한 AI(Caret) 참조용 기술 가이드입니다. 프로젝트 구조, 주요 모듈 설계, 핵심 로직 구현 방향 등을 포함합니다.

## 2. 프로젝트 설정 및 환경

* **프로젝트 루트 디렉토리 구조:**
    ```
    /multi-post-agent
    |-- /app # FastAPI 애플리케이션 코드
    | |-- /routers # API 엔드포인트 라우터
    | | |-- post_router.py
    | |-- /services # 비즈니스 로직 및 외부 서비스 연동
    | | |-- content_service.py # 콘텐츠 변환 로직 (LangGraph 호출)
    | | |-- blogger_service.py # Blogger API 연동
    | |-- /models # Pydantic 모델 (데이터 유효성 검사)
    | | |-- post_models.py
    | |-- /core # 핵심 설정, 유틸리티
    | | |-- config.py # 환경 변수 및 설정 로드
    | |-- /graph # LangGraph 관련 모듈
    | | |-- state.py # LangGraph 상태 정의
    | | |-- nodes.py # LangGraph 노드 정의
    | | |-- builder.py # LangGraph 그래프 빌드
    | |-- main.py # FastAPI 애플리케이션 인스턴스
    |-- /static # 정적 파일 (CSS, JS, 이미지)
    | |-- style.css
    | |-- script.js
    | |-- app_title.jpg # 로고 이미지 (resources에서 복사)
    |-- /templates # HTML 템플릿 (Jinja2)
    | |-- index.html
    |-- .env # 환경 변수 파일 (API 키 등) - .gitignore에 추가
    |-- requirements.txt # Python 의존성
    |-- README.md
    |-- docs/
        |-- requirement.md
        |-- page-design.md
        |-- development_guide.md (본 문서)
        |-- google-api-references.md
        |-- google-blogger-api-guide.md
    ```
* **가상 환경 설정:** `python -m venv venv` 및 `source venv/bin/activate` (또는 Windows: `venv\Scripts\activate`)
* **의존성:** FastAPI, Uvicorn, Langchain, Langgraph, Google API Client 등 (`requirements.txt` 관리)
* **`.env` 파일 설정:**
    ```
    GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
    GOOGLE_CLIENT_ID="YOUR_GOOGLE_CLIENT_ID"
    GOOGLE_CLIENT_SECRET="YOUR_GOOGLE_CLIENT_SECRET"
    # 필요한 경우 추가 설정
    ```

## 3. 백엔드 설계 (FastAPI)

* **`app/main.py`:** FastAPI 앱 인스턴스화, 라우터 포함, 정적/템플릿 디렉토리 설정.
* **`app/core/config.py`:** `pydantic.BaseSettings`를 활용하여 `.env` 파일로부터 환경 변수 로드 및 관리.
* **`app/models/post_models.py`:** API 요청/응답 데이터 구조 정의 (Pydantic 모델).
    * `PostCreateRequest`: 제목, 내용, 처리 옵션(`auto`|`review`).
    * `PostResponse`: 변환된 콘텐츠, 상태, 포스팅 URL 등.
    * `ReviewRequest`: 사용자 검토 의견.
* **`app/routers/post_router.py`:** API 엔드포인트 정의.
    * `/` (GET): `index.html` 렌더링.
    * `/process_post` (POST): `content_service.process_content` 호출. (최초 변환 및 자동 개시 처리)
    * `/submit_review` (POST): `content_service.process_review` 호출. (사용자 의견 반영 및 최종 배포 처리)
    * `/auth/google` (GET), `/auth/google/callback` (GET): Google OAuth 처리.
* **`app/services/blogger_service.py`:** Blogger API 연동 로직. (참고: [Blogger API 가이드](./google-blogger-api-guide.md))
    * **중요:** Blogger API를 사용하여 글을 생성(POST), 수정(PUT/PATCH), 삭제(DELETE)하는 작업에는 **반드시 OAuth 2.0을 통한 사용자 인증이 필요**합니다. API 키만으로는 이러한 쓰기 권한을 가진 요청을 수행할 수 없습니다. (읽기 작업은 API 키로 가능할 수 있음)
    * Google API 클라이언트 초기화.
    * OAuth 2.0 인증 토큰 관리:
        * 최초 인증 시 사용자 동의를 통해 **액세스 토큰(Access Token)**과 **리프레시 토큰(Refresh Token)**을 발급받습니다.
        * 발급받은 인증 정보(토큰 포함)는 JSON 형식으로 로컬 파일 시스템의 안전한 위치(예: 프로젝트 루트의 `.credentials/default_user_oauth.json`, `.gitignore`에 추가됨)에 저장되어 서버 재시작 후에도 재사용됩니다.
        * 액세스 토큰은 유효 기간이 짧으므로(보통 1시간), 만료 시 저장된 리프레시 토큰을 사용하여 새로운 액세스 토큰을 자동으로 재발급받고, 갱신된 인증 정보를 다시 파일에 저장하는 로직이 구현되어 있습니다.
        * 리프레시 토큰은 장기간 유효합니다.
    * Blogger 포스트 생성/수정 API 호출 함수 구현.
* **`app/services/content_service.py`:** 핵심 비즈니스 로직.
    * `process_content`: `/process_post` 요청 처리. LangGraph 워크플로우를 실행하여 초기 콘텐츠 변환 또는 자동 포스팅을 수행하고, "검토 후 개시"의 경우 변환된 내용과 함께 사용자 검토가 필요함을 알리는 상태를 반환합니다.
    * `process_review`: `/submit_review` 요청 처리. 사용자 피드백을 반영하여 LangGraph를 통해 콘텐츠를 재수정하거나 최종 포스팅을 수행합니다.

## 4. AI 워크플로우 설계 (LangGraph)

* **`app/graph/state.py`:** `TypedDict`를 사용한 `GraphState` 정의.
    * 입력 데이터 (제목, 내용, 옵션), 중간 결과 (변환된 내용, 사용자 의견), 최종 결과 (포스팅 URL, 오류 메시지), 상태 플래그 (예: `is_review_needed`, `current_iteration`) 등을 포함하여 각 단계의 상태를 명확히 관리합니다.
* **`app/graph/nodes.py`:** LangGraph의 각 처리 단계를 노드로 구현.
    * `transform_content_node`: Google Gemini API (`gemini-2.5-flash-preview-05-20` 모델) 호출하여 초기 콘텐츠 스타일 변환.
    * `post_to_blogger_node`: `blogger_service`를 사용하여 Blogger에 포스팅.
    * `request_user_review_node`: 사용자 검토가 필요함을 상태(`is_review_needed=True`)에 표시하고, 현재까지 변환된 내용을 반환하기 위해 그래프 실행을 일단 종료시키는 역할.
    * `revise_content_node`: 사용자 의견을 반영하여 LLM으로 콘텐츠 재변환.
    * `should_post_or_review_node` (Conditional Edge용 함수): `/process_post` API 요청 시, 사용자의 처리 옵션('auto' 또는 'review')에 따라 `post_to_blogger_node` 또는 `request_user_review_node`로 분기합니다.
    * `should_revise_or_post_final_node` (Conditional Edge 또는 서비스 로직 내 판단 함수): `/submit_review` API 요청 시, 사용자 피드백 유무 및 최종 배포 여부에 따라 `revise_content_node` 또는 `post_to_blogger_node`로 분기합니다.
    * `handle_error_node`: 예외 발생 시 로깅 및 오류 상태 설정.
* **`app/graph/builder.py`:** `create_graph` 함수.
    * `StateGraph` 인스턴스 생성.
    * 노드 추가 (`add_node`).
    * 엣지 연결 (`add_edge`, `add_conditional_edges`). "검토 후 개시" 흐름을 위해 `request_user_review_node` 이후에는 `END`로 연결하여 중간 결과를 반환하도록 설계합니다.
    * 진입점/종료점 설정.
    * 그래프 컴파일 (`compile()`).

## 5. 프론트엔드 설계 (HTML, CSS, JavaScript)

* **`templates/index.html`:** (`docs/page-design.md` 기반)
    * Jinja2 템플릿 엔진 사용.
    * 사용자 입력 폼 (제목, 내용, 처리 옵션).
    * 결과 표시 영역 (상태 메시지, 변환된 콘텐츠, 포스팅 URL 링크 등).
    * Blogger 인증 관련 UI 요소.
* **`static/style.css`:** 기본 UI 스타일.
* **`static/script.js`:**
    * Fetch API를 사용한 비동기 백엔드 통신 (API 응답에 따른 성공/실패 메시지 및 결과 데이터(예: 포스팅 URL)를 UI에 명확히 반영하는 로직 포함).
    * DOM 조작을 통한 UI 업데이트 (상태 메시지, 변환된 콘텐츠, 검토 UI, 포스팅 URL 등).
    * OAuth 인증 흐름 트리거.

## 6. 주요 기술적 고려사항

* **비동기 프로그래밍:** FastAPI의 `async` 및 `await`를 사용하여 외부 API 호출 등 I/O 집약적 작업의 효율적 처리.
* **오류 처리 및 예외 관리:** 각 계층(라우터, 서비스, LangGraph 노드)에서 발생 가능한 예외를 적절히 처리하고, 일관된 방식으로 오류 응답 구성.
* **보안:**
    * API 키 및 민감 정보는 `.env` 파일을 통해 주입하고, 버전 관리에서 제외.
    * OAuth 인증 토큰 정보는 `.credentials/` 디렉토리 내 파일에 저장되며, 이 경로는 `.gitignore`에 추가되어 버전 관리에서 제외됩니다.
    * CSRF 보호 (FastAPI 미들웨어 사용 고려).
    * 입력 데이터 유효성 검증 (Pydantic).
* **로깅:** Python `logging` 모듈을 사용하여 애플리케이션의 주요 실행 흐름, 오류, 외부 API 호출 정보 등을 기록.

### 6.1. API 테스트 및 로컬 개발 환경 관련 팁

* **Windows PowerShell에서 `Invoke-WebRequest` (또는 `curl` 별칭) 사용 시 주의사항:**
    * **명령어 구문:** Linux/macOS의 `curl`과 명령어 옵션이 다릅니다.
        * HTTP 메소드 지정: `-Method POST` (Linux `curl`의 `-X POST`에 해당)
        * 헤더 지정: `-Headers @{"Header-Name"="Header-Value"}` (Linux `curl`의 `-H "Header-Name: Header-Value"`에 해당)
        * 요청 본문 지정: `-Body '{"key":"value"}'` (Linux `curl`의 `-d '{"key":"value"}'`에 해당)
        * 여러 줄 명령어: 각 줄 끝에 백틱(`` ` ``)을 사용합니다. (Linux `curl`의 백슬래시 `\`에 해당)
    * **문자 인코딩 (특히 한글 등 비 ASCII 문자 전송 시):**
        * PowerShell은 내부적으로 문자열을 UTF-16LE 등으로 다룰 수 있으며, 이로 인해 외부 API가 기대하는 UTF-8 인코딩과 불일치하여 한글 등이 깨지는 문제가 발생할 수 있습니다.
        * `Invoke-WebRequest`로 JSON 본문을 전송할 때, 문자열을 명시적으로 UTF-8 바이트 배열로 변환하여 `-Body` 매개변수에 전달하고, `-ContentType "application/json; charset=utf-8"` 헤더를 설정하는 것이 안전합니다.
            ```powershell
            # 예시: PowerShell에서 UTF-8로 JSON 본문 전송
            $bodyJson = '{"title": "테스트 제목", "content": "테스트 내용입니다."}'
            $utf8Bytes = [System.Text.Encoding]::UTF8.GetBytes($bodyJson)
            Invoke-WebRequest -Uri "YOUR_API_ENDPOINT" \`
              -Method POST \`
              -ContentType "application/json; charset=utf-8" \`
              -Headers @{"Authorization"="Bearer YOUR_ACCESS_TOKEN"} \`
              -Body $utf8Bytes
            ```
    * **OAuth 2.0 테스트 토큰 발급:**
        * Google API 등 OAuth 2.0 인증이 필요한 API를 테스트할 때는 [Google OAuth 2.0 Playground](https://developers.google.com/oauthplayground/)와 같은 도구를 사용하여 임시 액세스 토큰을 발급받을 수 있습니다.
        * Playground 사용 시, "OAuth 2.0 configuration" (톱니바퀴 아이콘)에서 "Use your own OAuth credentials"를 선택하고, `.env` 파일 등에 저장된 클라이언트 ID와 시크릿을 입력해야 올바른 애플리케이션 컨텍스트에서 토큰이 발급됩니다.
