# AMOS Ontology Phase 1

> 이 문서는 이제 AMOS 반도체 ontology의 active 기준 문서다.
> 기존 agent 저장소의 동명 문서는 중복 방지를 위해 이 문서를 가리키는 포인터로만 유지한다.

## 목적
이 문서는 AMOS의 반도체/AMHS 도메인 온톨로지를 **실행 가능한 최소 단위**로 정리한 active 문서다.

중요:
- 이 문서는 `docs/archive/` 아래의 과거 draft를 대체하는 현재 작업 기준이다.
- ontology는 단순 용어집이 아니라 **routing, tool selection, trace, request log, phase 1 확장 설계**와 연결되어야 한다.
- Phase 1에서는 거대한 지식그래프보다 **운영에 바로 쓰이는 공통 의미 체계**를 우선한다.

## 현재 source of truth
AMOS의 ontology는 아직 한 파일에만 모여 있지 않다. 현재는 아래 파일들이 함께 source of truth 역할을 한다.

1. 용어/입문
   - `README.md`
   - `docs/GUIDE.md`
2. 실행 규칙
   - `docs/supervisor-rules.md`
3. 런타임 상태/trace 스키마
   - `app/api/graph/state.py`
   - `app/api/schemas.py`
4. tool ↔ 도메인 연결
   - `app/api/tools/loader.py`
5. 요청 추적/로그 구조
   - `docs/request-tracking-spec.md`

즉, 지금까지는 ontology가 **분산 관리**되고 있었다. 앞으로는 이 문서를 중심 축으로 두고, 나머지 문서는 이 문서를 구현/설명하는 구조로 맞춘다.

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
- `PlanStep` — supervisor가 만든 실행 단계
- `TraceEntry` — 실행 중 구조화 로그

Phase 1에서는 장비를 세분화하되, 코드 영향은 최소화한다.
- `EquipmentType.AGv`
- `EquipmentType.STK`
- `EquipmentType.CNV`
- `EquipmentType.LFT`

표현은 enum으로 고정할 수도 있고 문자열로 유지할 수도 있으나, 의미는 위 4개 축으로 통일한다.

### 2) Runtime State Layer
현재 시점 상태를 정의한다.

핵심 상태:
- Carrier 위치/상태
- Lot 위치/상태
- Equipment 상태
- PlanStep 상태
  - `pending`
  - `in_progress`
  - `done`
  - `failed`
- Request 상태
  - `streaming`
  - `done`
  - `error`

현재 구현 매핑:
- plan step 상태: `app/api/graph/agents/supervisor.py`, `app/api/schemas.py`
- request 상태: `app/api/main.py`
- trace 상태: `app/api/graph/state.py`

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
- done/failed step 불변
- 동시에 `in_progress` step 최대 1개
- failed 발생 시 `END`
- action approval 없으면 실행 금지
- 정보 부족 시 `CLARIFY`

현재 구현/설명 위치:
- `docs/supervisor-rules.md`
- `app/api/graph/agents/supervisor.py`

### 5) Evaluation Layer
실행이 잘 되었는지 판단하는 기준이다.

Phase 1 핵심 KPI 후보:
- clarify rate
- first feedback latency
- tool success/failure rate
- request completion rate
- route correction 빈도
- trace 기준 실패 유형 분포

관련 문서:
- `docs/request-tracking-spec.md`
- `docs/analytics-instrumentation-proposal.md`

---

## 핵심 관계

### 도메인 관계
- `Carrier` is transported by `Equipment`
- `Carrier` may contain or represent work for `Lot`
- `Lot` is processed in `Fab`
- `Equipment` belongs to `Fab`

### 실행 관계
- `Request` creates `PlanStep[]`
- `Supervisor` selects next `PlanStep`
- `PlanStep` is executed by one worker agent
- worker execution emits `TraceEntry[]`
- tool execution emits `tool_call` / `tool_result`
- request lifecycle emits SSE events and request log events

### 관찰 관계
- `TraceEntry` explains why a route or state changed
- `RequestLog` stores operational evidence
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
다음 정보가 비면 `CLARIFY` 후보다.
- carrier id
- lot id
- equipment type
- equipment id/name
- metric time range
- log keyword / level / time window
- action 대상

---

## 현재 코드와의 직접 연결

### supervisor
- `PlanStep`
- `RouteDecision`
- `AgentName`
- `CLARIFY`, `END`

파일:
- `app/api/graph/agents/supervisor.py`

의미:
- ontology는 supervisor가 무엇을 step으로 보고,
- 어떤 상태 전이를 허용하며,
- 언제 clarify가 필요한지 판단하는 기준이다.

### state / trace
파일:
- `app/api/graph/state.py`

핵심 타입:
- `AgentState`
- `TraceEntry`

의미:
- ontology는 trace를 사람이 읽는 로그가 아니라,
- 실행 객체와 상태 전이를 설명하는 구조화 데이터로 본다.

### request log / SSE
파일:
- `app/api/main.py`
- `app/api/schemas.py`
- `docs/request-tracking-spec.md`

의미:
- ontology는 요청을 단발성 응답이 아니라
- `stream_start → plan_update → agent_start → tool_call/tool_result → done` 흐름의 관찰 가능한 단위로 본다.

### tool loader
파일:
- `app/api/tools/loader.py`

의미:
- tool은 단순 API wrapper가 아니라
- 어떤 entity/intent에 연결되는지 분류된 실행 수단이다.
- Phase 1에서는 `agent → tool set` 관계를 안정화하는 것이 중요하다.

---

## Phase 1 실전 원칙
1. ontology는 문서만 예쁘게 만들지 않는다.
2. 새 용어를 추가하면 반드시 다음 중 하나와 연결한다.
   - supervisor rule
   - schema
   - trace/event
   - tool mapping
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
