# 프로젝트 회고 및 문서 개선 보고서 (1차 개발 사이클)

## 1. 서론

본 보고서는 "AI 콘텐츠 변환 및 Blogger 포스팅 에이전트" 프로젝트의 1차 개발 사이클을 완료한 후, 그 과정에서 발생했던 주요 이슈들과 해결 과정을 회고하고, 이를 바탕으로 향후 교육생들이 프로젝트를 처음부터 진행할 경우 더 원활하고 효과적으로 학습할 수 있도록 주요 프로젝트 문서(요구사항 명세서, 페이지 디자인, 개발 가이드, 태스크 문서 등)의 개선 방안을 제안하는 것을 목적으로 합니다.

본 보고서에서 제안되는 개선 사항들은 실제 개발 과정에서 AI 어시스턴트(Caret)와의 긴밀한 협업과 반복적인 테스트, 그리고 문제 해결 과정을 통해 도출되었습니다. 이는 처음부터 완벽한 문서를 만들기 어렵고, 개발 과정 중 지속적인 문서 업데이트와 현행화, 그리고 AI와의 명확한 소통 및 로깅이 매우 중요함을 시사합니다.

## 2. 1차 개발 사이클 주요 이슈 및 해결 과정 요약

1차 개발 사이클에서는 다음과 같은 주요 이슈들이 발생했으며, 이를 해결하는 과정을 거쳤습니다.

*   **환경 설정 및 의존성 문제**:
    *   **이슈**: `requirements.txt`에 일부 필수 라이브러리(Jinja2, google-auth-oauthlib, google-api-python-client, pydantic-settings, langgraph 등)가 누락되어 `ModuleNotFoundError` 또는 `ImportError` 발생.
    *   **해결**: 오류 발생 시점마다 필요한 라이브러리를 `requirements.txt`에 추가하고 `pip install -r requirements.txt`를 통해 설치.
*   **API 키 및 인증 설정 문제**:
    *   **이슈**:
        *   `.env` 파일의 API 키 변수명 불일치 (초기 `LLM_API_KEY`와 실제 사용하려던 Gemini API 키 변수명 혼동).
        *   Google Cloud Console에서 Blogger API 미활성화로 인한 403 오류.
        *   Google Cloud Console OAuth 동의 화면에서 테스트 사용자 미등록으로 인한 403 `access_denied` 오류.
    *   **해결**:
        *   `.env.template` 및 `app/core/config.py`의 API 키 변수명을 `GEMINI_API_KEY`로 통일.
        *   Google Cloud Console에서 Blogger API 활성화.
        *   Google Cloud Console OAuth 동의 화면에 테스트 사용자 등록.
*   **LangGraph 순환 흐름 및 재귀 오류**:
    *   **이슈**: "검토 후 게시" 흐름에서 사용자 피드백을 반영하는 순환 로직이 LangGraph 내에서 무한 루프를 발생시켜 `GraphRecursionError` 발생.
    *   **해결**:
        *   `app/graph/builder.py`에서 `request_user_review_node` 이후 바로 `END`로 가도록 수정하여, `/process_post` API 호출 시에는 1차 변환 및 검토 요청까지만 처리하고 그래프 종료.
        *   `app/services/content_service.py`의 `process_review` 함수 로직을 수정하여, `/submit_review` API 호출 시 이전 상태와 사용자 피드백을 바탕으로 `revise_content_node` 또는 `post_to_blogger_node`를 직접 호출하도록 변경.
        *   `compile()` 및 `ainvoke()`의 `recursion_limit` 설정 조정.
*   **LLM 응답 처리 문제**:
    *   **이슈**: Gemini API가 HTML 콘텐츠 외에 불필요한 머리말 텍스트나 마크다운 코드 블록(```html ... ```)을 함께 반환하여 Blogger에 그대로 게시되는 문제.
    *   **해결**: `app/graph/nodes.py`의 `transform_content_node` 및 `revise_content_node` 함수 내에서 LLM 응답 수신 후, 정규식 또는 문자열 처리를 통해 머리말과 마크다운 코드 블록을 제거하고 순수 HTML만 추출하도록 로직 추가.
*   **UI 피드백 부족**:
    *   **이슈**: Blogger 포스팅 성공 후에도 UI에 명확한 성공 메시지나 게시글 URL이 표시되지 않는 문제.
    *   **해결**: `static/script.js`를 수정하여, API 응답에서 `posted_url`을 받아 상태 메시지 영역과 별도의 URL 표시 영역에 명확히 출력하도록 개선.

## 3. 문서별 개선 제안

