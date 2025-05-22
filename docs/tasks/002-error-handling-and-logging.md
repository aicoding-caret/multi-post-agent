# 태스크-002: 오류-처리-및-로깅 (error-handling-and-logging)

**id:** 002
**태스크-명칭:** 오류-처리-및-로깅-추가
**상태:** to-do
**담당자:** ai-어시스턴트 (Caret)
**관련-문서:**
* `docs/development-guide.md` (섹션 6. 주요 기술적 고려사항 - 로깅)
* 모든 `app/` 하위 파이썬 파일 (특히 `main`)

## 1. 목표 (goal)

애플리케이션의 안정성을 높이고 문제 발생 시 원인 파악을 용이하게 하기 위해, Python의 `logging` 모듈을 사용하여 주요 이벤트 및 오류 정보를 기록합니다.

## 2. 체크리스트 (checklist)

- [ ] `app/main.py` 파일 생성 및 FastAPI 기본 애플리케이션 설정.
- [ ] `app/main.py`에 Python `logging` 모듈 기본 설정:
    * 로그 레벨 설정 (예: `INFO` 또는 `DEBUG`).
    * 로그 포맷 설정 (예: 타임스탬프, 로그 레벨, 모듈명, 메시지).
    * 로그 핸들러 설정 (예: 콘솔 출력 `StreamHandler`).
- [ ] `app/main.py` 상단에 로거 인스턴스 생성 (`logger = logging.getLogger(__name__)`).
- [ ] `app/main.py`의 주요 함수 시작/종료, 중요한 분기점에 로깅 구문 추가:
    * `logger.info()`: 일반 정보, 함수 호출 등.
    * `logger.error()`: 오류 발생 시 (예외 처리 블록 내).

## 3. 진행-상황-및-특이사항 (progress-and-notes)

* (여기에 진행하면서 발생하는 특이사항이나 결정 사항 등을 기록합니다.)

## 4. 다음-태스크 (next-task)

* [태스크-003: blogger-api-연동-및-oauth-인증](./003-blogger-api-and-oauth.md)
