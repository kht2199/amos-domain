# Implementation Repo Reference Guide

## 목적
이 문서는 `agent`, `backend`, `frontend`, `glb-editor` 같은 구현 저장소가
AMOS domain 기준을 참고할 때 **무엇을 먼저 읽어야 하는지**, **어떤 기준으로 self-check 해야 하는지**를 안내한다.

중요:
- 이 문서는 구현 저장소의 구체 클래스명, 필드명, API 경로, 이벤트 이름을 canonical로 고정하지 않는다.
- 이 문서의 역할은 domain 저장소가 구현 세부안을 대신 설계하는 것이 아니라,
  각 저장소가 자기 코드와 로컬 문서 기준으로 자율적으로 반영할 수 있게 reference pack을 제공하는 것이다.
- 이 문서는 semantic contract 자체를 새로 정의하지 않는다.
- canonical 문서를 구현 저장소가 어떤 순서로 읽고 self-check 할지 안내하는 active reference guide다.
- 저장소별 세부 concept-to-code mapping 표는 각 구현 저장소가 소유한다.

## 기본 원칙
- domain은 **의미 / 경계 / 상태 / 판단 축**을 제공한다.
- 구현 저장소는 **실제 코드 / 테스트 / 로컬 문서 / concept-to-code mapping**을 소유한다.
- domain 저장소에는 저장소별 구현 매핑 표를 복제하지 않는다.
- 구현 저장소는 domain 기준을 참고하되, 실제 반영 방식은 각 저장소가 자율적으로 결정한다.

## 공통 읽는 순서
### 1. 최소 runtime vocabulary 확인
우선 읽을 문서:
- `../ontology/minimal-runtime-model.md`

확인할 것:
- 최소 runtime vocabulary와 canonical 상태 6개가 우리 저장소의 로컬 이름/상태와 어떻게 대응되는지

질문:
- 우리 저장소에서 위 개념은 어떤 로컬 이름으로 나타나는가?
- 상태 6개는 어디에서 어떻게 대응되는가?
- 없는 개념은 무엇인가?
- 부분 대응만 되는 것은 무엇인가?

### 2. supervisor 판단 축 확인
우선 읽을 문서:
- `../engine/supervisor-structure.md`

확인할 것:
- `intent`
- `entity_candidates`
- `missing_slots`
- `risk_level`
- `policy_precheck`
- `clarify_question`
- `clarify_options`

질문:
- 우리 저장소는 어떤 판단 축을 직접 다루는가?
- `clarifying / blocked / failed` 구분은 어디에서 표현되는가?
- supervisor 또는 그에 준하는 결정 계층이 어떤 출력 shape를 가지는가?

### 3. execution trace 기준 확인
우선 읽을 문서:
- `../engine/execution-trace-structure.md`

확인할 것:
- `request_intent`
- `entity_candidates`
- `missing_slots`
- `policy_decision`
- `task_transitions`
- `tool_call_results`
- `state_transition_reason`
- `final_outcome`

질문:
- 위 정보 중 무엇이 event / trace / request log / stream에 남는가?
- `clarifying / blocked / failed` 이유를 복기할 수 있는가?
- request 단위 복기가 가능한가?

### 4. policy / permission 기준 확인
우선 읽을 문서:
- `../engine/policy-permission-structure.md`

확인할 것:
- `risk_tier`
- `permission_context`
- `policy_precheck`
- `decision`
- `decision_reason`
- `confirmation_requirement`
- `block_condition`

canonical decision:
- `allow`
- `deny`
- `confirm`
- `clarify`

질문:
- 우리 저장소는 confirm과 clarify를 어떻게 구분하는가?
- blocked와 failed를 어떤 규칙으로 나누는가?
- approval / permission / safety 제약이 어디에 표현되는가?

### 5. Phase 1 checklist로 마무리 확인
우선 읽을 문서:
- `../roadmap/ontology-implementation-checklist.md`

확인할 것:
- 구현 저장소가 맞춰야 하는 개념 목록
- 최소 상태 집합
- supervisor / trace / policy / clarify 연결 우선순위

질문:
- 우리 저장소가 지금 당장 참고해야 할 항목은 무엇인가?
- 아직 구현하지 않더라도 concept-to-code mapping 문서에는 어떤 gap을 적어야 하는가?

## 구현 저장소가 자기 쪽에서 반드시 볼 것
각 구현 저장소는 아래를 자기 저장소 기준으로 확인한다.
- `AGENTS.md`
- `README.md`
- `docs/concept-code-mapping.md`
- 실제 코드
- 실제 테스트
- 실제 event / API / schema / state 정의

중요:
- 최종 구현 기준은 해당 저장소의 코드와 테스트다.
- domain 문서는 semantic reference이고, 로컬 구현 ownership을 대체하지 않는다.

## 저장소별 안내 방식
### agent
- 자연어 요청 해석, clarify, supervisor, trace, request log 자산을 본다.
- domain 문서를 보고 runtime vocabulary / 상태 / trace / policy 의미를 맞춘다.
- 다만 실제 class / state / event 이름은 agent 로컬 문서가 소유한다.

### backend
- 현실 API / action / permission / metric / log 경계를 본다.
- domain 문서를 보고 action safety, blocked vs failed, policy semantics를 맞춘다.
- 실제 endpoint / DTO / service naming은 backend 로컬 기준을 따른다.

### frontend
- 사용자가 보는 clarify / confirm / trace / KPI / explainability 표면을 본다.
- domain 문서를 보고 어떤 상태와 판단 축을 UI에서 드러내야 하는지 참고한다.
- 실제 화면 구조와 component state는 frontend 로컬 기준을 따른다.

### glb-editor
- simulation/digital twin 쪽 자산과 직접 연결되는 object / scene / state 표현을 본다.
- domain 문서를 보고 ontology 용어와 entity 의미를 참고한다.
- 실제 데이터 구조와 editor interaction은 glb-editor 로컬 기준을 따른다.

## domain 저장소가 여기서 해야 할 일
- reference 문서를 유지한다.
- 읽는 순서를 유지한다.
- self-check 질문을 유지한다.
- 저장소별 세부 구현안을 대신 작성하지 않는다.
- 저장소별 mapping 표를 여기로 복제하지 않는다.

## 관련 문서
- `../../AGENTS.md`
- `../ontology/minimal-runtime-model.md`
- `../engine/supervisor-structure.md`
- `../engine/execution-trace-structure.md`
- `../engine/policy-permission-structure.md`
- `../roadmap/ontology-implementation-checklist.md`
- `program-map.md`

분류 참고:
- 이 문서는 primary semantic contract가 아니라, canonical 문서를 읽는 순서와 self-check 질문을 제공하는 active reference guide다.
