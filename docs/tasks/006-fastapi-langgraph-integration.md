# 태스크-006: fastapi와-langgraph-연동-(기본-변환-흐름)

**id:** 006
**태스크-명칭:** fastapi와-langgraph-연동-(기본-변환-흐름)
**상태:** to-do
**담당자:** ai-어시스턴트 (Caret)
**관련-문서:**
* `docs/development-guide.md` (섹션 3. 백엔드 설계 (FastAPI), 섹션 4. AI 워크플로우 설계 (LangGraph))
* `docs/requirement.md` (섹션 2.3. 사용자 시나리오)
* `app/graph/builder.py` (컴파일된 LangGraph 앱)
* `app/services/content_service.py`
* `app/routers/post_router.py`
* `app/models/post_models.py`
* `static/script.js`

## 1. 목표 (goal)

사용자가 프론트엔드 UI를 통해 입력한 데이터를 FastAPI 엔드포인트가 수신하고, 이를 LangGraph 워크플로우에 전달하여 콘텐츠 변환을 실행한 후, 그 결과를 다시 프론트엔드에 반환하는 기본 연동 흐름을 구현합니다.

## 2. 체크리스트 (checklist)

- [ ] `app/models/post_models.py` 파일 생성 및 Pydantic 모델 정의:
    * `PostCreateRequest`: 사용자가 프론트엔드에서 전송할 데이터 모델 (예: `title: str`, `content: str`, `processing_option: str`).
    * `PostResponse`: FastAPI가 프론트엔드로 반환할 데이터 모델 (예: `status: str`, `transformed_content: Optional[str]`, `message: Optional[str]`).
- [ ] `app/services/content_service.py` 파일 생성 및 `process_content` 함수 구현:
    * `PostCreateRequest` 객체를 입력으로 받음.
    * `app.graph.builder.create_graph()`를 호출하여 컴파일된 LangGraph 앱 가져오기 (또는 앱 초기화 시 미리 생성하여 주입).
    * LangGraph 앱 실행 (`app.invoke`) 시 필요한 초기 상태(`GraphState` 형식)를 `PostCreateRequest` 데이터로 구성하여 전달.
        * 예: `initial_state = {"original_title": request.title, "original_content": request.content, "processing_option": request.processing_option, "transformed_content": ""}`
        * LangGraph 실행 결과(최종 상태)를 받아 `PostResponse` 객체로 변환하여 반환.
        * 성공 시: `status="success"`, `transformed_content=final_state.get("transformed_content")`.
- [ ] `app/routers/post_router.py` 파일 생성 (또는 기존 파일에 추가):
    * `APIRouter` 인스턴스 생성.
    * `/process_post` POST 엔드포인트 정의:
        * `PostCreateRequest`를 요청 본문으로 받음.
        * `content_service.process_content` 함수 호출.
        * 반환된 `PostResponse`를 클라이언트에 응답.
- [ ] `app/main.py`에서 `post_router`를 FastAPI 앱에 등록 (`app.include_router`).
- [ ] `static/script.js` 수정:
    * "변환 및 처리 요청" 버튼 클릭 시, 입력된 제목, 내용, 처리 옵션을 JSON 형태로 구성.
    * Fetch API를 사용하여 `/process_post` 엔드포인트로 POST 요청 전송 (올바른 `Content-Type: application/json` 헤더 포함).
    * 서버로부터 받은 `PostResponse` 데이터를 파싱.
    * `status-message` 영역에 상태 메시지(성공/실패) 표시.
    * `transformed-content-display` 영역에 변환된 콘텐츠 표시.
- [ ] (테스트) 프론트엔드에서 텍스트 입력 후 "변환 및 처리 요청" 버튼 클릭 시, FastAPI를 거쳐 LangGraph가 실행되고, 변환된 결과가 다시 프론트엔드에 표시되는지 확인.

## 3. 진행-상황-및-특이사항 (progress-and-notes)

* (여기에 진행하면서 발생하는 특이사항이나 결정 사항 등을 기록합니다.)
* FastAPI의 비동기(`async/await`) 처리를 LangGraph 호출 시 적절히 활용해야 합니다. (LangGraph의 `ainvoke` 사용)
* Pydantic 모델을 사용하여 API 요청/응답 데이터의 유효성을 검증하고, 명확한 데이터 구조를 정의해야 합니다.
* LangGraph 실행 시 초기 상태(`GraphState`)를 올바르게 구성하여 전달해야 합니다.

## 4. 다음-태스크 (next-task)

* [태스크-007: "자동-개시"-옵션-처리](./007-auto-publish-flow.md)
