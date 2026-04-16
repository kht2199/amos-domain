# Implementation Roadmap

## Foundation — Domain Foundation
목표: 실행 가능한 최소 ontology/engine/policy/observability 기준 확립
- ontology 기준 문서 확정
- minimal runtime vocabulary / 상태 canonical 확정
- 실전 원칙 확정
- engine 최소 개념 정리
- scenario skeleton 정리
- source-of-truth 규칙 정리
- domain 기준과 구현 매핑 ownership 분리

### Foundation primary active docs
- ontology 큰 그림: `../phase/foundation-ontology.md`
- runtime vocabulary / 상태 canonical: `../phase/foundation-ontology.md`
- 실전 원칙: `../engine/practical-principles.md`
- foundation 엔진 구조: `../phase/foundation-engine.md`
- active checklist: `ontology-implementation-checklist.md`

### Foundation에서 선참고하는 later expansion reference docs
- supervisor 구조 확장 기준: `../engine/supervisor-structure.md`
- execution trace 구조화 기준: `../engine/execution-trace-structure.md`
- policy / permission 구조화 기준: `../engine/policy-permission-structure.md`

위 3개 문서는 **후속 단계 확장 구조**를 설명하는 reference contract다.
foundation에서는 self-check와 개념 분리를 위해 선참고하되, 지금 당장 맞춰야 하는 우선순위는 `ontology-implementation-checklist.md`를 따른다.

### 현재 진행 순서
1. `foundation-ontology.md` 기준으로 용어와 상태 canonical 고정
2. `supervisor-structure.md` / `execution-trace-structure.md` / `policy-permission-structure.md`는 후속 확장 기준으로 유지하되, foundation에서는 self-check reference로만 선참고
3. `ontology-implementation-checklist.md` 기준으로 구현 저장소가 참고해야 할 개념/판단 축/점검 기준을 정리
4. 각 구현 저장소가 자신의 concept-to-code mapping 문서와 로컬 코드에서 self-check 할 수 있도록 참고 문서 경로와 읽는 순서를 제공
5. foundation 범위 밖인 simulator / digital twin / predictor 세부 설계는 다음 단계로 넘김

### Foundation exit criteria
- `Request / Task / ToolCall / Policy / ClarifyRequest / ExecutionTrace` 의미가 domain에서 canonical로 고정돼 있다.
- canonical 상태 `pending / clarifying / executing / completed / blocked / failed` 가 active 기준으로 정리돼 있다.
- supervisor / trace / policy 판단 축이 각각 독립 문서로 분리돼 있다.
- domain은 의미와 경계를 소유하고, 저장소별 code mapping은 구현 저장소가 소유한다는 ownership 경계가 문서상 명확하다.
- 다음 단계 구현 점검은 `ontology-implementation-checklist.md` 기준으로 진행할 수 있다.

### Foundation 문서 재검토 결과
- foundation 문서군의 역할은 현재 기준으로 분리돼 있다.
  - 큰 그림 / 의미 체계: `../phase/foundation-ontology.md`
  - runtime vocabulary / 상태 canonical: `../phase/foundation-ontology.md`
  - 엔진 의미 축과 observability: `../phase/foundation-engine.md`
  - self-check 기준: `ontology-implementation-checklist.md`
  - 구현 저장소 읽는 순서: `../programs/implementation-repo-reference-guide.md`
- `../phase/foundation-scenarios.md` 는 full simulator spec이 아니라 **scenario skeleton** 문서로 유지한다.
- 우선순위/남은 작업은 이 roadmap에 모으고, checklist 문서는 semantic alignment 축과 완료 기준만 유지한다.

### Foundation 남은 작업
1. 각 구현 저장소에 `concept-code-mapping.md` 또는 동등 문서를 두고 `foundation-ontology.md` 기준 대응표를 만든다.
2. 각 구현 저장소에서 `pending / clarifying / executing / completed / blocked / failed` 상태가 실제 코드/API/event 어디에 대응되는지 정리한다.
3. `supervisor-structure.md`, `execution-trace-structure.md`, `policy-permission-structure.md` 기준으로 현재 구현이 어떤 축까지 반영했는지 gap을 기록한다.
4. `../phase/foundation-scenarios.md` 에서 최소 request/clarify/failure/observability 시나리오별 예시 request/trace 샘플을 연결한다.
5. pointer 문서(`runtime-observability.md`, `project-classification.md`)를 계속 유지할지, 구현 저장소 정렬이 안정화된 뒤 archive로 내릴지 재판단한다.

### 구현 저장소별 관련 메모
> 아래 항목은 canonical contract가 아니라, 각 저장소가 roadmap를 읽을 때 어디에 먼저 연결해서 보면 좋은지 정리한 **적용 메모**다.

#### agent
- 현재는 core라기보다 **orchestration / assistant surface** 관점에서 본다.
- 우선 연결할 것
  - `Request / Task / ClarifyRequest / ExecutionTrace` 의미 정렬
  - supervisor 판단 축과 request-level trace 복기 가능성 점검
  - `clarifying / blocked / failed` 구분이 stream/log/event에서 재구성 가능한지 확인
- 남길 산출물
  - concept-to-code mapping 또는 동등 문서
  - request/trace/event 기준 gap note

#### backend
- 현재는 **execution / integration surface** 관점에서 본다.
- 우선 연결할 것
  - action safety, permission, approval, blocked vs failed 구분
  - runtime state와 실제 API/DTO/service 경계 대응표
  - carrier / lot / equipment / metric / log 관련 domain 용어 정렬
- 남길 산출물
  - 상태/용어 대응표
  - policy / permission gap note

#### frontend
- 현재는 **operator / visualization surface** 관점에서 본다.
- 우선 연결할 것
  - clarify / confirm / trace / KPI / explainability를 어떤 화면과 상태에서 드러낼지 정리
  - `pending / clarifying / executing / completed / blocked / failed`가 UI에서 어떻게 읽히는지 점검
  - replay / event timeline / request detail에 필요한 최소 trace shape 확인
- 남길 산출물
  - 화면 상태 매핑 메모
  - trace / KPI / explainability 노출 gap note

#### glb-editor
- 현재는 **simulation / modeling surface** 관점에서 본다.
- 우선 연결할 것
  - ontology entity와 scene/object/state 표현의 연결 가능성 확인
  - simulator / digital twin 방향에서 필요한 topology/entity semantics 선별
  - foundation 범위를 넘는 상세 simulator contract는 보류하고 연결 포인트만 기록
- 남길 산출물
  - entity / object 표현 대응 메모
  - 후속 확장 후보 메모

#### infra
- 현재는 **runtime infrastructure surface** 관점에서 본다.
- 우선 연결할 것
  - runtime observability, 배포/운영, trace 수집 기반 정리
  - replay/KPI/diagnostics를 뒷받침할 로그·이벤트 적재 가능성 확인
- 남길 산출물
  - 운영/관측 연결 메모
  - 수집/보관 경계 note

## Replay / Scenario / Synthetic Data
- request/event/trace 기반 replay 설계
- scenario generator 초안
- synthetic data accumulation 규칙
- KPI 정의 구체화

## Full Simulator
- topology 모델
- OHT/bridge/lift/port/rail 세분화
- dispatch/routing policy
- queue/resource model

## Fast Predictor
- reduced model
- inference-friendly feature schema
- full simulator와의 calibration

## Digital Twin
- real data sync
- state reconciliation
- anomaly / drift detection
- operation feedback loop
