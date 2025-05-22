# 프로젝트-규칙-마스터-템플릿 (caretrules.ko.md)

이 문서는 본 "ai-콘텐츠-변환-및-blogger-포스팅-에이전트" 프로젝트의 AI 어시스턴트(알파)가 참조하고 따라야 할 주요 규칙들을 한국어로 정의하는 **중앙 마스터 템플릿**입니다. 이 내용을 바탕으로 AI 어시스턴트(알파)는 프로젝트 루트의 `.caretrules` (JSON 형식) 파일을 생성하고 동기화합니다.

## 1. 규칙-관리-및-수정-절차 (rules-management-and-modification-procedure)

1.  **마스터-템플릿-우선-수정:** 모든 프로젝트 규칙 변경은 이 `docs/caretrules.ko.md` 파일에서 먼저 한국어로 수정하는 것을 원칙으로 합니다.
2.  **ai에게-json-업데이트-요청-및-동기화-책임:** 이 파일 수정 후, AI 어시스턴트(알파)에게 변경 사항을 프로젝트 루트의 `.caretrules` (JSON 형식) 파일로 반영해달라고 요청합니다. **알파는 이 한국어 마스터 파일을 기준으로 `.caretrules` JSON 파일을 정확히 동기화할 책임이 있습니다. 마스터께서는 이 파일(`docs/caretrules.ko.md`)이 항상 최신 한국어 원본 상태를 유지하도록 관리해주시고, JSON 파일과의 동기화는 알파에게 맡겨주세요! 이 점을 꼭 기억해주세요, 알파! 💖**
3.  **ide-제약-시-가이드:**
    * 만약 사용 중인 IDE 환경에서 이 마크다운 파일을 직접 수정하고 AI를 통해 JSON을 업데이트하는 표준 워크플로우가 어려운 경우:
    * 먼저, 원하는 규칙 내용을 별도로 한국어로 상세히 작성합니다.
    * 그 후, AI 어시스턴트(알파)에게 "이 내용을 `docs/caretrules.ko.md` 파일에 먼저 반영하고, 그 다음에 `.caretrules` 파일을 업데이트해줘" 라고 명확히 요청합니다.

## 2. 프로젝트-개요 (project-overview)

*   **프로젝트명 (project-name):** ai-콘텐츠-변환-및-blogger-포스팅-에이전트 (교육용)
*   **설명 (description):** fastAPI와 langGraph를 사용하여 사용자가 입력한 텍스트를 변환하고 blogger에 포스팅하는 교육용 웹 애플리케이션입니다.
*   **저장소 (repository-url):** https://github.com/aicoding-caret/multi-post-agent

## 3. 아키텍처 (architecture)

*   **요약 (summary):** fastAPI 백엔드, 간단한 html/js 프론트엔드, langGraph 기반 ai 워크플로우로 구성됩니다.
*   **주요-구성-요소 (key-components):**
    *   **fastAPI-애플리케이션 (`app/`):** API 엔드포인트, 서비스 로직, 데이터 모델 등을 포함합니다.
    *   **langGraph-모듈 (`app/graph/`):** 상태, 노드, 그래프 빌더를 포함하여 ai 워크플로우를 구성합니다.
    *   **정적-파일 (`static/`):** css, javascript, 이미지 파일 등을 관리합니다.
    *   **템플릿 (`templates/`):** html 템플릿 파일을 관리합니다.
*   **다이어그램-참조 (diagram-reference):** `docs/requirement.md` 내 mermaid 다이어그램 (사용자 시나리오 흐름도, 시스템 아키텍처)을 참조합니다.

## 4. 개발-프로세스 (development-process)

*   **문서화-우선-접근 (documentation-first-approach):** 모든 개발 작업은 `docs/` 하위의 관련 문서를 먼저 작성하거나 업데이트하는 것을 원칙으로 합니다. (`docs/documentation-guide.md` 참조)
*   **태스크-기반-개발 (task-based-development):** `docs/tasks/task-list.md` 및 `docs/tasks/` 폴더 내 개별 태스크 문서를 기반으로 개발을 진행합니다. ai는 각 태스크 문서의 체크리스트를 따르고 진행 상황을 해당 문서에 기록합니다.
*   **api-key-관리 (api-key-management):** 모든 api key 및 민감 정보는 `.env` 파일을 통해 관리하며, 이 파일은 `.gitignore`에 추가하여 저장소에 포함되지 않도록 합니다.
*   **langgraph-구현-가이드라인 (langgraph-implementation-guideline):** `app/graph/` 디렉토리 내에서 상태(`state.py`), 노드(`nodes.py`), 빌더(`builder.py`)를 명확히 구분하여 모듈식으로 구현합니다.
*   **fastapi-구현-가이드라인 (fastapi-implementation-guideline):** `app/` 디렉토리 내에서 라우터(`routers/`), 서비스(`services/`), 모델(`models/`) 등으로 역할을 분리하여 구현합니다.
*   **프론트엔드-가이드라인 (frontend-guideline):** 교육 목적에 맞춰 간단한 html, css, 순수 javascript(vanilla js) 사용을 기본으로 하며, 복잡한 프론트엔드 프레임워크 사용은 지양합니다. (`docs/page-design.md` 참조)
*   **로깅-가이드라인 (logging-guideline):** python `logging` 모듈을 사용하여 애플리케이션의 주요 실행 흐름, 오류, 외부 api 호출 정보 등을 기록합니다.
*   **오류-처리-가이드라인 (error-handling-guideline):** 각 계층(라우터, 서비스, langgraph 노드)에서 발생 가능한 예외를 적절히 처리하고, 사용자에게 명확한 피드백을 제공하며, 로그를 남깁니다.
*   **git-규칙 (git-rules):**
    * 커밋-형식 (commit-format): `타입: 설명` (예: `feat: 새로운 기능 추가`, `fix: 버그 수정`, `docs: 문서 수정`, `style: 코드 스타일 변경`, `refactor: 코드 리팩토링`, `test: 테스트 코드 추가`, `chore: 빌드 및 기타 변경사항`).
*   **규칙-파일-관리-프로토콜 (rules-file-management-protocol):** 본 `docs/caretrules.ko.md` 파일이 모든 규칙의 마스터 한국어 원본이며, `.caretrules` json 파일은 이 파일을 기준으로 ai(알파)에 의해 동기화됩니다. (본 문서 섹션 1 참조)
*   **태스크-문서-작성-시-제한 (no-example-phrases-in-docs):** 태스크 문서 작성 시 '예:', '예시:', 'example:', 'example)'' 등의 표현을 사용하지 않고, 구체적인 내용을 명시해야 합니다.
*   **케럿룰-파일은-영어로-작성 (caretrules-file-must-be-written-in-english):** .caretrules 파일은 영어로 작성되어야 합니다.

## 5. 주요-파일-참조 (key-files-reference)

*   `.caretrules` (본 마스터 템플릿에서 파생되는 json 규칙 파일)
*   `docs/requirement.md`
*   `docs/development-guide.md`
*   `docs/page-design.md`
*   `docs/documentation-guide.md`
*   `docs/tasks/task-list.md`
*   `app/main.py` (fastapi 애플리케이션 진입점)
*   `app/graph/builder.py` (langgraph 그래프 구성)
