# Supervisor Structure

## 목적
이 문서는 AMOS Phase 2에서 supervisor를 단순 다음 실행 주체 선택기가 아니라, **구조적 판단 계층**으로 확장할 때 domain 수준에서 무엇을 고정해야 하는지 정리한다.

중요:
- 이 문서는 **Phase 2 full expansion**을 설명한다.
- 다만 Phase 1에서도 구현 저장소가 self-check 할 수 있도록, 일부 판단 축을 reference contract로 선참고한다.
- Phase 1에서 지금 당장 맞춰야 하는 범위와 우선순위는 `../roadmap/ontology-implementation-checklist.md`를 우선 기준으로 본다.
- 이 문서는 구현 저장소의 구체 클래스/필드/schema를 canonical로 고정하지 않는다.
- 이 문서의 역할은 supervisor가 어떤 판단 축을 가져야 하는지, 그 판단이 `Request` / `Task` / `Policy` / `ClarifyRequest` / `ExecutionTrace`와 어떻게 연결되는지 정의하는 것이다.
- 구체 구현은 각 저장소가 자신의 state / event / API / trace 구조에서 관리하며, ownership boundary는 `../ontology/source-of-truth.md`를 따른다.

## 위치
Supervisor는 AMOS에서 다음 위치를 차지한다.
- 입력: `Request`
- 내부 판단 대상: `Task`, target entity, missing information, policy constraints
- 출력: 다음 실행 방향 또는 `ClarifyRequest` / `blocked` / `failed`
- 근거 기록: `ExecutionTrace`

즉 supervisor는 단순 라우터가 아니라, 요청을 **실행 가능한 구조**로 바꾸는 판단 계층이다.

## domain이 고정하는 최소 판단 축
Phase 2에서 supervisor는 최소한 아래 축을 구조적으로 다뤄야 한다.

### 1. `intent`
요청이 무엇을 하려는지에 대한 1차 분류.

예:
- `query_status`
- `query_location`
- `query_history`
- `query_metric`
- `query_system_status`
- `execute_action`
- `clarify_response`

의미:
- 어떤 worker / tool / retrieval 경로가 우선인지 결정한다.
- clarify가 필요한지, policy 확인이 먼저인지 판단하는 출발점이 된다.

### 2. `entity_candidates`
요청 안에서 추출되었거나 추정된 대상 엔티티 후보.

예:
- `Carrier`
- `Lot`
- `Equipment`
- `Fab`
- `System`
- `Request`

의미:
- supervisor는 하나의 정답 엔티티만 보는 것이 아니라, **후보와 불확실성**을 함께 다룬다.
- 후보가 여러 개면 clarify 또는 disambiguation 근거가 필요하다.
- retrieval / worker routing / trace 설명의 핵심 입력이 된다.

### 3. `missing_slots`
실행 전에 더 필요한 정보가 무엇인지 표현하는 구조.

대표 예:
- carrier id
- lot id
- equipment id / name
- equipment type
- metric time range
- log keyword / level / time window
- action target
- approval / confirmation context

의미:
- `missing_slots`는 clarify 발생 이유를 구조적으로 설명한다.
- clarify는 단순 질문 문자열이 아니라, 어떤 slot이 비어 있어서 멈췄는지와 연결돼야 한다.

### 4. `risk_level`
실행 또는 판단의 위험도를 표현하는 축.

대표 예:
- `low`: 단순 조회/설명
- `medium`: 해석 오류 시 운영 혼선 가능
- `high`: 실제 action / 승인 필요 / 안전 영향 가능

의미:
- `risk_level`은 approval / confirm / blocked 판단과 연결된다.
- 같은 intent라도 action 대상, 범위, 외부 영향에 따라 달라질 수 있다.

참고:
- exact enum은 구현 저장소가 정할 수 있다.
- 하지만 domain 관점에서는 “위험도 축이 있다”는 사실이 중요하다.

### 5. `policy_precheck`
실행 전에 선행 확인해야 하는 정책/권한/승인 조건.

예:
- approval 필요 여부
- action 허용 여부
- 안전 제약 위반 가능성
- 사용자 확인 필요 여부
- permission 부족 여부

의미:
- `policy_precheck`는 `Policy` 개념이 supervisor 판단에 실제로 연결되는 첫 관문이다.
- 정보는 충분하지만 정책 때문에 못 가는 경우를 `clarifying`가 아니라 `blocked` 쪽으로 분리하는 근거가 된다.

