[View English README (영문 README 보기)](README.md)

# Multi-Post AI Agent (Education Project)

## 🌟 프로젝트 소개

이 프로젝트는 AI 기반의 다국어 블로그 포스팅 자동화 에이전트 개발을 목표로 하는 교육용 프로젝트입니다.
Caret AI 어시스턴트와 함께 "바이브코딩" 방식으로 진행되며, AI 에이전트 개발 및 LangGraph 활용에 대한 실습 경험을 제공합니다.

## 🎯 프로젝트 목적

* Caret AI 어시스턴트를 활용한 코딩 실습 경험 제공
* AI 에이전트 개발 프로세스 학습
* LangGraph를 이용한 AI 워크플로우 구축 실습
* 교육용 시나리오 및 관련 리소스 제공

## 🚀 1차 개발 목표 (교육용 실습 단계)

* **플랫폼:** 로컬에서 실행되는 간단한 웹 애플리케이션 (Python FastAPI + HTML/JS)
* **핵심 기능:**
    1. 사용자 입력을 받아 LangGraph를 통해 특정 스타일/형태로 변환 (예: 말투 변경, 번역)
    2. 변환된 결과를 단일 블로그 플랫폼에 자동 포스팅
* **학습 목표:** AI 에이전트의 기본 흐름 이해 및 FastAPI, LangGraph 기초 실습

## 🔧 기술 스택 (1차 목표 기준)

* **Backend & AI Workflow:** Python, FastAPI, LangGraph
* **Frontend:** HTML, CSS, JavaScript (간단한 UI)
* **Development Assistant:** Caret

## 📝 교육 시나리오

상세한 교육 진행 시나리오는 [docs/education-scenario.md](docs/education-scenario.md) 문서를 참고해주세요.

## 📂 프로젝트 설계 및 주요 문서 안내 

본 프로젝트는 AI 어시스턴트(Caret - 알파)와 함께 체계적인 개발을 진행하기 위해 다음과 같은 순서와 목적으로 주요 문서들을 아래와 같은 과정을 거쳐 준비했습니다. 이 과정은 AI가 프로젝트의 요구사항을 명확히 이해하고, 계획에 따라 개발 작업을 수행하며, 교육생들이 AI와의 협업을 효과적으로 실습할 수 있도록 지원하기 위함입니다. 각 문서는 이전 단계의 문서를 기반으로 점진적으로 구체화되며, 이는 성공적인 프로젝트 수행과 교육 목표 달성에 중요한 역할을 합니다. 

1. **교육 시나리오 정의 (`docs/education-scenario.md`):**
    * **SW 공학 단계:** 프로젝트 착수 및 목표 설정 (Initiation & Goal Setting)
    * **작성 이유 및 의존성:** 모든 프로젝트와 교육의 시작점입니다. 이 문서가 없으면 프로젝트의 방향과 목표가 불분명해집니다. '무엇을 왜 하는가'에 대한 명확한 정의를 제공하여, 이후 모든 문서와 개발 활동의 기준이 됩니다.
    * **주요 내용:** 교육 목표, 전체 과정, 시간 계획, 준비물 등을 포함합니다. 초기 아이디어를 구체화하여 Blogger API 연동까지 포함하도록 교육 목표와 범위를 확장하고, 관련 설정 단계를 추가했습니다.

2. **외부 API 연동 가이드 마련 (`docs/google-api-references.md`):**
    * **SW 공학 단계:** 기술적 타당성 분석 및 환경 조사 (Technical Feasibility & Environment Scan)
    * **작성 이유 및 의존성:** `education-scenario.md`에서 Blogger 연동이 목표로 설정됨에 따라, 해당 기능 구현에 필요한 외부 API(Google Blogger API)의 사용법 및 인증 방식(OAuth 2.0)을 사전에 파악하고 정리합니다. 이는 기술적 실현 가능성을 검토하고, 이후 요구사항 및 개발 과정에서의 시행착오를 줄이는 데 목적이 있습니다.
    * **주요 내용:**
        * [Google API 일반 참조 문서](./docs/google-api-references.md): Blogger API 활성화 및 OAuth 2.0 클라이언트 ID 설정에 대한 구체적인 안내를 포함합니다.
        * [Google Blogger API 상세 가이드](./docs/google-blogger-api-guide.md): Blogger API의 각 엔드포인트 사용법에 대한 상세 내용을 포함합니다.

