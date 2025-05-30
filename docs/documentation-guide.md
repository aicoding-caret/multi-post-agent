# 문서화 가이드 (documentation-guide)

## 1. 개요 (overview)

본 문서는 "ai-콘텐츠-변환-및-blogger-포스팅-에이전트" 프로젝트의 모든 관련 문서들을 효과적으로 작성하고 관리하기 위한 지침을 제공합니다. 일관성 있고 유용한 문서화를 통해 프로젝트 이해도를 높이고, 개발 및 교육 과정을 원활하게 지원하는 것을 목표로 합니다.

## 2. 문서-목록-및-목적 (document-list-and-purpose)

| 문서명 (한글) | 파일 경로 (케밥-케이스) | 주요 내용 및 목적 | 대상 독자 | 업데이트 주기 |
| :----------------------------------- | :----------------------------------- | :--------------------------------------------------------------------------------------------------------------- | :----------------------------------------- | :---------------- |
| **교육 시나리오** | `docs/education-scenario.md` | 교육 목표, 전체 과정, 시간 계획, 준비물 등 교육 프로그램의 전체적인 개요와 흐름을 안내합니다. | 강사, 교육생, 프로젝트 관리자 | 교육 내용 변경 시 |
| **요구사항 명세서** | `docs/requirement.md` | 프로젝트가 제공해야 할 기능, 사용자 시나리오, 시스템 아키텍처(개념적) 등을 명확히 정의합니다. "무엇을 만들 것인가"에 대한 답변. | 프로젝트 관리자, 개발자(ai), 교육생(참고용) | 요구사항 변경 시 |
| **페이지 디자인** | `docs/page-design.md` | 웹 애플리케이션의 ui 레이아웃, 주요 구성 요소, 간단한 스타일 가이드 등을 정의합니다. | 개발자(ai), ui/ux 디자이너(있을 경우) | ui 디자인 변경 시 |
| **google-api-참조** | `docs/google-api-references.md` | google 계정 생성, gemini api 키, blogger api 설정 등 google 관련 api 사용법 및 주의사항을 안내합니다. | 개발자(ai), 교육생 | api 정보 변경 시 |
| **개발-가이드 (ai-참조용)** | `docs/development-guide.md` | ai(caret)가 웹 애플리케이션을 개발할 때 참조하는 기술적 지침입니다. 프로젝트 구조, 모듈 설계, 핵심 로직 구현 방향 등을 포함. | 개발자(ai - caret) | 개발 방향 변경 시 |
| **태스크-리스트** | `docs/tasks/task-list.md` | 실제 개발 작업을 구체적인 태스크 단위로 나누어 관리합니다. 각 태스크의 상태, 담당자, 관련 문서 등을 기록. | 프로젝트 관리자, 개발자(ai) | 개발 진행 중 수시 |
| **문서화-가이드 (본 문서)** | `docs/documentation-guide.md` | 프로젝트 문서화 원칙, 각 문서의 목적 및 관리 지침을 정의합니다. | 프로젝트 참여자 모두 | 필요시 |
| **readme** | `readme.md` | 프로젝트의 최상위 소개 문서. 프로젝트 개요, 설치 방법, 실행 방법, 주요 기술 스택 등을 간략히 안내. | 모든 프로젝트 접근자 | 주요 변경 시 |
| **작업-로그 (work-logs)** | `work-logs/yyyymmdd-log.md` | (선택 사항) 일일 또는 주간 단위의 개발 진행 상황, 문제점, 해결 과정 등을 기록합니다. | 프로젝트 관리자, 개발자(ai) | 주기적 (일/주) |
| **발표-자료 (교육용)** | `presentation/` | (선택 사항) 교육 진행 시 사용할 발표 자료 (pdf, pptx 등). | 강사, 교육생 | 교육 내용 변경 시 |

## 3. 문서-작성-원칙 (document-writing-principles)

