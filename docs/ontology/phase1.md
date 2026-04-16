# AMOS Ontology Phase 1

> 이 문서는 이제 AMOS 반도체 ontology의 active 기준 문서다.
> 기존 구현 저장소의 동명/유사 문서는 중복 방지를 위해 이 문서를 가리키는 포인터로만 유지한다.

## 목적
이 문서는 AMOS의 반도체/AMHS 도메인 온톨로지를 **Phase 1에서 먼저 고정해야 할 개념/관계 중심**으로 정리한 active 문서다.

중요:
- 이 문서는 `docs/archive/` 아래의 과거 draft를 대체하는 현재 작업 기준이다.
- 이 문서는 ontology의 **의미와 경계**를 우선 정의한다.
- runtime 상태 canonical과 구현 적용 절차는 이 문서 밖의 하위 문서가 소유한다.
- Phase 1에서는 거대한 지식그래프보다 **운영에 바로 쓰이는 공통 의미 체계**를 우선한다.

---

## Phase 1에서 다루는 ontology 범위

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
- canonical 용어 정의는 `minimal-runtime-model.md`를 따른다.
- 구현별 로컬 이름, 클래스, event shape는 이 문서의 범위가 아니다.

Phase 1에서는 장비를 세분화하되, 코드 영향은 최소화한다.
- `EquipmentType.AGV`
- `EquipmentType.STK`
- `EquipmentType.CNV`
- `EquipmentType.LFT`

표현은 enum으로 고정할 수도 있고 문자열로 유지할 수도 있으나, 의미는 위 4개 축으로 통일한다.

### 2) Runtime State Layer
현재 ontology가 구분해야 할 상태 축이 무엇인지 정의한다.

Phase 1에서는 다음 상태 축을 구분한다.
- Carrier 위치/상태
- Lot 위치/상태
- Equipment 상태
- Request / Task / execution lifecycle 상태

참고:
- Request / Task / execution lifecycle의 canonical 용어와 상태 집합은 `minimal-runtime-model.md`에서 고정한다.
- 이 문서는 상태 집합의 존재와 구분 필요성만 설명한다.

### 3) Event Layer
상태 변화와 판단 결과가 어떤 종류의 사건으로 표현되는지 정의한다.

Phase 1에서 최소로 구분하는 사건 종류:
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
Phase 1에서는 자연어 요청을 아래 ontology 축으로 해석한다.

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
- runtime 상태 canonical은 `minimal-runtime-model.md`가 소유한다.
- 이 문서는 구현 저장소별 클래스/필드/API 이름이나 concept-to-code mapping을 소유하지 않는다.
- 상세 ownership / source-of-truth 규칙은 `source-of-truth.md`를 따른다.

## Phase 1 이후 확장 방향
다음은 다음 단계에서 ontology를 더 세분화할 때의 후보 축이다.
- OHT / bridge / port / rail / lift topology 세분화
- scenario / replay / synthetic data generation을 위한 추가 ontology
- fast predictor / full simulator 이원화에 필요한 상태/관계 축
- real data sync 기반 digital twin 연결 ontology
- ontology-driven routing hints
- trace taxonomy 고도화
