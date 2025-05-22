**id:** 004
**태스크-명칭:** 기본-프론트엔드-ui-구현
**상태:** to-do
**담당자:** ai-어시스턴트 (Caret)
**관련-문서:**
* `docs/page-design.md` (섹션 3. 주요 페이지 구성 요소, 섹션 4. 스타일 가이드)
* `docs/development-guide.md` (섹션 5. 프론트엔드 설계)
* `docs/requirement.md` (섹션 2.2. 주요 기능 - 웹 UI)
* `static/app_title.jpg` (로고 이미지 파일)

## 1. 목표 (goal)

사용자가 콘텐츠 초안을 작성하고 처리 옵션을 선택하여 서버에 요청을 보낼 수 있는 기본적인 웹 UI를 HTML, CSS, JavaScript를 사용하여 구현합니다. 서버로부터의 응답을 받아 사용자에게 간단한 결과를 표시합니다.

## 2. 체크리스트 (checklist)

- [ ] `templates/index.html` 파일 생성 및 기본 HTML5 구조 작성.
    - [ ] 문서 제목 설정 (예: "AI 콘텐츠 에이전트").
    - [ ] `static/style.css` 링크.
    - [ ] `static/script.js` `<script defer>`로 링크.
- [ ] `templates/index.html`에 `docs/page-design.md`를 참조하여 다음 요소들 추가:
    - [ ] "입력 영역":
        - [ ] "제목" 레이블 및 `text` 타입 `input` 필드 (id: `post-title`).
        - [ ] "내용" 레이블 및 `textarea` 필드 (id: `post-content`).
    - [ ] "처리 옵션 영역":
        - [ ] "처리 옵션" 레이블.
        - [ ] "자동 개시" 라디오 버튼 (name: `processing-option`, value: `auto`, `checked` 기본값).
    - [ ] "실행 버튼 영역":
        - [ ] "변환 및 처리 요청" 버튼 (id: `submit-post-button`, type: `button` 또는 `submit`).
    - [ ] "결과 표시 영역":
        - [ ] 상태 메시지 표시 영역 (id: `status-message`).
        - [ ] 변환된 콘텐츠 표시 영역 (id: `transformed-content-display`, 읽기 전용).
    - [ ] Blogger 인증을 위한 링크/버튼 (id: `auth-blogger-button`, 텍스트: "Blogger 인증하기").
- [ ] `static/style.css` 파일 생성 및 기본적인 스타일 적용:
    * 전체 페이지 레이아웃 (간단한 중앙 정렬).
    * 입력 필드, 버튼 등의 기본 스타일.
    * (교육 범위에서는 미려함보다는 기능 구분에 초점).
* UI 라이브러리 (Bootstrap 5)를 사용하여 UI 요소들을 빠르게 구현합니다. CDN 링크를 통해 Bootstrap CSS와 JavaScript 파일을 프로젝트에 추가합니다.
- [ ] `static/script.js` 파일 생성 및 기본 JavaScript 로직 구현:
    * DOM 요소 선택 (예: `document.getElementById`).
    * "Blogger 인증하기" 버튼 클릭 시, `/auth/google` 엔드포인트로 페이지 이동 (`window.location.href`)하는 JavaScript 코드 추가.
    * "변환 및 처리 요청" 버튼 클릭 시, 폼 데이터를 수집하여 `/process_post` 엔드포인트로 전송하고, 결과를 받아와서 UI에 표시하는 JavaScript 코드 추가.
        - [ ] 폼 데이터 (제목, 내용, 처리 옵션) 가져오기.
        - [ ] Fetch API를 사용하여 `/process_post` 엔드포인트로 POST 요청 전송.
        - [ ] 요청 중 상태 메시지 업데이트 (예: "처리 중...").
        - [ ] 응답 수신 후 `status-message` (성공/실패 메시지, 포스팅 URL 포함) 및 `transformed-content-display` 업데이트 로직 구현. (API 응답 결과에 따라 사용자에게 명확한 피드백 제공)

## 3. 진행-상황-및-특이사항 (progress-and-notes)

* (여기에 진행하면서 발생하는 특이사항이나 결정 사항 등을 기록합니다.)
* 프론트엔드 유효성 검사는 최소화하고, 주로 서버 사이드 유효성 검사에 의존합니다 (교육 단순화).
* API 호출 결과(성공/실패 메시지, 데이터)를 사용자에게 명확히 보여주는 기본 영역 및 로직을 포함해야 합니다.

## 4. 다음-태스크 (next-task)

* [태스크-005: langgraph-기본-구조-및-변환-노드](./005-langgraph-basic-and-transform.md)