위 이슈들을 바탕으로, 교육생들이 프로젝트를 처음부터 진행할 때 참고할 주요 문서들에 다음과 같은 개선 사항을 반영할 것을 제안합니다. (아래 내용은 이미 본 개발 사이클에서 부분적으로 또는 전체적으로 반영되었을 수 있으나, 초기 문서 상태를 기준으로 개선점을 명시합니다.)

### 3.1. `docs/requirement.md` (요구사항 명세서)

*   **"2.2. 주요 기능" > "웹 UI" 섹션 개선**:
    *   "모든 API 요청(콘텐츠 처리, 검토 제출, 인증 등)에 대한 성공/실패 여부를 명확한 상태 메시지로 UI에 표시하고, 성공 시 필요한 결과(예: 포스팅 URL 링크)를 함께 제공." 항목을 명시적으로 추가하여 UI 피드백의 중요성을 강조.
*   **"2.3. 사용자 시나리오" > "검토 후 개시 선택 시" 흐름 구체화**:
    *   시나리오를 2단계 API 호출로 명확히 구분:
        1.  **1단계 (최초 변환 및 검토 요청 - `/process_post` API)**: 사용자가 "검토 후 게시"로 요청하면, 시스템은 콘텐츠를 1차 변환하고, 변환된 콘텐츠와 함께 사용자 검토를 위한 UI를 표시한 후 **사용자의 다음 액션을 대기** (LangGraph 실행은 이 단계에서 일단 종료되고 중간 결과를 반환).
        2.  **2단계 (사용자 피드백 처리 - `/submit_review` API)**: 사용자가 UI에서 "의견 제출" 또는 "최종 배포 승인" 시, 해당 내용을 별도 API로 전달받아 LangGraph 워크플로우를 재개하거나 특정 노드부터 실행하여 처리.
    *   각 단계별로 UI에 표시되어야 할 정보(상태 메시지, 변환된 콘텐츠, URL 등)를 명시.

### 3.2. `docs/page-design.md` (페이지 디자인)

*   **"3.4. 결과 표시 및 검토 영역" 보강**:
    *   "상태 메시지 표시 영역" 설명을 구체화: "AI 에이전트의 처리 상태(예: "변환 중...", "Blogger에 포스팅 중...", "오류 발생: ...") 및 최종 결과(예: "Blogger에 성공적으로 게시되었습니다! URL: [클릭 가능한 링크]")를 사용자에게 명확하게 알려주는 텍스트 영역. 메시지 유형(정보, 성공, 경고, 오류)에 따라 적절한 스타일(예: Bootstrap Alert 컴포넌트 활용)을 적용합니다."
    *   "포스팅 URL 표시 영역 (성공 시)" 항목 신설: "Blogger 포스팅 성공 시, 게시된 글의 URL을 클릭 가능한 링크 형태로 명확하게 표시하는 영역. (예: "포스팅 URL: <a href='...' target='_blank'>...</a>") 이 영역은 포스팅 성공 시에만 나타납니다."

### 3.3. `docs/development-guide.md` (개발 가이드)

*   **"3. 백엔드 설계 (FastAPI)" > "`app/routers/post_router.py`" 및 "`app/services/content_service.py`" 설명 보강**:
    *   `/process_post`와 `/submit_review` API 엔드포인트의 역할을 명확히 구분하여 설명 (위 요구사항의 2단계 흐름 반영).
*   **"4. AI 워크플로우 설계 (LangGraph)" 섹션 구체화**:
    *   `GraphState` 정의 시, `is_review_needed`, `user_review_feedback`, `current_iteration` 등 상태 값의 역할과 변경 시점을 명확히 기술.
    *   `request_user_review_node`의 역할을 "사용자 검토가 필요함을 상태에 표시하고, 그래프 실행을 일단 종료시켜 중간 결과를 반환하는 것"으로 명시.
    *   `should_revise_or_post_final_node`는 `/submit_review` 요청 처리 시 `content_service` 내에서 호출되어 다음 단계를 결정하는 로직으로 사용될 수 있음을 언급.
    *   `app/graph/builder.py`에서 `request_user_review_node` 이후 `END`로 연결하여 "검토 후 게시" 흐름의 1단계 종료를 명시.
*   **"5. 프론트엔드 설계 (HTML, CSS, JavaScript)" > "`static/script.js`" 설명 보강**:
    *   "Fetch API를 사용한 비동기 백엔드 통신" 항목에 "(API 응답에 따른 성공/실패 메시지 및 결과 데이터(예: 포스팅 URL, `is_review_needed` 상태 등)를 UI에 명확히 반영하고, 다음 사용자 액션을 위한 UI 상태를 적절히 업데이트하는 로직 포함)"과 같이 구체적인 UI 피드백 처리의 중요성을 강조.
