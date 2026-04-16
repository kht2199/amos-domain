# Execution Trace Structure

## 목적
이 문서는 AMOS Expansion에서 `ExecutionTrace`를 단순 디버그 로그가 아니라, **운영 복기와 explainability를 위한 구조적 근거**로 확장할 때 domain 수준에서 무엇을 고정해야 하는지 정리한다.

중요:
- 이 문서는 **후속 execution trace 확장 기준**을 설명한다.
- 다만 Foundation에서도 구현 저장소가 request 단위 복기 가능성을 self-check 할 수 있도록 reference contract로 선참고한다.
- Foundation에서 우선 맞춰야 하는 범위와 순서는 `../roadmap/ontology-implementation-checklist.md`를 따른다.
- 이 문서는 특정 저장소의 event payload, DB schema, JSON field를 canonical로 고정하지 않는다.
- 이 문서의 역할은 어떤 판단/실행 근거가 trace에 남아야 하는지, 그 근거가 `Request` / `Task` / `ToolCall` / `Policy` / `ClarifyRequest`와 어떻게 연결되는지 정의하는 것이다.
- 구체 구현은 각 저장소가 자신의 event stream / request log / trace entry 구조에서 관리하며, ownership boundary는 `../ontology/source-of-truth.md`를 따른다.

## 위치
`ExecutionTrace`는 AMOS에서 다음 위치를 가진다.
- 입력 맥락: `Request`
- 내부 연결: `Task`, `ToolCall`, `Policy`, `ClarifyRequest`
- 출력 역할: 왜 특정 경로와 상태가 선택되었는지 복기 가능하게 남김
- 재사용 목적: replay, debugging, KPI, explainability, failure analysis

즉 trace는 부수 로그가 아니라, **요청 처리의 운영 증거**다.

## domain이 고정하는 최소 trace 축
Expansion에서 `ExecutionTrace`는 최소한 아래 정보를 담을 수 있어야 한다.

### 1. `request_intent`
요청이 어떤 intent로 해석되었는지.

예:
- `query_status`
- `query_location`
- `query_history`
- `query_metric`
- `query_system_status`
- `execute_action`
- `clarify_response`

의미:
- 나중에 왜 이 worker/tool/policy 경로가 선택됐는지 설명하는 출발점이다.

### 2. `entity_candidates`
요청에서 고려된 대상 엔티티 후보.

예:
- `Carrier`
- `Lot`
- `Equipment`
- `Fab`
- `System`
- `Request`

의미:
- trace는 최종 선택된 대상만이 아니라, 어떤 후보가 고려되었는지도 남길 수 있어야 한다.
- ambiguity와 clarify 이유를 설명하는 핵심 근거다.

### 3. `missing_slots`
실행 전에 부족했던 정보.

예:
- carrier id
- lot id
- equipment id / name
- metric time range
- log keyword / time window
- action target

의미:
- 왜 `clarifying`으로 갔는지 설명하는 직접 근거다.
- clarify를 문자열 질답이 아니라 구조적 운영 판단으로 남기게 한다.

### 4. `policy_decision`
실행 가능 여부에 대한 정책 판단 결과.

대표 결과 예:
- `allow`
- `deny`
- `confirm`
- `clarify`

의미:
- 같은 request라도 정보 부족인지, 승인 부족인지, 안전 제약인지 구분할 수 있어야 한다.
- `blocked`와 `clarifying`을 구분하는 핵심 축이다.

참고:
- exact enum/value는 구현 저장소가 정할 수 있다.
- 하지만 domain 관점에서는 policy 판단 결과가 trace에 남아야 한다는 점이 중요하다.

### 5. `task_transitions`
요청 내부에서 어떤 `Task`가 생성/선택/진행/종료되었는지.

예:
- task created
- task selected by supervisor
- task execution started
- task completed
- task failed

