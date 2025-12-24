# [Coner] 1224 스터디
> 20240789 이지현 | node.js 2팀 

## 11.1 테스트 준비하기
-  수작업 테스트의 한계 극복, 자동화를 통한 효율성 증대, 리팩토링 시 안정성 확보.
- 도구: Jest (페이스북 오픈 소스, 테스팅 툴 통합 패키지).
- 환경 설정: npm i -D jest, package.json의 scripts에 test: "jest" 등록.
- 파일 규칙: 파일명.test.js 또는 파일명.spec.js.

## 11.2 유닛 테스트 (Unit Test)
- 정의: 작은 단위의 함수나 모듈이 의도대로 정확히 작동하는지 검증하는 테스트.
- 모킹 (Mocking): 테스트 환경에서 실제 객체나 함수(Express의 req, res, DB 모델 등) 대신 가짜 객체/함수를 사용하는 행위.
    - 이유: 실제 환경 구축의 어려움 해소 및 외부 의존성 제거.
    - 방법: jest.fn()(가짜 함수 생성), jest.mock()(모듈 모킹).

- 주요 메서드:
    - describe: 테스트 그룹화.
    - test / it: 개별 테스트 케이스 정의.
    - expect: 실행 결과 검증.
    - toBeCalledTimes(n): 호출 횟수 검증.
    - toBeCalledWith(arg): 특정 인수와 함께 호출되었는지 검증.
    - mockReturnValue(value): 모킹된 함수의 반환값 지정.

## 11.3 테스트 커버리지 (Test Coverage)
- 정의: 전체 코드 중 테스트 코드가 실행된 비율을 나타내는 지표.
- 실행: jest --coverage.
- 주요 항목:
    - File: 파일/폴더명.
    - % Stmt: 구문(Statement) 커버 비율.
    - % Branch: if문 등 분기점 커버 비율.
    - % Funcs: 함수 실행 비율.
    - % Lines: 코드 줄 수 비율.
- 주의: 커버리지 100%가 무결점(Bug-free)을 보장하지 않음. 핵심 로직 위주의 테스트 작성이 중요.

## 11.4 통합 테스트 (Integration Test)
- 정의: 라우터, 미들웨어, DB 등이 유기적으로 상호작용하며 제대로 작동하는지 검증. 
- 도구: Supertest (Express 서버에 요청을 보내고 응답을 검증하는 라이브러리).
- 환경 분리:
    - app.js(서버 로직)와 server.js(포트 리스닝) 분리 필요 (Supertest 내부적 실행 위함).
    - 테스트용 데이터베이스 별도 구성 (config.json의 test 환경).
- 라이프사이클 함수 (Lifecycle Hooks):
    - beforeAll / afterAll: 전체 테스트 전후 실행 (DB 생성/삭제 등).
    - beforeEach / afterEach: 각 테스트 케이스 전후 실행.
- 특징: agent 객체를 사용하여 세션(로그인 상태)을 유지하며 연속적인 요청 테스트 가능.

## 11.5 부하 테스트 (Load Test)
- 정의: 서버가 감당할 수 있는 요청량(Throughput)과 동시 접속자 수를 예측 및 검증.
- 목적: OOM(Out of Memory) 방지, 병목 구간 파악.
- 도구: Artillery.

- 실행 방식:
    - CLI: npx artillery quick ... (간단한 테스트).
    - Config File: JSON/YAML 파일로 시나리오(Scenario) 작성 (사용자 흐름 모방).

- 주요 지표:
    - http.response_time: 응답 속도. 중간값(median)과 하위 95%(p95)의 차이가 적을수록 안정적.
- 최적화: 테스트 결과에 따라 클러스터링(Clustering), 캐싱(Caching), 스케일 아웃 등을 고려.
- 주의: 실제 서비스(Production) 환경에서 수행 시 서비스 중단 위험이 있으므로, 스테이징(Staging) 서버에서 수행 권장.

## 11.6 프로젝트 마무리
- 테스트 범위: 외부 패키지 자체는 테스트하지 않음(신뢰 가정). 자신의 코드와 외부 패키지를 사용하는 로직을 테스트.

- 기타 테스트:
    - 시스템 테스트 (System Test): QA가 전체 시스템 요구사항 검증.
    - 인수 테스트 (Acceptance Test): 실제 사용자 관점에서 서비스 사용성 검증.
- 결론: 테스트는 에러를 완전히 막지 못하지만, 코드에 대한 신뢰성을 높이고 리팩토링 시 사이드 이펙트(Side Effect)를 감지하는 데 필수적임. 