# 태스크-005: langgraph-기본-구조-및-변환-노드 (langgraph-basic-structure-and-transform-node)

**id:** 005
**태스크-명칭:** langgraph-기본-구조-및-콘텐츠-변환-노드-구현
**상태:** to-do
**담당자:** ai-어시스턴트 (Caret)
**관련-문서:**
* `docs/development-guide.md` (섹션 4. AI 워크플로우 설계 (LangGraph))
* `docs/requirement.md` (섹션 2.2. 주요 기능 - 콘텐츠 스타일 변환)
* `app/core/config.py` (LLM API Key 접근)

## 1. 목표 (goal)

LangGraph를 사용하여 AI 콘텐츠 변환 워크플로우의 기본 구조를 설정하고, LLM을 호출하여 입력된 텍스트를 미리 정의된 스타일로 변환하는 핵심 노드를 구현합니다.

## 2. 체크리스트 (checklist)

- [ ] `app/graph/state.py` 파일 생성 및 `GraphState` (TypedDict) 정의:
    * `docs/development-guide.md` (섹션 4)에 정의된 필드들 포함 (예: `original_title`, `original_content`, `transformed_content`, `error_message` 등).
- [ ] `app/graph/nodes.py` 파일 생성 및 기본 구조 작성.
- [ ] `app/graph/nodes.py`에 `transform_content_node` 함수 구현:
    * `GraphState`를 입력으로 받음.
    * `state.original_content` (및 필요시 `original_title`)를 가져옴.
    * LLM 클라이언트 초기화 (`ChatGoogleGenerativeAI` 사용, 모델: `gemini-2.5-flash-preview-05-20`).
        * API 키는 `app.core.config.settings.GEMINI_API_KEY`에서 가져옴.
    * LLM에 전달할 프롬프트 정의 (전문적이고 정중한 스타일의 HTML 블로그 게시물 작성용, 예: "주어진 텍스트를 기반으로 전문적이고 정중한 스타일의 블로그 게시물을 작성해주세요. 결과는 HTML 형식이어야 하며, 제목은 `<h3>` 태그로, 본문은 여러 개의 `<p>` 태그로 구성해주세요. 만약 원본 제목이 제공되지 않았다면, 내용에 어울리는 제목을 생성해주세요. [원본 데이터] 제목: {title} 내용: {text} [변환된 블로그 게시물 HTML]").
    * LLM API 호출하여 콘텐츠 변환 실행.
    * 결과를 `state.transformed_content`에 저장.
    * 성공 시 업데이트된 `state` 반환.
- [ ] `app/graph/builder.py` 파일 생성 및 기본 구조 작성.
- [ ] `app/graph/builder.py`에 `create_graph` 함수 구현:
    * `StatefulGraph` (또는 `Graph`) 인스턴스 생성 (`GraphState` 사용).
    * `transform_content_node`를 그래프에 노드로 추가 (`add_node`).
    * 그래프의 진입점(entry point)을 `transform_content_node`로 설정.
    * 그래프 컴파일 (`compile()`).

## 3. 진행-상황-및-특이사항 (progress-and-notes)

* (여기에 진행하면서 발생하는 특이사항이나 결정 사항 등을 기록합니다.)
* 사용할 LLM(예: Gemini) 및 해당 API 키 설정을 확인해야 합니다.
* LangGraph 상태(`GraphState`) 정의 시, 이후 단계에서 필요한 모든 필드를 고려해야 합니다.
* 초기 변환 노드(`transform_content_node`)의 프롬프트는 명확하고 구체적으로 작성되어야 원하는 결과를 얻을 수 있습니다.

## 4. 다음-태스크 (next-task)

* [태스크-006: fastapi와-langgraph-연동](./006-fastapi-langgraph-integration.md)
