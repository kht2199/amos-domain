# AMOS Ontology Foundation

> 이 문서는 이제 AMOS 반도체 ontology의 active 기준 문서다.
> 기존 구현 저장소의 동명/유사 문서는 중복 방지를 위해 이 문서를 가리키는 포인터로만 유지한다.

## 목적
이 문서는 AMOS의 반도체/AMHS 도메인 온톨로지를 **현재 foundation에서 먼저 고정해야 할 개념/관계 중심**으로 정리한 active 문서다.

중요:
- 이 문서는 `docs/archive/` 아래의 과거 draft를 대체하는 현재 작업 기준이다.
- 이 문서는 ontology의 **의미와 경계**를 우선 정의한다.
- runtime vocabulary와 상태 canonical도 ontology boundary 안에서 함께 정리한다.
- 현재 foundation에서는 거대한 지식그래프보다 **운영에 바로 쓰이는 공통 의미 체계**를 우선한다.

---

## 현재 foundation에서 다루는 ontology 범위

### 1) Structure Layer
무엇이 존재하는지와 어떤 상위 개념 아래에 놓이는지 정의한다.

핵심 엔티티:
- `Carrier` — FOUP 단위 운반 객체
- `Lot` — 공정 추적 단위
- `Equipment` — AGV, STK, CNV, LFT 같은 설비의 상위 개념
- `Fab` — 제조 구역/공간 컨텍스트
- `Request` — 사용자의 작업/질의 단위
- `Task` — `Request`를 처리하기 위해 나눈 내부 작업 단위
- `ToolCall` — 내부/외부 실행 수단 호출 단위
- `Policy` — 허용/차단/확인/보류 판단 규칙 집합
- `ClarifyRequest` — 추가 정보 요청 구조
- `ExecutionTrace` — 요청 처리 과정을 복기 가능하게 남기는 실행 근거

참고:
- 구현별 로컬 이름, 클래스, event shape는 이 문서의 범위가 아니다.

현재 foundation에서는 장비를 세분화하되, 코드 영향은 최소화한다.
- `EquipmentType.AGV`
- `EquipmentType.STK`
- `EquipmentType.CNV`
- `EquipmentType.LFT`

표현은 enum으로 고정할 수도 있고 문자열로 유지할 수도 있으나, 의미는 위 4개 축으로 통일한다.

### 2) Runtime State Layer
현재 ontology가 구분해야 할 상태 축이 무엇인지 정의한다.

현재 foundation에서는 다음 상태 축을 구분한다.
- Carrier 위치/상태
- Lot 위치/상태
- Equipment 상태
- Request / Task / execution lifecycle 상태

참고:
- 이 문서는 Request / Task / execution lifecycle의 canonical 용어와 상태 집합도 함께 고정한다.

### 3) Event Layer
상태 변화와 판단 결과가 어떤 종류의 사건으로 표현되는지 정의한다.

현재 foundation에서 최소로 구분하는 사건 종류:
- request 수용
- task 분해/선택
- tool/action 호출
- clarify 요청/응답
- policy 판단
- state transition
- completion / blocked / failed 종료

참고:
- 실제 event 이름과 payload canonical은 engine/trace 문서에서 다룬다.
- ontology는 어떤 사건 종류가 존재해야 하는지를 먼저 정의한다.

### 4) Policy Layer
무엇을 할 수 있는지보다 **어떤 판단 축이 필요한지**를 정의한다.

현재 정책 예시:
- `completed` / `failed` 상태의 task는 함부로 되돌리지 않는다.
- 동시에 하나의 active task만 실행한다.
- `failed`가 발생했는데도 후속 실행을 억지로 이어가지 않는다.
- action approval 없으면 실행 금지
- 정보 부족 시 `clarifying` 우선

참고:
- Policy Layer는 domain이 안전 원칙과 불변식을 정의하는 위치다.
- 구체적인 decision shape와 permission 구조는 engine 문서에서 다룬다.