의미:
- `Request` 내부 실행 흐름을 복기할 수 있어야 한다.
- 구현 저장소가 `Task` 대신 `PlanStep` 같은 로컬 이름을 쓰더라도 domain 의미는 동일하다.

### 6. `tool_call_results`
실제 실행 수단 호출과 그 결과.

예:
- 어떤 tool/API/service를 호출했는가
- 호출 성공/실패 여부
- 재시도 또는 fallback 여부

의미:
- `failed`와 `blocked`를 혼동하지 않게 해 준다.
- 운영 복기와 장애 분석의 핵심 증거다.

### 7. `state_transition_reason`
왜 특정 상태로 갔는지에 대한 구조화 근거.

예:
- `pending -> clarifying`: missing slot 발생
- `pending -> blocked`: approval 미충족
- `executing -> failed`: tool failure
- `executing -> completed`: 필요한 조회/실행 성공 종료

의미:
- trace는 단순 이벤트 나열이 아니라, 상태 전이 이유를 복기 가능하게 해야 한다.

### 8. `final_outcome`
요청이 최종적으로 어떻게 끝났는지.

예:
- `completed`
- `clarifying`
- `blocked`
- `failed`

의미:
- 요청 종료 상태와 그 직전 근거를 함께 읽을 수 있어야 한다.
- completion rate, clarify rate, blocked rate, failed rate 같은 KPI의 기반이 된다.

## 최소 execution trace semantic structure
Expansion에서 trace는 로컬 구현 이름과 무관하게 최소한 아래 의미를 재구성할 수 있어야 한다.

### 필수 의미 축
- `request_ref`
  - 어떤 요청 흐름의 trace인지 식별할 수 있는 참조
- `request_intent`
  - 요청을 어떤 intent로 해석했는지
- `entity_candidates`
  - 어떤 엔티티 후보를 고려했는지
- `missing_slots`
  - 무엇이 비어 있었는지 또는 어떤 ambiguity가 있었는지
- `policy_decision`
  - allow/deny/confirm/clarify 등 정책 판단 결과
- `supervisor_decision`
  - execute/clarify/blocked/failed 중 어떤 분기가 선택되었는지
- `task_transitions`
  - 어떤 task가 생성/선택/진행/종료되었는지
- `tool_call_results`
  - 어떤 실행 수단을 호출했고 어떤 결과가 났는지
- `state_transition_reasons`
  - 왜 상태가 바뀌었는지에 대한 구조적 근거
- `final_outcome`
  - 최종 종료 상태와 종료 이유 요약

### 선택 가능 필드
- `trace_entries`
  - 구현 저장소가 event/step 단위로 더 잘게 남기는 항목들
- `timestamps`
  - 시작/전이/종료 시각
- `durations`
  - 판단/실행/대기 시간
- `confidence`
  - intent/entity 해석 자신도
- `policy_actions`
  - approval 요청, confirm 대기 같은 후속 액션
- `artifacts`
  - 응답 payload, 조회 결과, 오류 메시지, 참조 링크 등

중요:
- domain은 위 필드명 자체를 강제하지 않는다.
- 아래 항목은 단일 JSON/object/schema를 강제하는 계약이 아니라 semantic checklist다.
- 구현 저장소는 위 의미를 request log / event stream / trace entry 중 적절한 곳에 남기거나, 여러 구조를 통해 재구성 가능하게 만들면 된다.
- trace는 단순 로그 수집이 아니라, 요청 결과를 나중에 explain/replay/audit 가능하게 만드는 구조여야 한다.

## 최소 trace 흐름
Expansion에서 trace는 아래 흐름을 복기 가능하게 해야 한다.

1. `Request` 수용
2. `request_intent` 해석
3. `entity_candidates` 추출
4. `missing_slots` 및 ambiguity 판단
5. `policy_decision` 확인
6. `supervisor_decision` 기록
7. `Task` 생성/선택
8. `ToolCall` 실행 및 결과 기록
9. 상태 전이 이유 기록
10. `final_outcome` 기록

