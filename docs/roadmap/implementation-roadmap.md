# Implementation Roadmap

## Phase 1 — Domain Foundation
목표: 실행 가능한 최소 ontology/engine/policy/observability 기준 확립
- ontology 기준 문서 확정
- minimal runtime vocabulary / 상태 canonical 확정
- 실전 원칙 확정
- engine 최소 개념 정리
- scenario skeleton 정리
- source-of-truth 규칙 정리
- domain 기준과 구현 매핑 ownership 분리

### Phase 1 primary active docs
- ontology 큰 그림: `../ontology/phase1.md`
- 최소 runtime vocabulary / 상태 canonical: `../ontology/minimal-runtime-model.md`
- 실전 원칙: `../engine/practical-principles.md`
- Phase 1 엔진 구조: `../engine/phase1-engine.md`
- active checklist: `ontology-implementation-checklist.md`

### Phase 1에서 선참고하는 expansion reference docs
- supervisor 구조 확장 기준: `../engine/supervisor-structure.md`
- execution trace 구조화 기준: `../engine/execution-trace-structure.md`
- policy / permission 구조화 기준: `../engine/policy-permission-structure.md`

위 3개 문서는 **Phase 2 이후 full expansion**을 설명하는 reference contract다.
Phase 1에서는 self-check와 개념 분리를 위해 선참고하되, 지금 당장 맞춰야 하는 우선순위는 `ontology-implementation-checklist.md`를 따른다.

### 현재 진행 순서
1. `minimal-runtime-model.md` 기준으로 용어와 상태 canonical 고정
2. `supervisor-structure.md` / `execution-trace-structure.md` / `policy-permission-structure.md`는 Phase 2 이후 full expansion 기준으로 유지하되, Phase 1에서는 self-check reference로만 선참고
3. `ontology-implementation-checklist.md` 기준으로 구현 저장소가 참고해야 할 개념/판단 축/점검 기준을 정리
4. 각 구현 저장소가 자신의 concept-to-code mapping 문서와 로컬 코드에서 self-check 할 수 있도록 참고 문서 경로와 읽는 순서를 제공
5. Phase 1 범위 밖인 simulator / digital twin / predictor 세부 설계는 Phase 2 이후로 넘김

### Phase 1 exit criteria
- `Request / Task / ToolCall / Policy / ClarifyRequest / ExecutionTrace` 의미가 domain에서 canonical로 고정돼 있다.
- canonical 상태 `pending / clarifying / executing / completed / blocked / failed` 가 active 기준으로 정리돼 있다.
- supervisor / trace / policy 판단 축이 각각 독립 문서로 분리돼 있다.
- domain은 의미와 경계를 소유하고, 저장소별 code mapping은 구현 저장소가 소유한다는 ownership 경계가 문서상 명확하다.
- 다음 단계 구현 점검은 `ontology-implementation-checklist.md` 기준으로 진행할 수 있다.

### Phase 1 문서 재검토 결과
- Phase 1 문서군의 역할은 현재 기준으로 분리돼 있다.
  - 큰 그림 / 의미 체계: `../ontology/phase1.md`
  - 최소 runtime vocabulary / 상태 canonical: `../ontology/minimal-runtime-model.md`
  - 엔진 의미 축과 observability: `../engine/phase1-engine.md`
  - self-check 기준: `ontology-implementation-checklist.md`
  - 구현 저장소 읽는 순서: `../programs/implementation-repo-reference-guide.md`
- `phase1-scenarios.md` 는 full simulator spec이 아니라 **scenario skeleton** 문서로 유지한다.
- 우선순위/남은 작업은 이 roadmap에 모으고, checklist 문서는 semantic alignment 축과 완료 기준만 유지한다.

### Phase 1 남은 작업
1. 각 구현 저장소에 `concept-code-mapping.md` 또는 동등 문서를 두고 `minimal-runtime-model.md` 기준 대응표를 만든다.
2. 각 구현 저장소에서 `pending / clarifying / executing / completed / blocked / failed` 상태가 실제 코드/API/event 어디에 대응되는지 정리한다.
3. `supervisor-structure.md`, `execution-trace-structure.md`, `policy-permission-structure.md` 기준으로 현재 구현이 어떤 축까지 반영했는지 gap을 기록한다.
4. `phase1-scenarios.md` 에서 최소 request/clarify/failure/observability 시나리오별 예시 request/trace 샘플을 연결한다.
5. pointer 문서(`runtime-observability.md`, `project-classification.md`)를 계속 유지할지, 구현 저장소 정렬이 안정화된 뒤 archive로 내릴지 재판단한다.

## Phase 2 — Replay / Scenario / Synthetic Data
- request/event/trace 기반 replay 설계
- scenario generator 초안
- synthetic data accumulation 규칙
- KPI 정의 구체화

## Phase 3 — Full Simulator
- topology 모델
- OHT/bridge/lift/port/rail 세분화
- dispatch/routing policy
- queue/resource model

## Phase 4 — Fast Predictor
- reduced model
- inference-friendly feature schema
- full simulator와의 calibration

## Phase 5 — Digital Twin
- real data sync
- state reconciliation
- anomaly / drift detection
- operation feedback loop