### 5) Runtime Vocabulary and Lifecycle
foundation ontology 안에서 구현 저장소들이 공통으로 맞춰야 할 최소 runtime 용어와 상태 집합을 함께 정리한다.

#### 최소 용어
##### Request
사용자의 질의 또는 실행 요청을 표현하는 최상위 작업 단위.

- 시작점: 사용자 입력 또는 시스템이 수용한 명시적 요청
- 포함 가능 요소: intent, target entity, time scope, constraints, clarify 결과, execution outcome
- 구현 포인트: 각 구현 저장소는 request ID, stream ID, tracking ID 등 서로 다른 로컬 식별자를 둘 수 있지만, domain 의미는 동일하게 `하나의 요청 단위`로 본다.

##### Task
`Request`를 처리하기 위해 나눈 내부 작업 단위.

- `Request` 하나가 여러 `Task`를 가질 수 있다.
- `Task`는 supervisor가 나누는 내부 실행 단위이며, 사용자에게 항상 직접 노출될 필요는 없다.
- 구현 포인트: 어떤 저장소는 `Task` 대신 `PlanStep`, job, worker assignment 같은 이름을 쓸 수 있다. 이 경우에도 domain 관점에서는 `Request` 아래의 내부 처리 단위인지가 중요하다.

##### ToolCall
시스템이 외부 또는 내부 실행 수단을 호출한 한 번의 행위.

- 예: API 호출, DB 조회, 검색, 액션 요청
- 최소 포함 의미: 무엇을 호출했는가, 어떤 입력으로 호출했는가, 성공/실패 여부가 어떠한가
- 구현 포인트: 함수 호출, REST endpoint, service method 등 형태는 달라도 실행 수단 호출이라는 의미를 공유해야 한다.

##### Policy
실행 가능/불가/추가 확인 필요 여부를 판단하는 규칙 집합.

- 예: 승인 없는 action 금지, 정보 부족 시 clarify 우선, 위험한 실행 전 confirm 필요
- `Policy`는 단순 validation과 달리 허용/거부/확인/보류 판단을 포함한다.
- 구현 포인트: guard, permission, approval, risk check 등으로 분산 구현될 수 있다.

##### ClarifyRequest
정보 부족 또는 모호성 때문에 사용자에게 추가 입력을 요청하는 구조.

- 발생 조건: missing slot, ambiguous target, insufficient scope, conflicting intent
- 최소 포함 의미: 왜 추가 정보가 필요한가, 무엇을 물어보는가, 선택지가 있는가
- 구현 포인트: 단순 문자열 질문만 있더라도 domain 관점에서는 `추가 정보 요청` 구조로 본다.

##### ExecutionTrace
요청이 왜 특정 경로로 진행되었는지 복기할 수 있게 해 주는 구조화된 실행 근거.

- 최소 포함 의미: request intent, 주요 판단, tool call 결과, 최종 상태
- 목적: replay, debugging, explainability, KPI, failure analysis
- 구현 포인트: event stream, trace entry, request log 등 여러 형태로 구현될 수 있다.

#### 최소 상태 집합
foundation에서 canonical로 맞출 최소 상태는 아래 6개다.

- `pending`
- `clarifying`
- `executing`
- `completed`
- `blocked`
- `failed`

#### 상태 의미
##### pending
아직 실행 전이지만 유효한 요청/작업으로 수용된 상태.

- 예: 요청이 접수되었고 아직 supervisor 판단 또는 실행 시작 전

##### clarifying
추가 정보가 필요해 사용자의 응답을 기다리는 상태.

- 핵심 차이: 시스템이 멈춘 것이 아니라, 정보 부족 때문에 다음 실행을 의도적으로 보류한 상태

##### executing
실행 계획 수립 또는 실제 tool/action 수행이 진행 중인 상태.

- 내부적으로 여러 세부 단계가 있어도 foundation canonical 수준에서는 하나의 실행 상태로 본다.

##### completed
도메인 관점에서 요청/작업이 정상 종료된 상태.

