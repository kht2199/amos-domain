# AMOS Ontology Phase 1

> 이 문서는 이제 AMOS 반도체 ontology의 active 기준 문서다.
> 기존 agent 저장소의 동명 문서는 중복 방지를 위해 이 문서를 가리키는 포인터로만 유지한다.

## 목적
이 문서는 AMOS의 반도체/AMHS 도메인 온톨로지를 **실행 가능한 최소 단위**로 정리한 active 문서다.

중요:
- 이 문서는 `docs/archive/` 아래의 과거 draft를 대체하는 현재 작업 기준이다.
- ontology는 단순 용어집이 아니라 **routing, tool selection, trace, request log, phase 1 확장 설계**와 연결되어야 한다.
- Phase 1에서는 거대한 지식그래프보다 **운영에 바로 쓰이는 공통 의미 체계**를 우선한다.

## 현재 기준 문서
Phase 1 ontology의 정식 기준은 이 저장소의 domain 문서다.

- 개념/엔티티 정의: `./`
- 실행 원칙과 정책 의미: `../engine/`
- 저장소 역할 경계: `../programs/`
- 단계별 추진 순서: `../roadmap/`

구현 저장소는 이 기준을 참조해 각자 자신의 코드 구조에 맞는 개념 ↔ 코드 매핑표를 별도로 유지한다.
이 문서는 구현별 클래스/필드/API 이름을 정식 기준으로 소유하지 않는다.

## Phase 1 읽는 순서
Phase 1 정렬이 필요하면 아래 순서로 읽는다.

1. `minimal-runtime-model.md`
2. `../engine/practical-principles.md`
3. `../engine/phase1-engine.md`
4. `../roadmap/ontology-implementation-checklist.md`
5. `../programs/implementation-repo-reference-guide.md`
6. `../engine/supervisor-structure.md` *(Phase 2 expansion reference)*
7. `../engine/execution-trace-structure.md` *(Phase 2 expansion reference)*
8. `../engine/policy-permission-structure.md` *(Phase 2 expansion reference)*

중요:
- 이 문서는 Phase 1 ontology의 큰 그림과 의미 체계를 설명한다.
- Phase 1에서 지금 당장 맞춰야 하는 우선순위는 `../roadmap/ontology-implementation-checklist.md`를 따른다.
- `supervisor-structure.md`, `execution-trace-structure.md`, `policy-permission-structure.md`는 Phase 2 full expansion reference로 선참고한다.
- 저장소별 구현 매핑과 self-check 방식은 `../programs/implementation-repo-reference-guide.md` 및 각 구현 저장소의 로컬 문서가 소유한다.

---

## Phase 1에서 다루는 계층

### 1) Structure Layer
운영 대상이 무엇인지 정의한다.

핵심 엔티티:
- `Carrier` — FOUP 단위 운반 객체
- `Lot` — 공정 추적 단위
- `Equipment` — AGV, STK, CNV, LFT 같은 설비의 상위 개념
- `Fab` — 제조 구역/공간 컨텍스트
- `Request` — 사용자의 작업/질의 단위
- `Task` — `Request`를 처리하기 위해 나눈 내부 작업 단위
- `ExecutionTrace` — 요청 처리 과정을 복기 가능하게 남기는 실행 근거

참고:
- 구현 저장소는 `Task`를 `PlanStep` 같은 로컬 이름으로 표현할 수 있다.
- 구현 저장소는 `ExecutionTrace`를 `TraceEntry[]`, request log, event stream 같은 형태로 구현할 수 있다.
- canonical 용어 정의는 `minimal-runtime-model.md`를 따른다.

Phase 1에서는 장비를 세분화하되, 코드 영향은 최소화한다.
- `EquipmentType.AGv`
- `EquipmentType.STK`
- `EquipmentType.CNV`
- `EquipmentType.LFT`

표현은 enum으로 고정할 수도 있고 문자열로 유지할 수도 있으나, 의미는 위 4개 축으로 통일한다.

### 2) Runtime State Layer
현재 시점 상태를 정의한다.

Phase 1에서는 다음 세 층을 구분한다.
- Carrier 위치/상태
- Lot 위치/상태
- Equipment 상태
- Request / Task / execution lifecycle 상태

참고:
- Request / Task / execution lifecycle의 canonical 용어와 상태 집합은 `minimal-runtime-model.md`에서 고정한다.
- 각 구현 저장소는 이 상태를 자신의 schema/event/API에 어떻게 매핑하는지 별도 문서로 관리한다.
- domain 문서에는 저장소별 경로/필드명을 정식 표로 유지하지 않는다.