*   **LLM 연동 부분 명확화**:
    *   사용할 LLM을 Google Gemini (`gemini-2.5-flash-preview-05-20` 모델)로 명시.
    *   `.env` 파일의 API 키 변수명을 `GEMINI_API_KEY`로 통일하여 안내.
    *   `app/graph/nodes.py`의 `get_llm_client()` 함수에서 `ChatGoogleGenerativeAI` 사용 및 관련 설정을 명시.
    *   LLM 응답에서 불필요한 머리말이나 마크다운 코드 블록을 제거하는 전처리 로직의 필요성 언급.

### 3.4. `docs/tasks/*.md` (개별 태스크 문서)

*   **`docs/tasks/004-basic-frontend-ui.md` (기본 프론트엔드 UI 구현)**:
    *   체크리스트의 `static/script.js` 관련 항목에 "응답 수신 후 `status-message` (성공/실패 메시지, 포스팅 URL 포함) 및 `transformed-content-display` 업데이트 로직 구현. (API 응답 결과에 따라 사용자에게 명확한 피드백 제공)"과 같이 UI 피드백 처리 내용을 명시적으로 포함.
*   **`docs/tasks/005-langgraph-basic-and-transform.md` (LangGraph 기본 구조)**:
    *   `GraphState` 정의 시 `is_review_needed`와 같은 흐름 제어용 상태 필드의 목적과 사용 예시를 간략히 언급.
    *   LLM 클라이언트 초기화 시 사용할 모델(`gemini-2.5-flash-preview-05-20`) 및 API 키(`GEMINI_API_KEY`)를 명시.
    *   LLM 프롬프트 예시를 현재 코드에서 사용하는 "전문적이고 정중한 스타일의 HTML 블로그 게시물 작성용" 프롬프트로 업데이트.
*   **`docs/tasks/008-review-then-publish-flow.md` ("검토 후 개시" 옵션 처리)**:
    *   **목표 수정**: "검토 후 게시" 흐름을 위한 2단계 API(`/process_post` 및 `/submit_review`) 및 관련 LangGraph 로직 구현으로 구체화.
    *   **체크리스트 세분화**:
        *   `/process_post` API에서 "검토 후 게시" 선택 시, 1차 변환 후 `is_review_needed=True` 상태와 변환된 콘텐츠를 반환하는 로직 구현.
        *   `app/graph/builder.py`에서 `request_user_review_node` 이후 `END`로 연결 수정.
        *   `/submit_review` API 엔드포인트 구현.
        *   `app/services/content_service.py`의 `process_review` 함수에서 사용자 피드백에 따라 `revise_content_node` 또는 `post_to_blogger_node`를 적절히 호출하는 로직 구현.
        *   프론트엔드(`static/script.js`)에서 `/submit_review` API 호출 및 결과에 따른 UI 업데이트 로직 구현.
    *   **"진행-상황-및-특이사항" 섹션에 본 1차 개발 사이클에서 겪었던 주요 문제점(재귀 오류, LLM 응답 처리, UI 피드백 부재)과 그 해결 과정을 요약하여 기록**하여, 교육생들이 유사한 문제 발생 시 참고할 수 있도록 함. (이 부분은 현재 워크로그 내용을 요약하여 반영 가능)

## 4. 결론: 지속적인 개선의 중요성

본 보고서에서 제안된 문서 개선 사항들은 1차 개발 사이클의 경험을 통해 얻은 교훈을 반영한 것입니다. 이는 소프트웨어 개발, 특히 AI 에이전트와 같이 복잡한 시스템을 개발하는 과정에서는 초기 설계와 문서가 완벽할 수 없으며, 실제 구현과 테스트를 통해 발견되는 문제점들을 바탕으로 지속적으로 문서를 현행화하고 개선해나가는 것이 매우 중요함을 보여줍니다.

또한, AI 어시스턴트와의 협업 과정에서는 명확한 지시, 단계별 검증, 그리고 상세한 로깅을 통해 문제 발생 시 원인을 신속하게 파악하고 해결하는 능력이 중요합니다. 교육생들은 이러한 과정을 통해 실제 AI 협업 개발 환경에서 필요한 역량을 키울 수 있을 것입니다.

향후 개발 사이클에서도 이러한 회고와 개선 과정을 반복함으로써 프로젝트 문서의 완성도를 높이고, 교육 프로그램의 질을 향상시킬 수 있을 것으로 기대합니다.