### 6. `clarify_question`
사용자에게 실제로 제시할 추가 질문.

예:
- "어느 장비를 말씀하시나요?"
- "조회 범위를 최근 1시간으로 볼까요?"
- "carrier id를 알려주세요."

의미:
- 사용자 인터페이스에 직접 노출되는 문장이다.
- 하지만 domain 관점에서 이 문장은 `missing_slots` / `entity_candidates` / `policy_precheck`와 분리되지 않는다.

### 7. `clarify_options`
질문에 대해 선택형으로 제시할 수 있는 후보.

예:
- 후보 장비 목록
- 최근 시간 범위 presets
- action 전 confirm choices

의미:
- clarify를 자유문 답변만이 아니라 **구조화된 선택**으로 다루게 한다.
- 여러 상호작용 표면 / 실행 계층 / trace에서 재사용 가능한 입력 표면이 된다.

## 최소 supervisor semantic checklist
Phase 2에서 supervisor는 로컬 구현 이름과 무관하게 최소한 아래 의미를 재구성할 수 있어야 한다.

### 필수 의미 축
- `intent`
  - 이 요청을 어떤 작업 유형으로 해석했는지
- `entity_candidates`
  - 어떤 대상 후보를 보고 있는지
- `missing_slots`
  - 아직 비어 있어서 실행을 멈추는 정보가 무엇인지
- `risk_level`
  - 위험도/운영 영향도를 어떻게 평가했는지
- `policy_precheck`
  - 승인/권한/안전 제약을 사전에 어떻게 판단했는지
- `decision`
  - 최종적으로 `execute / clarify / blocked / failed` 중 어디로 분기했는지
- `next_task` 또는 동등 의미 구조
  - 실행으로 갈 때 어떤 task/worker/tool 방향으로 넘길지
- `clarify_request` 또는 동등 의미 구조
  - clarify로 갈 때 어떤 질문/옵션을 사용자에게 제시할지
- `trace_reason` 또는 동등 의미 구조
  - 왜 이 분기를 선택했는지 요약 근거

### 선택 가능 필드
- `confidence`
  - intent 또는 entity 해석 자신도
- `route_candidates`
  - worker/tool 후보 목록
- `policy_actions`
  - confirm 필요, approval 필요 등 후속 정책 액션
- `notes`
  - 구현 저장소가 별도로 남기고 싶은 내부 메모

중요:
- domain은 위 필드명 자체를 강제하지 않는다.
- 아래 항목은 단일 JSON/object/schema를 강제하는 계약이 아니라 semantic checklist다.
- 구현 저장소는 위 의미를 로컬 output shape 어디엔가 담거나, 여러 event/log/API에 분산해 재구성 가능하게 만들면 된다.
- supervisor 출력은 단순 다음 실행 주체 문자열이 아니라, 이후 trace/replay/explainability에서 재구성 가능한 판단 결과여야 한다.

## 최소 판단 흐름
Phase 2에서 supervisor는 아래 순서를 따르는 것이 바람직하다.

1. `Request` 수용
2. `intent` 해석
3. `entity_candidates` 추출
4. `missing_slots` 확인
5. `risk_level` 평가
6. `policy_precheck` 수행
7. 아래 중 하나로 분기
   - 실행 가능한 `Task` 생성 및 worker routing
   - `ClarifyRequest` 생성
   - `blocked` 판단
   - `failed` 판단
8. 위 분기 결과를 supervisor semantic checklist에 맞춰 기록
9. 핵심 근거를 `ExecutionTrace`에 남김

## 상태와의 연결
supervisor 판단은 최소 runtime state와 직접 연결된다.

- `pending`
  - 요청은 수용됐지만 아직 판단/실행 전
  - supervisor output이 아직 확정되지 않았거나 초안 단계다.
- `clarifying`
  - `missing_slots` 또는 ambiguity 때문에 `ClarifyRequest`를 생성한 상태
  - 이때 output에는 `decision=clarify`에 해당하는 의미와 `clarify_request`가 있어야 한다.
- `executing`
  - 다음 `Task`가 결정되어 worker / tool 호출이 진행되는 상태
  - 이때 output에는 `decision=execute`에 해당하는 의미와 `next_task`가 있어야 한다.