- 응답 제공 완료, 필요한 실행 완료, 더 이상의 clarify/재시도 필요 없음

##### blocked
실행 불가 조건 때문에 멈춘 상태.

- 예: policy deny, permission 부족, 승인 미충족, 안전상 차단
- `clarifying`와의 차이: 추가 정보만으로 바로 풀리는 문제가 아니라 정책/권한/외부 조건에 막힌 상태

##### failed
실행을 시도했지만 오류나 실패로 정상 종료하지 못한 상태.

- 예: tool failure, unexpected exception, unrecoverable dependency error
- `blocked`와의 차이: `blocked`는 실행 전/중 정책상 멈춤이고, `failed`는 시도 결과 실패가 발생한 상태다.

#### 구분 원칙
##### clarify vs blocked
- `clarifying`: 사용자 추가 정보가 오면 같은 요청 흐름을 계속 진행할 수 있다.
- `blocked`: 정책/권한/승인/운영 조건이 해결되기 전에는 진행할 수 없다.

##### blocked vs failed
- `blocked`: 멈춤의 원인이 정책/권한/안전 제약이다.
- `failed`: 멈춤의 원인이 실행 실패다.

##### pending vs executing
- `pending`: 아직 본격 실행 전
- `executing`: 내부 판단 또는 호출이 실제 진행 중

#### 구현 저장소에 대한 기대
각 구현 저장소는 concept-to-code mapping 문서에서 최소한 아래를 명시한다.

1. 위 용어가 로컬에서 어떤 이름으로 나타나는지
2. 위 6개 상태를 로컬 상태/event/API에 어떻게 매핑하는지
3. 없는 경우 무엇이 비어 있는지
4. 부분 대응인 경우 어디까지 일치하는지

---

## 핵심 관계

### 도메인 관계
- `Carrier` is transported by `Equipment`
- `Carrier` may contain or represent work for `Lot`
- `Lot` is processed in `Fab`
- `Equipment` belongs to `Fab`

### 처리 관계
- `Request` creates one or more `Task`
- one `Task` belongs to one `Request`
- `Task` may invoke one or more `ToolCall`
- `Policy` constrains `Request`, `Task`, and `ToolCall`
- `ClarifyRequest` may be produced while interpreting or executing a `Request`
- `ExecutionTrace` records why a `Request` changed state or path

---

## 사용자 질의 해석 기준
현재 foundation에서는 자연어 요청을 아래 ontology 축으로 해석한다.

### 1. Intent
- `query_status`
- `query_location`
- `query_history`
- `query_metric`
- `query_system_status`
- `execute_action`
- `clarify_response`

### 2. Target Entity
- `Carrier`
- `Lot`
- `Equipment`
- `Fab`
- `System`
- `Request`

### 3. Target Scope
- 단일 ID
- 특정 장비 타입 전체
- 특정 fab 범위
- 최근 n분/n시간
- 전체 시스템

### 4. Required Missing Slots
다음 정보가 비면 `clarifying` 또는 `ClarifyRequest` 후보다.
- carrier id
- lot id
- equipment type
- equipment id/name
- metric time range
- log keyword / level / time window
- action 대상

---

## 경계 원칙
- 이 문서는 ontology의 개념, 관계, 해석 축, 최소 경계를 정의한다.
- runtime vocabulary와 상태 canonical도 이 문서가 함께 소유한다.
- 이 문서는 구현 저장소별 클래스/필드/API 이름이나 concept-to-code mapping을 소유하지 않는다.
- 상세 ownership 경계 설명은 `../../README.md`를 따른다.

## 이후 확장 방향
다음은 다음 단계에서 ontology를 더 세분화할 때의 후보 축이다.
- OHT / bridge / port / rail / lift topology 세분화
- scenario / replay / synthetic data generation을 위한 추가 ontology
- fast predictor / full simulator 이원화에 필요한 상태/관계 축
- real data sync 기반 digital twin 연결 ontology
- ontology-driven routing hints
- trace taxonomy 고도화