### 3) Event Layer
상태 변화가 어떤 이벤트로 남는지 정의한다.

현재 AMOS 이벤트:
- `stream_start`
- `plan_update`
- `agent_start`
- `tool_call`
- `tool_result`
- `token`
- `clarify`
- `trace`
- `error`
- `done`

이벤트는 단순 UI 전송용이 아니라 다음 용도로 재사용 가능해야 한다.
- request log 조회
- replay
- debugging
- routing 개선
- KPI 계산

### 4) Policy Layer
무엇을 할 수 있는지보다 **어떻게 안전하게 할지**를 정의한다.

현재 정책 예시:
- `completed` / `failed` 상태의 task는 함부로 되돌리지 않는다.
- 동시에 하나의 active task만 실행한다.
- `failed`가 발생했는데도 후속 실행을 억지로 이어가지 않는다.
- action approval 없으면 실행 금지
- 정보 부족 시 `clarifying` 우선

참고:
- Policy Layer는 domain이 안전 원칙과 불변식을 정의하는 위치다.
- supervisor 규칙이나 approval guard의 구체 구현은 각 구현 저장소에서 관리한다.

### 5) Evaluation Layer
실행이 잘 되었는지 판단하는 기준이다.

Phase 1 핵심 KPI 후보:
- clarify rate
- first feedback latency
- tool success/failure rate
- request completion rate
- route correction 빈도
- trace 기준 실패 유형 분포

참고:
- KPI와 관찰 지표의 구현 수집 방식은 각 구현 저장소의 추적/관측 문서에서 구체화한다.
- domain 문서는 어떤 지표가 필요한지와 왜 필요한지를 정의한다.

---

## 핵심 관계

### 도메인 관계
- `Carrier` is transported by `Equipment`
- `Carrier` may contain or represent work for `Lot`
- `Lot` is processed in `Fab`
- `Equipment` belongs to `Fab`

### 실행 관계
- `Request` creates one or more `Task`
- `Supervisor` selects the next `Task`
- one `Task` is executed by one worker agent
- worker execution contributes to `ExecutionTrace`
- tool execution emits `tool_call` / `tool_result`
- request lifecycle emits SSE events and request log events

### 관찰 관계
- `ExecutionTrace` explains why a route or state changed
- `RequestLog` is one implementation form for operational evidence
- KPI is derived from request/event/trace history

---

## 사용자 질의 해석 기준
Phase 1에서는 자연어를 아래 축으로 해석한다.

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

## 구현 연결 원칙

### supervisor / state / trace / request log / tool 연결
- ontology는 supervisor가 무엇을 step으로 보고,
- 어떤 상태 전이를 허용하며,
- 언제 clarify가 필요한지,
- 어떤 이벤트/trace를 남겨야 하는지,
- 어떤 실행 수단이 어떤 개념과 연결되는지를 정의하는 기준이다.

단, 구체적인 파일 경로, 클래스명, 필드명, API 이름, tool 등록 구조는 각 구현 저장소가 자신의 active 문서와 코드에서 관리한다.

---

## Phase 1 실전 원칙
1. ontology는 문서만 예쁘게 만들지 않는다.
2. 새 용어를 추가하면 반드시 다음 중 하나와 연결한다.
   - supervisor rule
   - schema
   - trace/event
   - 실행 수단 또는 관찰 지점
   - KPI
3. 모호한 개념은 바로 세분화하지 않는다. 먼저 운영상 필요한 구분만 만든다.
4. archive 문서는 참고 자료일 뿐 active source가 아니다.
5. 사용자 질문/trace/request log에서 반복적으로 등장하는 개념만 ontology로 승격한다.
6. action은 정보 질의와 동일선상에 두지 않고 approval/policy를 별도 취급한다.
7. ontology 변경은 README/GUIDE 용어표와 런타임 규칙 문서 간 불일치가 생기지 않게 반영한다.

---

## Phase 1 이후 확장 방향
다음은 다음 단계에서 확장한다.
- OHT / bridge / port / rail / lift topology 세분화
- scenario / replay / synthetic data generation
- fast predictor / full simulator 이원화
- real data sync 기반 digital twin
- ontology-driven routing hints
- trace taxonomy 고도화

하지만 지금 Phase 1의 우선순위는 다음이다.
1. 현재 AMOS runtime 의미 체계 정렬
2. source-of-truth 문서 명확화
3. trace / event / policy / tool mapping 일관화
4. 이후 시뮬레이터/디지털트윈 확장을 위한 최소 기반 확보