- `blocked`
  - `policy_precheck` 또는 권한/승인/안전 제약 때문에 진행할 수 없는 상태
  - 이때 output에는 왜 차단되었는지와 어떤 제약이 원인인지가 남아야 한다.
- `failed`
  - 판단 또는 실행 과정에서 복구 불가 오류가 발생한 상태
  - 이때 output에는 시스템/의존성/해석 오류 등 실패 원인 분류가 최소한 요약돼야 한다.
- `completed`
  - 필요한 판단과 실행이 도메인 관점에서 정상 종료된 상태
  - completed는 supervisor 단독 상태라기보다, supervisor 판단 이후 흐름까지 끝난 결과 상태다.

## 최소 상태 전이 기준
supervisor는 적어도 아래 전이 기준을 설명 가능해야 한다.

- `pending → clarifying`
  - 필수 slot이 비어 있거나 entity ambiguity가 남아 있다.
- `pending → blocked`
  - 정보는 충분하지만 정책/권한/안전 제약상 진행 불가다.
- `pending → executing`
  - intent, entity, policy 조건이 모두 충족되어 다음 task를 만들 수 있다.
- `pending → failed`
  - 초기 해석 단계 자체가 오류로 깨졌다.
- `clarifying → executing`
  - 사용자의 추가 응답으로 `missing_slots`가 해소되었다.
- `clarifying → blocked`
  - 추가 응답 후에도 정책/권한 제약으로 진행 불가 판정이 났다.
- `executing → completed`
  - 실행과 응답이 정상 종료되었다.
- `executing → failed`
  - tool/dependency/route 오류로 흐름이 깨졌다.
- `executing → clarifying`
  - 실행 도중 새 missing slot 또는 ambiguity가 발견되었다.

중요:
- exact state machine은 구현 저장소가 더 세분화할 수 있다.
- 하지만 위 전이 이유는 domain 차원에서 explainability 가능한 수준으로 남아야 한다.

## clarify / blocked / failed 구분
### clarify
다음이 맞으면 `clarifying`이 우선이다.
- 필요한 정보가 빠져 있다.
- 대상이 여러 후보로 모호하다.
- 사용자의 추가 응답이 오면 같은 요청 흐름을 계속 진행할 수 있다.

### blocked
다음이 맞으면 `blocked`가 우선이다.
- 정책/권한/승인/안전 제약이 충족되지 않았다.
- 추가 정보만으로는 바로 해결되지 않는다.
- 실행을 진행하면 안 되는 이유가 명확하다.

### failed
다음이 맞으면 `failed`로 본다.
- 해석 또는 실행이 오류로 깨졌다.
- tool/dependency failure가 발생했다.
- 현재 흐름을 정상적으로 계속 잇기 어렵다.

## trace와의 연결
supervisor는 최소한 아래 판단 근거를 `ExecutionTrace`로 남길 수 있어야 한다.

- 어떤 `intent`로 해석했는가
- 어떤 `entity_candidates`를 봤는가
- 어떤 `missing_slots` 때문에 멈췄는가
- 어떤 `risk_level`로 평가했는가
- 어떤 `policy_precheck` 결과가 나왔는가
- 왜 `executing` / `clarifying` / `blocked` / `failed`로 갔는가

중요:
- trace는 디버그 로그가 아니라 운영 증거다.
- 나중에 replay / explainability / KPI / failure analysis에 재사용 가능해야 한다.

## 구현 저장소에 대한 기대
각 구현 저장소는 concept-to-code mapping 또는 로컬 설계 문서에서 최소한 아래를 명시한다.

1. 위 판단 축이 로컬에서 어떤 이름으로 표현되는지
2. supervisor가 어떤 출력 shape로 다음 단계를 넘기는지
3. `clarifying` / `blocked` / `failed` 구분을 어디서 어떻게 표현하는지
4. 어떤 판단 근거가 trace / event / request log에 남는지

## 비목표
이 문서는 아래를 직접 고정하지 않는다.
- 특정 필드 타입
- 특정 enum 값 집합
- 특정 event payload schema
- 특정 파일 경로/클래스명
- 특정 저장소의 supervisor implementation

이 문서의 목적은 구현 강제가 아니라, **AMOS에서 supervisor가 무엇을 판단해야 하는지의 공통 의미 구조**를 고정하는 것이다.
