# AMOS Domain Foundation

AMOS의 반도체/AMHS ontology, runtime 의미 체계, scenario, program boundary, roadmap를 한곳에서 관리하는 domain source-of-truth 저장소다.

## 이 저장소가 소유하는 것
- 반도체/AMOS 공통 개념과 관계
- runtime state / trace / policy / supervisor 의미 구조
- 단계별 설계 기준과 roadmap
- 구현 저장소들이 맞춰야 할 semantic alignment 기준

## 이 저장소가 소유하지 않는 것
- 구현 저장소의 실제 클래스/필드/API/event/tool 경로
- repo-local concept-to-code mapping 표
- 개별 구현 저장소의 로컬 설계 세부사항

구현 저장소는 자신의 코드/테스트/로컬 문서를 소유하고,
이 저장소는 **무엇이 어떤 의미를 가지는지**를 소유한다.

## 빠른 시작 경로
### 1) ontology부터 먼저 검증하고 싶은 사람
다음 순서로 읽으면 **온톨로지 정의 → 이를 사용하는 문서** 순서로 검증할 수 있다.

#### Step A. ontology 자체를 먼저 본다
1. [docs/ontology/index.md](docs/ontology/index.md)
2. [docs/ontology/semiconductor-amhs.md](docs/ontology/semiconductor-amhs.md)
3. [docs/ontology/foundation-ontology.md](docs/ontology/foundation-ontology.md)

#### Step B. ontology를 사용하는 active 문서를 본다
6. [docs/engine/practical-principles.md](docs/engine/practical-principles.md)
7. [docs/phase/foundation-engine.md](docs/phase/foundation-engine.md)
8. [docs/phase/foundation-scenarios.md](docs/phase/foundation-scenarios.md)
9. [docs/programs/program-map.md](docs/programs/program-map.md)
10. [docs/roadmap/implementation-roadmap.md](docs/roadmap/implementation-roadmap.md)

#### Step C. 구현 저장소 정렬이나 self-check가 필요할 때만 본다
11. [docs/programs/implementation-repo-reference-guide.md](docs/programs/implementation-repo-reference-guide.md)
12. [docs/roadmap/ontology-implementation-checklist.md](docs/roadmap/ontology-implementation-checklist.md)
13. [docs/engine/supervisor-structure.md](docs/engine/supervisor-structure.md) *(later expansion reference)*
14. [docs/engine/execution-trace-structure.md](docs/engine/execution-trace-structure.md) *(later expansion reference)*
15. [docs/engine/policy-permission-structure.md](docs/engine/policy-permission-structure.md) *(later expansion reference)*
16. 각 구현 저장소의 로컬 `AGENTS.md` / `README.md` / mapping 문서

### 2) 문서 정리/거버넌스 검토가 필요한 사람
1. [AGENTS.md](AGENTS.md)


## 문서 지도: 역할별 분류

### A. Canonical domain docs
이 저장소의 핵심 active 기준 문서다.

#### Ontology
- [docs/ontology/semiconductor-amhs.md](docs/ontology/semiconductor-amhs.md) — 반도체/AMHS 상위 개념
- [docs/ontology/foundation-ontology.md](docs/ontology/foundation-ontology.md) — 현재 foundation ontology 기준 + runtime vocabulary / 상태 canonical

#### Engine / runtime principles
- [docs/engine/practical-principles.md](docs/engine/practical-principles.md) — runtime / trace / policy 실전 원칙
- [docs/phase/foundation-engine.md](docs/phase/foundation-engine.md) — foundation 엔진 구조

#### Scenarios / programs / roadmap
- [docs/phase/foundation-scenarios.md](docs/phase/foundation-scenarios.md) — scenario skeleton과 replay/synthetic 방향
- [docs/programs/program-map.md](docs/programs/program-map.md) — domain과 구현 역할 층위 지도
- [docs/roadmap/implementation-roadmap.md](docs/roadmap/implementation-roadmap.md) — 단계별 구현 로드맵

### B. Canonical alignment / semantic contract docs
구현 저장소들이 맞춰야 하는 semantic alignment 기준이다.
이 문서들은 구현 세부 schema를 고정하지 않지만, **무엇을 맞춰야 하는지**를 고정한다.

- [docs/engine/supervisor-structure.md](docs/engine/supervisor-structure.md) — supervisor 구조 확장 기준
- [docs/engine/execution-trace-structure.md](docs/engine/execution-trace-structure.md) — execution trace 구조화 기준
- [docs/engine/policy-permission-structure.md](docs/engine/policy-permission-structure.md) — policy / permission 구조화 기준

### B-1. Implementation reference guide
구현 저장소가 self-check 할 때 참고할 읽는 순서와 질문을 제공하는 안내 문서다.

