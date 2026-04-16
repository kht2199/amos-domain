# Foundation Engine

## 목표
현재 foundation에서는 simulator 전체를 만드는 것이 아니라, 이후 simulator/digital twin이 올라갈 수 있는 최소 엔진 개념을 먼저 정리한다.

## 먼저 볼 문서
- 도메인 객체/상태 기준: `../ontology/foundation.md`
- 요청 해석 축: `../ontology/request-interpretation.md`
- request lifecycle 기준: `../engine/request-lifecycle.md`
- active checklist: `../roadmap/ontology-implementation-checklist.md`
- 구현 저장소 참고 순서: `../programs/implementation-repo-reference-guide.md`
- later expansion reference: `../engine/supervisor-structure.md`, `../engine/execution-trace-structure.md`, `../engine/policy-permission-structure.md`

중요:
- 이 문서는 foundation 엔진의 의미 축을 설명한다.
- foundation에서 지금 당장 맞춰야 하는 self-check 우선순위는 `../roadmap/ontology-implementation-checklist.md`를 따른다.
- supervisor / execution trace / policy-permission 확장 문서는 later expansion reference로 선참고한다.
- 저장소별 class / schema / event / API / state 이름은 각 구현 저장소의 로컬 문서가 소유한다.

## 핵심 엔진 축
### 1. Request Engine
- 사용자 질의를 `Request` 단위로 받는다.
- tracking_id 등 로컬 식별자는 구현 저장소가 정하되, domain 관점에서는 하나의 요청 단위로 본다.
- request 상태 전이는 `../engine/request-lifecycle.md` 기준을 따른다.

### 2. Routing Engine
- supervisor가 intent / target / missing slots를 보고 worker를 선택한다.
- 정보가 부족하면 clarify 경로로 전환하고 추가 입력을 요청한다.
- 해석 축은 `../ontology/request-interpretation.md`를, supervisor 구조 확장의 domain 기준은 `../engine/supervisor-structure.md`를 따른다.

### 3. Execution Engine
- worker agent가 tool 호출을 수행한다.
- tool call / tool result를 구조화 이벤트로 남긴다.

### 4. Observation / Observability Engine
- `stream_start → plan_update → agent_start → tool_call/tool_result → trace → done|error` 흐름을 관찰 가능하게 남긴다.
- 관찰 단위는 request / stream / event / trace / request log다.
- 이 흐름은 단순 UI 표시용이 아니라 replay / debugging / KPI / routing 개선의 기반이 된다.
- execution trace 구조화의 domain 기준은 `../engine/execution-trace-structure.md`를 따른다. 다만 이 문서는 later expansion reference다.
- foundation에서는 최소한 아래를 측정 가능하게 하는 것을 목표로 한다.
  - first feedback latency
  - clarify rate
  - tool success/failure rate
  - route correction 및 failed path 복기 가능성

### 5. Policy Engine
- approval guard
- 동시에 하나의 active task만 실행
- `failed` 발생 시 추가 실행을 억지로 이어가지 않음
- 정보 부족 시 clarify를 우선 고려
- 정책/권한/안전 제약으로 진행 불가할 때는 `blocked`로 본다.
- policy / permission 구조화의 domain 기준은 `../engine/policy-permission-structure.md`를 따른다. 다만 이 문서는 later expansion reference다.

## runtime / observability 메모
- 도메인 객체/상태 기준은 `../ontology/foundation.md`를 따른다.
- 요청 해석 축은 `../ontology/request-interpretation.md`를 따른다.
- request lifecycle 상태 기준은 `../engine/request-lifecycle.md`를 따른다.
- 이 문서는 foundation 엔진 구조와 관찰 가능성 관점에서만 정리한다.
- 세부 이벤트 payload, 저장 포맷, 구현별 화면 구성은 각 구현 저장소에서 관리한다.
- 구현 저장소가 실제 반영 위치를 찾을 때는 `../programs/implementation-repo-reference-guide.md`와 각 저장소의 로컬 mapping 문서를 함께 본다.

## 비포함
- full discrete-event simulation
- OHT/bridge/rail topology solver
- real-time data sync
- fast predictor model serving

## 이후 확장 연결
foundation 엔진은 나중에 아래로 확장된다.
- scenario generator
- replay engine
- synthetic data pipeline
- full simulator
- digital twin sync adapter
