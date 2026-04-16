# AMOS Domain Foundation

AMOS의 반도체/AMHS ontology, request/runtime 의미 체계, scenario, program boundary, roadmap를 한곳에서 관리하는 domain source-of-truth 저장소다.

## 이 저장소가 소유하는 것
- 반도체/AMOS 공통 개념과 관계
- request 해석 축과 request-side runtime semantics
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
3. [docs/ontology/foundation.md](docs/ontology/foundation.md)
4. [docs/ontology/request-interpretation.md](docs/ontology/request-interpretation.md)

#### Step B. ontology를 사용하는 active 문서를 본다
5. [docs/engine/request-lifecycle.md](docs/engine/request-lifecycle.md)
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
### A. Canonical ontology docs
- [docs/ontology/semiconductor-amhs.md](docs/ontology/semiconductor-amhs.md) — 반도체/AMHS 상위 개념
- [docs/ontology/foundation.md](docs/ontology/foundation.md) — 현재 foundation 기준의 도메인 객체/상태 축
- [docs/ontology/request-interpretation.md](docs/ontology/request-interpretation.md) — 자연어 요청의 intent / target / scope / missing-slot 해석 축

### B. Canonical engine / runtime docs
- [docs/engine/request-lifecycle.md](docs/engine/request-lifecycle.md) — request-side runtime vocabulary와 lifecycle 상태 canonical
- [docs/engine/practical-principles.md](docs/engine/practical-principles.md) — runtime / trace / policy 실전 원칙
- [docs/phase/foundation-engine.md](docs/phase/foundation-engine.md) — foundation 엔진 구조

### C. Scenarios / programs / roadmap docs
- [docs/phase/foundation-scenarios.md](docs/phase/foundation-scenarios.md) — scenario skeleton과 replay/synthetic 방향
- [docs/programs/program-map.md](docs/programs/program-map.md) — domain과 구현 역할 층위 지도
- [docs/roadmap/implementation-roadmap.md](docs/roadmap/implementation-roadmap.md) — 단계별 구현 로드맵
- [docs/roadmap/ontology-implementation-checklist.md](docs/roadmap/ontology-implementation-checklist.md) — domain과 구현 정렬을 위한 active checklist

### D. Canonical alignment / semantic contract docs
- [docs/engine/supervisor-structure.md](docs/engine/supervisor-structure.md) — supervisor 구조 확장 기준
- [docs/engine/execution-trace-structure.md](docs/engine/execution-trace-structure.md) — execution trace 구조화 기준
- [docs/engine/policy-permission-structure.md](docs/engine/policy-permission-structure.md) — policy / permission 구조화 기준
- [docs/programs/implementation-repo-reference-guide.md](docs/programs/implementation-repo-reference-guide.md) — 구현 저장소용 reference pack / self-check guide

### E. Pointer docs
- [docs/engine/runtime-observability.md](docs/engine/runtime-observability.md) → 핵심 내용은 [docs/phase/foundation-engine.md](docs/phase/foundation-engine.md)
- [docs/programs/project-classification.md](docs/programs/project-classification.md) → 핵심 내용은 [docs/programs/program-map.md](docs/programs/program-map.md)

### F. Governance / transitional / history docs
- [docs/archive/today-discussion-summary.md](docs/archive/today-discussion-summary.md) — 대화 기반 과도기 요약 *(archive/history)*
- [AGENTS.md](AGENTS.md) — 이 저장소에서 작업할 때의 운영 메모

### G. Secondary strategy docs
- [docs/engine/structural-intelligence-strategy.md](docs/engine/structural-intelligence-strategy.md) — GraphRAG / ReBAC / meta-ontology / domain-first agent 방향

## 구현 저장소와의 연결
이 저장소는 구현 저장소의 code mapping을 직접 소유하지 않는다.
대신 아래처럼 연결된다.

### domain repo가 제공하는 기준
- 구현 저장소용 읽는 순서 / self-check 질문 → [docs/programs/implementation-repo-reference-guide.md](docs/programs/implementation-repo-reference-guide.md)
- 도메인 객체 / 상태 축 → [docs/ontology/foundation.md](docs/ontology/foundation.md)
- request 해석 축 → [docs/ontology/request-interpretation.md](docs/ontology/request-interpretation.md)
- request lifecycle 기준 → [docs/engine/request-lifecycle.md](docs/engine/request-lifecycle.md)
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
- `docs/engine/` — request lifecycle, runtime, trace, policy, supervisor, 전략 문서
- `docs/phase/` — foundation 단계의 engine/scenario active 문서
- `docs/programs/` — 구현 저장소와의 책임 연결 지도
- `docs/roadmap/` — 단계 roadmap, active checklist
- `docs/archive/` — history / transitional notes

## 문서 추가/수정 규칙
앞으로 새 문서를 추가할 때는 반드시 이 README도 함께 업데이트한다.
최소한 아래를 반영한다.

1. 새 문서가 어느 역할인지 분류
   - canonical ontology
   - canonical engine / runtime
   - scenarios / programs / roadmap
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
