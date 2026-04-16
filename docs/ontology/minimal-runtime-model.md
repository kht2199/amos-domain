# Minimal Runtime Model

## 목적
이 문서는 AMOS Phase 1에서 구현 저장소들이 공통으로 맞춰야 할 최소 runtime 용어와 상태 집합을 고정한다.

- domain의 역할: 의미와 경계를 정의한다.
- 구현 저장소의 역할: 이 의미를 자신의 schema / API / event / state에 어떻게 매핑할지 관리한다.
- 비목표: 특정 저장소의 클래스명, 필드명, 파일 경로를 canonical로 고정하지 않는다.

## 최소 용어
### Request
사용자의 질의 또는 실행 요청을 표현하는 최상위 작업 단위.

- 시작점: 사용자 입력 또는 시스템이 수용한 명시적 요청
- 포함 가능 요소: intent, target entity, time scope, constraints, clarify 결과, execution outcome
- 구현 포인트: 각 구현 저장소는 request ID, stream ID, tracking ID 등 서로 다른 로컬 식별자를 둘 수 있지만, domain 의미는 동일하게 `하나의 요청 단위`로 본다.

### Task
`Request`를 처리하기 위해 나눈 내부 작업 단위.

- `Request` 하나가 여러 `Task`를 가질 수 있다.
- `Task`는 supervisor가 나누는 내부 실행 단위이며, 사용자에게 항상 직접 노출될 필요는 없다.
- 구현 포인트: 어떤 저장소는 `Task` 대신 `PlanStep`, job, worker assignment 같은 이름을 쓸 수 있다. 이 경우에도 domain 관점에서는 `Request` 아래의 내부 처리 단위인지가 중요하다.

### ToolCall
시스템이 외부 또는 내부 실행 수단을 호출한 한 번의 행위.

- 예: API 호출, DB 조회, 검색, 액션 요청
- 최소 포함 의미: 무엇을 호출했는가, 어떤 입력으로 호출했는가, 성공/실패 여부가 어떠한가
- 구현 포인트: 함수 호출, REST endpoint, service method 등 형태는 달라도 실행 수단 호출이라는 의미를 공유해야 한다.

### Policy
실행 가능/불가/추가 확인 필요 여부를 판단하는 규칙 집합.

- 예: 승인 없는 action 금지, 정보 부족 시 clarify 우선, 위험한 실행 전 confirm 필요
- `Policy`는 단순 validation과 달리 허용/거부/확인/보류 판단을 포함한다.
- 구현 포인트: guard, permission, approval, risk check 등으로 분산 구현될 수 있다.

### ClarifyRequest
정보 부족 또는 모호성 때문에 사용자에게 추가 입력을 요청하는 구조.

- 발생 조건: missing slot, ambiguous target, insufficient scope, conflicting intent
- 최소 포함 의미: 왜 추가 정보가 필요한가, 무엇을 물어보는가, 선택지가 있는가
- 구현 포인트: 단순 문자열 질문만 있더라도 domain 관점에서는 `추가 정보 요청` 구조로 본다.

### ExecutionTrace
요청이 왜 특정 경로로 진행되었는지 복기할 수 있게 해 주는 구조화된 실행 근거.

- 최소 포함 의미: request intent, 주요 판단, tool call 결과, 최종 상태
- 목적: replay, debugging, explainability, KPI, failure analysis
- 구현 포인트: event stream, trace entry, request log 등 여러 형태로 구현될 수 있다.

## 관계 정리
- `Request`는 사용자 관점의 최상위 단위다.
- `Task`는 `Request` 내부 처리 단위다.
- `ToolCall`은 `Task` 수행 중 발생하는 실행 행위다.
- `Policy`는 `Request`/`Task`/`ToolCall`의 허용 조건을 판단한다.
- `ClarifyRequest`는 `Request` 처리 중 정보 부족 시 발생한다.
- `ExecutionTrace`는 위 판단과 실행 흐름을 복기 가능하게 기록한다.

## 최소 상태 집합
Phase 1에서 canonical로 맞출 최소 상태는 아래 6개다.

- `pending`
- `clarifying`
- `executing`
- `completed`
- `blocked`
- `failed`

## 상태 의미
### pending
아직 실행 전이지만 유효한 요청/작업으로 수용된 상태.

- 예: 요청이 접수되었고 아직 supervisor 판단 또는 실행 시작 전

### clarifying
추가 정보가 필요해 사용자의 응답을 기다리는 상태.

- 핵심 차이: 시스템이 멈춘 것이 아니라, 정보 부족 때문에 다음 실행을 의도적으로 보류한 상태

### executing
실행 계획 수립 또는 실제 tool/action 수행이 진행 중인 상태.

- 내부적으로 여러 세부 단계가 있어도 Phase 1 canonical 수준에서는 하나의 실행 상태로 본다.

### completed
도메인 관점에서 요청/작업이 정상 종료된 상태.

- 응답 제공 완료, 필요한 실행 완료, 더 이상의 clarify/재시도 필요 없음

### blocked
실행 불가 조건 때문에 멈춘 상태.

- 예: policy deny, permission 부족, 승인 미충족, 안전상 차단
- `clarifying`와의 차이: 추가 정보만으로 바로 풀리는 문제가 아니라 정책/권한/외부 조건에 막힌 상태

### failed
실행을 시도했지만 오류나 실패로 정상 종료하지 못한 상태.

- 예: tool failure, unexpected exception, unrecoverable dependency error
- `blocked`와의 차이: `blocked`는 실행 전/중 정책상 멈춤이고, `failed`는 시도 결과 실패가 발생한 상태다.

## 구분 원칙
### clarify vs blocked
- `clarifying`: 사용자 추가 정보가 오면 같은 요청 흐름을 계속 진행할 수 있다.
- `blocked`: 정책/권한/승인/운영 조건이 해결되기 전에는 진행할 수 없다.

### blocked vs failed
- `blocked`: 멈춤의 원인이 정책/권한/안전 제약이다.
- `failed`: 멈춤의 원인이 실행 실패다.

### pending vs executing
- `pending`: 아직 본격 실행 전
- `executing`: 내부 판단 또는 호출이 실제 진행 중

## 구현 저장소에 대한 기대
각 구현 저장소는 concept-to-code mapping 문서에서 최소한 아래를 명시한다.

1. 위 용어가 로컬에서 어떤 이름으로 나타나는지
2. 위 6개 상태를 로컬 상태/event/API에 어떻게 매핑하는지
3. 없는 경우 무엇이 비어 있는지
4. 부분 대응인 경우 어디까지 일치하는지

## 참고
- `phase1.md`는 Phase 1 ontology의 전체 문맥을 설명한다.
- 이 문서는 구현 저장소들이 공통으로 맞춰야 할 최소 runtime vocabulary와 lifecycle을 별도로 고정하기 위한 문서다.
