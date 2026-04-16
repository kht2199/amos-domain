# AMOS Domain Foundation

> 이 문서는 AMOS 반도체/AMHS ontology의 active 기준 문서다.
> 기존 구현 저장소의 동명/유사 문서는 중복 방지를 위해 이 문서를 가리키는 포인터로만 유지한다.

## 목적
이 문서는 AMOS의 반도체/AMHS 도메인 온톨로지를 **현재 foundation에서 먼저 고정해야 할 개념과 관계** 중심으로 정리한 active 문서다.

중요:
- 이 문서는 `docs/archive/` 아래 과거 draft를 대체하는 현재 작업 기준이다.
- 이 문서는 ontology의 **의미와 경계**를 우선 정의한다.
- 도메인 객체와 도메인 상태 축까지를 ontology boundary 안에서 함께 정리한다.
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

참고:
- `Request`, `Task`, `ToolCall`, `ClarifyRequest`, `ExecutionTrace`, `Policy` 같은 agent/orchestration 용어는 이 문서의 핵심 엔티티로 올리지 않는다.
- 구현별 로컬 이름, 클래스, event shape는 이 문서의 범위가 아니다.

현재 foundation에서는 장비를 세분화하되 코드 영향은 최소화한다.
- `EquipmentType.AGV`
- `EquipmentType.STK`
- `EquipmentType.CNV`
- `EquipmentType.LFT`

표현은 enum으로 고정할 수도 있고 문자열로 유지할 수도 있으나, 의미는 위 4개 축으로 통일한다.

### 2) Runtime State Layer
현재 ontology가 구분해야 할 도메인 상태 축이 무엇인지 정의한다.

현재 foundation에서는 다음 상태 축을 구분한다.
- Carrier 위치/점유/이송 상태
- Lot 위치/진행 상태
- Equipment 가용/사용/장애 상태
- Fab 레벨의 구역/가동 컨텍스트

참고:
- request lifecycle 상태 canonical은 `request-lifecycle.md`가 소유한다.

### 3) Event Layer
도메인 상태 변화가 어떤 종류의 사건으로 표현되는지 정의한다.

현재 foundation에서 최소로 구분하는 사건 종류:
- carrier handoff / move / dock
- lot 진행 또는 소속 변화
- equipment 상태 전이
- fab 구역/가용성 변화

참고:
- request 수용, tool 호출, clarify, policy 판단, execution trace 같은 agent 실행 사건은 engine 문서에서 다룬다.
- ontology는 어떤 도메인 사건 종류가 존재해야 하는지를 먼저 정의한다.

### 4) Policy Layer
무엇을 할 수 있는지보다 **도메인 차원에서 어떤 안전/운영 불변식이 필요한지**를 정의한다.

현재 정책 예시:
- 동일 `Carrier`가 상충하는 두 위치에 동시에 존재한다고 해석하지 않는다.
- `Equipment` 상태가 사용 불가라면 해당 설비를 정상 처리 경로로 간주하지 않는다.
- `Lot`의 진행 상태 변화는 임의 점프가 아니라 추적 가능한 전이로 남겨야 한다.
- 도메인 객체 관계를 해칠 정도의 모순 상태는 정상 상태로 승격하지 않는다.

참고:
- approval / permission / confirm / clarify 같은 agent 정책 결정은 engine 문서에서 다룬다.
- 이 레이어는 domain이 안전 원칙과 불변식을 정의하는 위치다.

### 5) Domain Boundary Note
이 foundation 문서는 반도체/AMHS 객체와 그 객체의 상태 축을 우선 소유한다.

별도 문서가 소유하는 것:
- 자연어 요청의 intent / target / scope / missing-slot 해석 축 → `request-interpretation.md`
- request-side runtime vocabulary와 lifecycle 상태 canonical → `../engine/request-lifecycle.md`
- supervisor / trace / policy / permission 구조 → `../engine/*.md`

즉 `Request`, `Task`, `ToolCall`, `ClarifyRequest`, `ExecutionTrace` 같은 agent 실행 용어를
AMHS 객체 ontology와 같은 층위의 핵심 엔티티로 섞지 않는다.

---

## 핵심 관계

### 도메인 관계
- `Carrier` is transported by `Equipment`
- `Carrier` may contain or represent work for `Lot`
- `Lot` is processed in `Fab`
- `Equipment` belongs to `Fab`

### 도메인 경계 관계
- `Carrier`, `Lot`, `Equipment`, `Fab` 관계가 foundation ontology의 직접 범위다.
- request 해석/실행 구조는 도메인 객체 ontology를 참조하지만, 같은 층위의 객체 관계로 병치하지 않는다.

---

## 연결 문서
- 자연어 요청 해석 축: `request-interpretation.md`
- request lifecycle 기준: `../engine/request-lifecycle.md`
- 상세 ownership 경계 설명: `../../README.md`

## 경계 원칙
- 이 문서는 ontology의 개념, 관계, 도메인 상태 축, 최소 경계를 정의한다.
- request lifecycle, clarify 구조, trace/policy 실행 용어는 별도 문서가 소유한다.
- 이 문서는 구현 저장소별 클래스/필드/API 이름이나 concept-to-code mapping을 소유하지 않는다.

## 이후 확장 방향
다음은 다음 단계에서 ontology를 더 세분화할 때의 후보 축이다.
- OHT / bridge / port / rail / lift topology 세분화
- scenario / replay / synthetic data generation을 위한 추가 ontology
- fast predictor / full simulator 이원화에 필요한 상태/관계 축
- real data sync 기반 digital twin 연결 ontology
- ontology를 사용하는 엔진/시뮬레이션 연결 규칙 정교화
