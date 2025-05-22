# 태스크-001: 프로젝트-초기-설정

**id:** 001
**태스크-명칭:** 프로젝트-초기-설정
**상태:** to-do
**담당자:** ai-어시스턴트 (Caret)
**관련-문서:**
* `docs/development-guide.md` (섹션 2. 프로젝트 설정 및 환경)
* `docs/documentation-guide.md` (섹션 3. 표기법)

## 1. 목표 (goal)

프로젝트 개발을 시작하기 위한 기본적인 디렉토리 구조, 가상 환경, 필수 의존성, 초기 설정 파일들을 준비합니다.

## 2. 체크리스트 (checklist)

- [ ] 프로젝트 루트 디렉토리 (`multi-post-agent`) 내에 `docs/development-guide.md`에 명시된 주요 디렉토리들 (`app`, `static`, `templates`, `docs/tasks` 등)을 생성한다.
    - [ ] `app` 하위 디렉토리 (`routers`, `services`, `models`, `core`, `graph`) 생성.
- [ ] Python 가상 환경 (`venv`)을 생성하고 활성화한다. (활성화는 사용자가 직접 수행)
- [ ] `requirements.txt` 파일을 생성하고, 초기 필수 의존성 (fastapi, uvicorn, python-dotenv, langchain-google-genai)을 명시한다.
    * `fastapi`
    * `uvicorn[standard]`
    * `python-dotenv`
    * `langchain-google-genai` (Gemini API 사용)
- [ ] `pip install -r requirements.txt` 명령으로 의존성을 설치한다.
- [ ] `.env.template` 파일을 생성하고, 필요한 환경 변수 목록 (GEMINI_API_KEY, GOOGLE_CLIENT_ID, GOOGLE_CLIENT_SECRET, BLOG_ID)과 예시 값을 작성한다. (실제 값은 `.env` 파일에 작성)
- [ ] `.gitignore` 파일을 생성하고, `venv/`, `__pycache__/`, `*.pyc`, `.env` 등을 추가한다.

## 3. 진행-상황-및-특이사항 (progress-and-notes)

* (여기에 진행하면서 발생하는 특이사항이나 결정 사항 등을 기록합니다.)
* 가상 환경 활성화(`venv\Scripts\activate` 또는 `source venv/bin/activate`)는 사용자가 직접 터미널에서 수행해야 합니다.
* `.env` 파일에는 실제 API 키와 ID를 입력해야 합니다.

## 4. 다음-태스크 (next-task)

* [태스크-002: 오류-처리-및-로깅](./002-error-handling-and-logging.md)