3. **요구사항 명세서 구체화 (`docs/requirement.md`):**
    * **SW 공학 단계:** 요구사항 분석 및 명세 (Requirements Analysis & Specification)
    * **작성 이유 및 의존성:** `education-scenario.md`의 교육 목표와 `google-api-references.md`의 기술적 검토를 바탕으로, 실제로 "무엇을 만들 것인가"를 구체적으로 정의합니다. 이 문서는 AI가 개발해야 할 기능, 사용자 경험(시나리오), 시스템의 전체적인 구조를 이해하는 핵심 기반이 됩니다.
    * **주요 내용:** 간소화된 웹 애플리케이션 요구사항, "자동 개시" 옵션을 포함한 사용자 시나리오, 시스템 아키텍처(Mermaid 다이어그램)를 추가했습니다.

4. **페이지 디자인 정의 (`docs/page-design.md`):**
    * **SW 공학 단계:** UI/UX 설계 (UI/UX Design)
    * **작성 이유 및 의존성:** `requirement.md`에 정의된 기능과 사용자 시나리오를 사용자가 어떻게 화면을 통해 상호작용할 것인지 시각적으로 구체화합니다. AI가 프론트엔드를 구현할 때 일관된 사용자 경험을 제공하고, 필요한 UI 요소들을 빠짐없이 만들 수 있도록 안내합니다.
    * **주요 내용:** 웹 UI 레이아웃, 주요 구성 요소, 간단한 스타일 가이드, 로고 이미지 사용 등을 명시했습니다.

5. **AI 개발 가이드라인 수립 (`docs/development-guide.md`):**
    * **SW 공학 단계:** 아키텍처 설계 및 상세 설계 (Architectural & Detailed Design)
    * **작성 이유 및 의존성:** `requirement.md`와 `docs/page-design.md`에서 정의된 "무엇을"과 "어떻게 보일지"를 바탕으로, AI가 "어떻게 기술적으로 구현할 것인가"에 대한 청사진을 제공합니다. 프로젝트의 기술 스택, 디렉토리 구조, 주요 모듈 설계, 핵심 로직 구현 방향 등을 명시하여, AI가 체계적이고 일관된 방식으로 코드를 작성하도록 돕습니다.
    * **주요 내용:** 프로젝트 디렉토리 구조, FastAPI 백엔드 설계, LangGraph 워크플로우 설계 등을 포함합니다.

6. **문서화 표준 정립 (`docs/documentation-guide.md`):**
    * **SW 공학 단계:** 개발 표준 및 환경 설정 (Development Standards & Environment Setup) - 문서화 부분
    * **작성 이유 및 의존성:** 프로젝트가 진행됨에 따라 다양한 문서들(`education-scenario.md`부터 `development-guide.md`까지)이 생성되었으므로, 이 문서들을 포함한 모든 프로젝트 문서의 작성 및 관리 일관성을 유지하기 위한 표준이 필요합니다. 이는 AI와 사용자 모두가 문서를 효율적으로 활용하고, 프로젝트 정보를 명확하게 공유하는 데 기여합니다.
    * **주요 내용:** 각 문서의 역할과 업데이트 주기, 파일명 표기법(케밥 케이스), 태스크 문서 작성법 등을 명시했습니다.

7. **프로젝트 규칙 정의 (`.caretrules`):**
    * **SW 공학 단계:** 개발 표준 및 환경 설정 (Development Standards & Environment Setup) - AI 협업 규칙 부분
    * **작성 이유 및 의존성:** AI 어시스턴트가 프로젝트의 특성(`requirement.md`, `development-guide.md` 등에서 도출된)과 개발 프로세스(`documentation-guide.md`에서 정의된)를 더 깊이 이해하고, 사용자의 의도에 맞는 방식으로 작업을 수행하도록 돕는 맞춤형 규칙 세트입니다.
    * **주요 내용:** 프로젝트 개요, 아키텍처, 개발 프로세스(문서화 우선, 태스크 기반 개발 등)를 정의했습니다.

8. **세부 실행 계획 수립 (태스크 문서 `docs/tasks/`):**
    * **SW 공학 단계:** 작업 분할 및 계획 (Work Breakdown & Planning)
    * **작성 이유 및 의존성:** 앞서 정의된 모든 문서를 종합하여, 실제 개발 작업을 구체적이고 실행 가능한 작은 단위의 태스크로 나눕니다. 이는 AI가 단계적으로 작업을 수행하고 진행 상황을 명확하게 추적할 수 있도록 하며, 복잡한 프로젝트를 체계적으로 관리하고 각 단계별 목표를 달성하는 데 효과적입니다.
    * **주요 내용:**
        * `docs/tasks/task-list.md`: 전체 태스크 목록과 각 상세 태스크 문서로의 링크를 제공하는 요약 문서.
        * `docs/tasks/001-*.md` ~ `009-*.md`: 각 태스크별 목표, 상세 체크리스트, 관련 문서 등을 포함하는 개별 태스크 문서. 담당자는 "ai-어시스턴트 (Caret)"로 명시.
    * **특이 사항:** 1시간 30분 안에 완료할 수 있도록 태스크 범위를 축소하고, LangGraph 설정을 간소화했습니다. 

