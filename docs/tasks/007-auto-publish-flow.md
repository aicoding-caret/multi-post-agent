# 태스크-007: "자동-개시"-옵션-처리 (auto-publish-option-flow)

**id:** 007
**태스크-명칭:** "자동-개시"-옵션-처리-흐름-구현
**상태:** to-do
**담당자:** ai-어시스턴트 (Caret)
**관련-문서:**
* `docs/development-guide.md` (섹션 4. AI 워크플로우 설계 (LangGraph) - `should_post_or_review_node`, `post_to_blogger_node` 부분)
* `docs/requirement.md` (섹션 2.3. 사용자 시나리오 - 자동 개시 분기)
* `app/graph/nodes.py`
* `app/graph/builder.py`
* `app/services/blogger_service.py` (포스팅 함수)

## 1. 목표 (goal)

사용자가 "자동 개시" 옵션을 선택했을 경우, LangGraph 워크플로우가 콘텐츠 변환 후 즉시 Blogger API를 호출하여 포스팅을 완료하는 흐름을 구현합니다.

## 2. 체크리스트 (checklist)

- [ ] `app/graph/nodes.py`에 `post_to_blogger_node` 함수 구현:
    * `GraphState`를 입력으로 받음.
    * `state.transformed_content` (및 필요시 `state.original_title`, `state.transformed_title`)를 가져옴.
    * `app.services.blogger_service`의 포스팅 함수를 호출하여 Blogger에 글 게시.
    * 포스팅 성공 시 `state.posted_url`에 게시된 글의 URL 저장.
    * 업데이트된 `state` 반환.
- [ ] `app/graph/nodes.py`에 `should_post_or_review_node` 함수 (조건부 엣지용) 구현:
    * `GraphState`를 입력으로 받음.
    * `state.processing_option` 값을 확인.
    * "auto"이면 "post_to_blogger" (다음 노드 이름) 반환.
    * 그 외의 경우 "end" 반환 (1차 목표에서는 'auto' 외 옵션은 바로 종료).
- [ ] `app/graph/builder.py`의 `create_graph` 함수 수정:
    * `post_to_blogger_node`를 그래프에 노드로 추가.
    * `transform_content_node` 다음에 `should_post_or_review_node`를 호출하는 조건부 엣지(`add_conditional_edges`) 추가.
        * `source`: `transform_content_node`.
        * `path`: `should_post_or_review_node` 함수.
        * `path_map`: `{"post_to_blogger": "post_to_blogger_node", "end": END}` (실제 노드 이름 및 종료점으로 매핑).
    * `post_to_blogger_node` 다음에는 그래프의 종료점(END)으로 연결.

## 3. 진행-상황-및-특이사항 (progress-and-notes)

* (여기에 진행하면서 발생하는 특이사항이나 결정 사항 등을 기록합니다.)
* `BLOG_ID`가 `.env` 파일 및 `app/core/config.py`에 올바르게 설정되어 있는지 확인해야 합니다.
* `should_post_or_review_node` 함수가 "auto" 옵션일 때 정확히 `post_to_blogger_node`로 분기하는지 확인해야 합니다.
* `post_to_blogger_node`에서 `blogger_service`를 통해 실제 포스팅이 성공적으로 이루어지는지 테스트해야 합니다.

## 4. 다음-태스크 (next-task)

* [태스크-008: 검토-후-개시"-옵션-처리 (review-then-publish-flow)](./008-review-then-publish-flow.md)