* **명확성 (clarity):** 모호한 표현을 피하고, 쉽고 명확하게 작성합니다.
* **정확성 (accuracy):** 최신 정보를 반영하고, 사실에 기반하여 작성합니다.
* **간결성 (conciseness):** 불필요한 내용은 최소화하고, 핵심 정보를 중심으로 작성합니다.
* **일관성 (consistency):** 용어, 스타일, 포맷 등을 일관되게 유지합니다.
* **대상-독자-고려 (target-audience-consideration):** 각 문서의 주요 독자를 고려하여 수준과 상세도를 조절합니다.
* **버전-관리 (version-control):** 모든 문서는 git을 통해 버전 관리합니다. 중요한 변경 시에는 커밋 메시지에 변경 내용을 명확히 기록합니다.
* **mermaid-활용 (using-mermaid):** 다이어그램이 필요한 경우, mermaid.js를 사용하여 텍스트 기반으로 작성하고 문서에 포함시켜 버전 관리가 용이하도록 합니다.
* **표기법 (naming-conventions):**
    * **파일명, 디렉토리명, 문서-내-제목 (섹션-제목-등):** 모든 명칭은 **소문자 케밥-케이스(kebab-case)**를 사용합니다.
        * 띄어쓰기 대신 하이픈(-)을 사용하고, 여러 단어로 이루어진 경우 각 단어를 하이픈으로 연결합니다.
        * 예시: `## 3.1. 사용자-시나리오-흐름도-(mermaid)`
    * 이는 url 친화적이며, 다양한 운영체제에서 일관성을 유지하고, 시프트 키 사용을 최소화하는 데 도움이 됩니다.

## 4. 태스크-문서-작성법 (task-document-writing-method)

태스크 관련 문서들(예: `docs/tasks/task-list.md`, 개별 태스크 상세 문서 등)은 다음 원칙에 따라 작성하고 관리합니다.

* **구체적인 체크리스트:** 각 태스크는 수행해야 할 구체적인 작업 항목들을 체크리스트 형태로 명확히 작성합니다. 이를 통해 진행 상황을 쉽게 파악하고 누락되는 부분이 없도록 합니다.
* **작은 단위의 태스크:** 태스크는 너무 큰 단위로 정의하지 않고, 관리 가능하고 측정 가능한 작은 단위로 분할합니다. 이는 AI가 각 태스크를 명확히 이해하고 단계적으로 처리하는 데 도움이 됩니다.
* **진행-상황-및-특이사항-기록:** 각 태스크의 진행 상태(예: To Do, In Progress, Done, Blocked)를 명시하고, 개발 과정에서 발생하는 특이사항, 문제점, 해결 과정, 결정 사항 등을 상세히 기록합니다.
* **사용자와의-협력-및-현행화:** 태스크 문서는 AI와 사용자(마스터) 간의 주요 소통 도구입니다. 진행 상황에 따라 사용자와 협력하여 내용을 업데이트하고, 항상 최신 상태를 유지(현행화)합니다.
* **세션-간-연속성-확보:**
    * 각 태스크를 시작하거나 이어갈 때, 해당 태스크 수행에 필요한 모든 관련 문서의 위치(경로)를 명시적으로 포함합니다. (예: "관련 문서: `docs/requirement.md` 섹션 2.2, `docs/development-guide.md` 섹션 3")
    * 이를 통해 AI가 이전 작업 내용을 쉽게 파악하고, 중단된 세션에서도 원활하게 작업을 이어갈 수 있도록 합니다.
* **파일-위치:** 모든 태스크 관련 문서는 `docs/tasks/` 디렉토리 하위에 생성하고 관리합니다.

## 5. 문서-업데이트-및-관리 (document-update-and-management)

* 각 문서는 정의된 "업데이트 주기"에 따라 검토하고 필요시 업데이트합니다.
* 요구사항 변경, 설계 변경, 기능 추가/수정 등 프로젝트의 주요 변경 사항 발생 시 관련 문서를 즉시 업데이트하는 것을 원칙으로 합니다.
* 문서 간의 내용이 충돌하지 않도록 주의하며, 상호 참조가 필요한 경우 링크를 활용합니다.