```mermaid
flowchart TD 

    subgraph AI와 함께 진화하는 문서화 프로세스 

    subgraph PRD_방식 [PRD 중심 개발 방식]
        A1["• PRD 기반 초기 사용"]
        A2["• 단방향 지시"]
        A3["• 문서 갱신 없음"]
        A4["• 속도 우선"]
    end

        USE["사용자 시나리오"]
        REQ["요구사항 명세 - 기능 및 비기능"]
        ARCH["시스템 아키텍처 / 개발 가이드 / 디자인 명세"]
        TASK["작업 리스트 - 업무 맵"]
        GUIDE["문서화 가이드 및 AI 지침"]
        MARKDOWN["마크다운 문서 (.md)"]
        JSON["JSON 변환 → .rules 파일"]
        AI["AI 입력 및 중간 동기화, 반복 사용"]
        
        subgraph 철학 [핵심 철학]
            P1["• 문서는 기록이 아닌 AI와의 계약"]
            P2["• .rules는 명령이 아닌 지속 가능한 컨텍스트"]
        end
    end

     

    USE --> REQ --> ARCH --> TASK --> GUIDE --> JSON --> AI
    MARKDOWN --> JSON
    TASK -- "현행화 🔁" --> TASK
    GUIDE --> JSON
    철학 --> AI

```
### ✨ 문서 개선 과정 및 지속적인 현행화의 중요성

본 프로젝트에서 제공되는 요구사항 명세서, 개발 가이드, 페이지 디자인, 개별 태스크 문서 등 주요 산출물들은 단번에 완벽하게 작성된 것이 아닙니다. 이 문서들은 AI 어시스턴트(Caret)와의 긴밀한 협업, 여러 번의 개발 사이클, 실제 구현 과정에서의 테스트 및 디버깅, 그리고 그 과정에서 발생한 다양한 시행착오를 통해 지속적으로 개선되고 현행화된 결과물입니다.

특히, 1차 개발 사이클에서는 환경 설정, API 연동, LangGraph 워크플로우 설계, UI 피드백 처리 등 다양한 기술적 이슈와 커뮤니케이션 과정에서의 어려움이 있었습니다. 이러한 경험을 바탕으로 각 문서들은 교육생들이 프로젝트를 처음부터 진행할 때 발생할 수 있는 문제점들을 최소화하고, 보다 명확한 가이드라인을 제공할 수 있도록 보강되었습니다. 상세한 회고 및 문서별 개선 제안 내용은 [프로젝트 회고 및 문서 개선 보고서](./docs/project-retrospective-and-improvement-report.md)에서 확인하실 수 있습니다.

소프트웨어 개발, 특히 AI 에이전트와 같이 복잡하고 빠르게 변화하는 기술을 다루는 프로젝트에서는 초기 설계나 문서가 완벽할 수 없다는 점을 인지하는 것이 중요합니다. 실제 개발 과정 중에 AI와 적극적으로 소통하고, 발생한 이슈와 결정 사항들을 꾸준히 로깅하며, 이를 바탕으로 관련 문서를 살아있는 상태로 계속 업데이트하고 개선해나가는 것이 성공적인 프로젝트 수행과 효과적인 학습을 위한 핵심입니다.

## 📄 라이선스

이 프로젝트는 다음과 같은 라이선스 조건 하에 배포됩니다.

### 소스 코드 (Source Code)
- **영어 (English Original)**: [`LICENSE`](LICENSE)
- **한국어 (Korean Translation)**: [`LICENSE.ko.md`](LICENSE.ko.md)
  * (참고: 한국어 번역본은 이해를 돕기 위한 것이며, 법적 효력은 영어 원문을 따릅니다.)

### 에셋 (문서, 이미지, 미디어 등) (Assets - Documents, Images, Media, etc.)
- **영어 (English Original)**: [`ASSETS_LICENSE.md`](ASSETS_LICENSE.md)
- **한국어 (Korean Translation)**: [`ASSETS_LICENSE.ko.md`](ASSETS_LICENSE.ko.md)

콘텐츠를 재사용하거나 수정할 경우 원작자 표기를 유지해 주십시오.
