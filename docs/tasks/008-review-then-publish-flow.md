샇샇# 태스크-008: "검토-후-개시"-옵션-처리 (review-then-publish-flow)

**id:** 008
**태스크-명칭:** "검토-후-개시"-옵션-처리-흐름-구현
**상태:** to-do 
**담당자:** ai-어시스턴트 (Caret)
**관련-문서:**
* `docs/development-guide.md` (섹션 4. AI 워크플로우 설계 (LangGraph))
* `docs/requirement.md` (섹션 2.3. 사용자 시나리오 - 검토 후 개시 분기)
* `app/graph/nodes.py`
* `app/graph/builder.py`
* `app/services/blogger_service.py` (포스팅 함수)

## 1. 목표 (goal)

사용자가 "검토 후 개시" 옵션을 선택했을 경우, LangGraph 워크플로우가 콘텐츠 변환 후 사용자에게 검토를 요청하고, 사용자의 의견을 반영하여 콘텐츠를 수정하고, 최종적으로 Blogger API를 호출하여 포스팅을 완료하는 흐름을 구현합니다.

## 2. 체크리스트 (checklist)

* [x] `app/graph/nodes.py`에 `request_user_review_node` 함수 구현 (코드 작성 완료)
* [x] `app/graph/nodes.py`에 `revise_content_node` 함수 구현 (코드 작성 완료)
* [x] `app/graph/builder.py`의 `create_graph` 함수 수정 (코드 작성 완료, `ImportError`로 실행 불가)
* [x] `app/routers/post_router.py`에 `/submit_review` POST 엔드포인트 추가 (코드 작성 완료)
* [x] `app/services/content_service.py`에 `process_review` 함수 구현 (코드 작성 완료)
* [x] `static/script.js` 수정 (코드 작성 완료)
* [x] (테스트) 프론트엔드에서 "검토 후 개시" 옵션 선택 후 요청 시, 콘텐츠 변환, 사용자 검토, 의견 반영, 최종 배포까지 정상적으로 이루어지는지 확인. (Gemini API 연동 및 HTML 마크다운 제거 후 최종 확인 완료)

## 3. 진행-상황-및-특이사항 (progress-and-notes)

* (2025-05-22) "검토 후 개시" 흐름을 위한 코드 작성은 대부분 완료되었으나, `app/graph/builder.py` 파일에서 `from langgraph.graph import StatefulGraph` (또는 `StateGraph`) 관련 `ImportError`가 발생하여 애플리케이션 실행 및 기능 테스트가 불가능한 상태임. 해당 오류는 다음 작업 세션에서 해결을 시도할 예정임.
* 코드 작성 완료된 파일 목록:
    * `app/graph/nodes.py`: `request_user_review_node`, `revise_content_node`, `should_revise_or_post_final_node` 추가 및 `should_post_or_review_node` 수정.
    * `app/models/post_models.py`: `ReviewRequest` 모델 확장 및 `PostResponse` 모델 필드(`is_review_needed`, `current_iteration`) 추가.
    * `app/routers/post_router.py`: `/submit_review` 엔드포인트 추가.
    * `app/services/content_service.py`: `process_review` 함수 구현 및 `process_content` 함수에서 `PostResponse` 업데이트.
    * `templates/index.html`: "검토 후 개시" 관련 UI 요소 추가 및 활성화.
    * `static/script.js`: "검토 후 개시" UI 로직 및 서버 통신 로직 구현, 상태 관리 로직 추가.
* 서버 실행 시도 중 `requirements.txt`에 누락된 다수의 라이브러리(`Jinja2`, `google-auth-oauthlib`, `google-api-python-client`, `pydantic-settings`, `langgraph`)를 발견하여 순차적으로 추가 및 설치함.
* 이러한 예측 불가능한 오류 발생이 교육 프로그램의 예측 가능성을 저해할 수 있다는 점, 단계별 테스트의 중요성, AI의 단독 작업으로 인한 사용자 가시성 부족 문제에 대해 마스터와 논의함.
* `.env` 파일 생성 및 사용법 안내를 `docs/education-scenario.md`와 `docs/google-api-references.md`에 보강함.
* 상세 논의 내용 및 현재까지의 진행 상황은 `work-logs/20250522-01-mid-task008-review-and-scenario-discussion.md`에 기록됨.
* (기존 특이사항) LangGraph의 순환 구조(사용자 검토 -> 재변환)를 정확히 구현해야 합니다.
* (기존 특이사항) 프론트엔드와 백엔드 간 데이터 흐름을 명확히 파악하고, 각 단계에서 필요한 데이터를 정확하게 전달해야 합니다.
* (2025-05-22 추가) "검토 후 게시" 기능 테스트 중, LangGraph 재귀 오류 및 LLM 응답(마크다운 포함) 처리, UI 피드백(포스팅 URL 미표시) 등 다수의 문제점을 발견하고 해결함. 이 과정에서 명확한 API 응답 처리 및 UI 피드백의 중요성을 재확인함. 관련 요구사항 및 가이드 문서에 해당 내용 보강.

## 4. 다음-태스크 (next-task)

* [태스크-009: 최종-테스트-및-문서화-정리](./009-final-testing-and-documentation.md)