- [docs/programs/implementation-repo-reference-guide.md](docs/programs/implementation-repo-reference-guide.md) — 구현 저장소용 reference pack / self-check 가이드

### C. Pointer docs
중복 canonical을 만들지 않기 위해 유지하는 포인터 문서다.
새로운 active 내용을 여기서 계속 확장하지 않는다.

- [docs/engine/runtime-observability.md](docs/engine/runtime-observability.md) → 핵심 내용은 [docs/phase/foundation-engine.md](docs/phase/foundation-engine.md)
- [docs/programs/project-classification.md](docs/programs/project-classification.md) → 핵심 내용은 [docs/programs/program-map.md](docs/programs/program-map.md)

### D. Governance / transitional / history docs
작업 규칙, archive 문서, 운영 메모처럼 primary domain canon 밖의 메타 정보를 담는다.

- [docs/archive/today-discussion-summary.md](docs/archive/today-discussion-summary.md) — 대화 기반 과도기 요약 *(archive/history)*
- [AGENTS.md](AGENTS.md) — 이 저장소에서 작업할 때의 운영 메모

### E. Secondary strategy docs
핵심 foundation canon은 아니지만, 장기 방향을 이해하는 데 유효한 문서다.

- [docs/engine/structural-intelligence-strategy.md](docs/engine/structural-intelligence-strategy.md) — GraphRAG / ReBAC / meta-ontology / domain-first agent 방향

### E-1. Active checklist
현재 foundation에서 지금 당장 맞춰야 하는 self-check 우선순위를 정리한 문서다.

- [docs/roadmap/ontology-implementation-checklist.md](docs/roadmap/ontology-implementation-checklist.md) — domain과 구현 정렬을 위한 active checklist

## 구현 저장소와의 연결
이 저장소는 구현 저장소의 code mapping을 직접 소유하지 않는다.
대신 아래처럼 연결된다.

### domain repo가 제공하는 기준
- 구현 저장소용 읽는 순서 / self-check 질문 → [docs/programs/implementation-repo-reference-guide.md](docs/programs/implementation-repo-reference-guide.md)
- runtime vocabulary / 상태 → [docs/ontology/foundation-ontology.md](docs/ontology/foundation-ontology.md)
- supervisor 구조 → [docs/engine/supervisor-structure.md](docs/engine/supervisor-structure.md)
- trace semantics → [docs/engine/execution-trace-structure.md](docs/engine/execution-trace-structure.md)
- policy / permission semantics → [docs/engine/policy-permission-structure.md](docs/engine/policy-permission-structure.md)
- ownership 경계 → README의 "이 저장소가 소유하는 것 / 소유하지 않는 것" 섹션

### 구현 저장소가 소유하는 것
- 각 저장소의 로컬 `AGENTS.md`
- 각 저장소의 로컬 `README.md`
- 각 저장소의 concept-to-code mapping 또는 동등 문서
- 각 저장소의 실제 코드 / 테스트 / schema / event / API / state 정의

원칙:
- domain은 의미를 정의한다.
- 구현 저장소는 그 의미가 어디에 구현되는지 정의한다.
- 같은 내용을 양쪽에 복제해 parallel canon을 만들지 않는다.

## source-of-truth 원칙
- 이 저장소의 active 문서가 ontology / engine / scenario / roadmap의 기준이다.
- 구현/매핑 기준은 각 구현 저장소의 실제 코드와 테스트가 우선이다.
- archive 문서는 역사/참고용이며 active 판단 근거로 직접 쓰지 않는다.


## 디렉토리 구조
- `docs/ontology/` — ontology 기준 문서와 반도체/AMHS 상위 개념
- `docs/engine/` — runtime / trace / policy / supervisor / 전략 문서
- `docs/scenarios/` — replay/synthetic/오류 주입 전 단계 시나리오
- `docs/programs/` — 구현 저장소와의 책임 연결 지도
- `docs/roadmap/` — 단계 roadmap, active checklist
- `docs/archive/` — history / transitional notes

## 문서 추가/수정 규칙
앞으로 새 문서를 추가할 때는 반드시 이 README도 함께 업데이트한다.
최소한 아래를 반영한다.

1. 새 문서가 어느 역할인지 분류
   - canonical domain
   - canonical alignment / semantic contract
   - pointer
   - governance / transitional / history
   - secondary strategy
2. 기존 문서를 대체하면 이전 문서를 pointer 또는 archive로 전환
3. 구현 저장소 mapping과 중복되는 내용이면 domain에 복제하지 말고 링크만 유지
4. 읽는 경로가 바뀌면 `빠른 시작 경로`와 `문서 지도`를 함께 수정

## 관련 문서
- [AGENTS.md](AGENTS.md)
- [docs/README.md](docs/README.md)
- [docs/ontology/index.md](docs/ontology/index.md)