## 상태와의 연결
`ExecutionTrace`는 최소 runtime state와 직접 연결된다.

- `pending`
  - 아직 판단/실행 전이거나 접수 직후 상태
  - 최소한 request 수용 사실과 초기 해석 시도가 남아 있어야 한다.
- `clarifying`
  - `missing_slots` 또는 ambiguity가 원인임을 trace로 설명 가능해야 함
  - 이때 trace에는 어떤 slot이 비었고 어떤 clarify 요청이 만들어졌는지가 남아야 한다.
- `executing`
  - 어떤 `Task` / `ToolCall`이 진행 중인지 trace에서 복기 가능해야 함
  - 이때 trace에는 supervisor 결정과 실행 대상으로 넘어간 근거가 연결돼야 한다.
- `blocked`
  - `policy_decision` 또는 permission / approval 제약이 원인임을 남겨야 함
  - blocked는 단순 미실행이 아니라 왜 진행하면 안 되는지 설명 가능한 상태여야 한다.
- `failed`
  - tool/dependency/error 원인이 무엇인지 남겨야 함
  - failed는 정책 차단과 달리 오류/장애/해석 실패라는 점이 trace에 구분돼야 한다.
- `completed`
  - 어떤 결과가 정상 종료를 만들었는지 남겨야 함
  - completed에는 마지막 응답/결과와 그 직전 핵심 판단 근거가 함께 남아야 한다.

## 최소 trace entry 기대치
구현 저장소는 단일 trace 문서이든 event stream이든 최소한 아래 수준을 재구성 가능하게 해야 한다.

1. 요청 접수 정보
2. intent 해석 결과
3. entity 후보와 ambiguity 정보
4. missing slot 또는 policy 판단 결과
5. supervisor decision
6. task / tool 실행 기록
7. 상태 전이 이유
8. 최종 outcome

중요:
- 위 항목이 하나의 JSON 객체일 필요는 없다.
- 하지만 운영자가 사후에 읽었을 때 “왜 이 요청이 이 상태로 끝났는가”를 재구성할 수 있어야 한다.

## supervisor와의 연결
supervisor는 trace의 핵심 생산자 중 하나다.

최소한 아래 판단이 trace와 연결되어야 한다.
- 어떤 `intent`로 해석했는가
- 어떤 `entity_candidates`를 고려했는가
- 어떤 `missing_slots`를 발견했는가
- 어떤 `policy_decision`을 내렸는가
- 왜 `Task`를 생성하거나 `ClarifyRequest`를 만들었는가

즉 supervisor 구조 확장은 trace 구조화와 분리될 수 없다.

## KPI와의 연결
`ExecutionTrace`는 나중에 아래 지표 계산의 근거가 된다.
- clarify rate
- completion rate
- blocked rate
- failed rate
- tool success/failure rate
- route correction 빈도
- 반복 missing slot 유형
- 반복 policy deny / confirm 패턴

## 구현 저장소에 대한 기대
각 구현 저장소는 concept-to-code mapping 또는 로컬 설계 문서에서 최소한 아래를 명시한다.

1. 위 trace 축이 로컬에서 어떤 event / log / entry 이름으로 나타나는지
2. `Task`와 `ToolCall` 결과가 어떤 식으로 연결되는지
3. `clarifying` / `blocked` / `failed` 이유가 어디에 남는지
4. request log, event stream, trace entry 중 무엇이 canonical `ExecutionTrace` 대응물인지

## 비목표
이 문서는 아래를 직접 고정하지 않는다.
- 특정 event type enum
- 특정 JSON payload 필드명
- 특정 로그 저장소 포맷
- 특정 DB schema
- 특정 UI trace viewer 설계

이 문서의 목적은 구현 강제가 아니라, **AMOS에서 trace가 무엇을 증명해야 하는지의 공통 의미 구조**를 고정하는 것이다.
