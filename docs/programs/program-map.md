# Program Map

## 이 폴더의 역할
도메인 문서와 실제 구현 층위가 어떻게 대응되는지 정리한다.

구현 저장소가 domain 기준을 참고할 때의 읽는 순서와 self-check 질문은
`implementation-repo-reference-guide.md`를 우선 참고한다.

중요:
- Phase 1에서 지금 당장 맞춰야 하는 우선순위는 `../roadmap/ontology-implementation-checklist.md`를 따른다.
- `supervisor-structure.md`, `execution-trace-structure.md`, `policy-permission-structure.md`는 구현 저장소가 선참고하는 Phase 2 expansion reference다.

## 분류 기준

### 1. core
- simulator / digital twin의 개념 중심을 직접 담는다.
- ontology / state / event / policy / scenario / replay / KPI / prediction 같은 핵심 축을 관리한다.
- 다른 저장소가 이 프로젝트를 기준으로 의미 체계를 맞춘다.

### 2. adapter
- core를 직접 구현하지는 않지만, core를 외부 시스템/사용자/운영 환경과 연결한다.
- UI, API facade, 기존 운영 시스템 연동, 배포/관측 같은 역할을 담당한다.

### 3. archive / legacy
- 현재 목표인 simulator / digital twin과 중심축이 어긋난다.
- 일부 자산은 참고할 가치가 있지만 더 이상 주 설계 기준은 아니다.
- 필요시 일부만 추출하고, 전체 프로젝트를 중심으로 계속 키우지는 않는다.

## 현재 역할 분류 요약

### 1. Core
#### Canonical domain foundation
- ontology, engine, scenarios, roadmap의 active source of truth
- simulator → digital twin으로 가는 기준 층위
- 다른 구현 층위가 이 문서를 참조해 의미 체계를 맞춘다

### 2. Adapter
#### Execution / integration surface
- 실제 설비/운영 API와의 연결 지점
- carrier / lot / equipment / log / metric / status / action controller 같은 현실 시스템 접점을 담당
- future digital twin에서 현실 데이터 sync 경계가 될 수 있음

#### Operator / visualization surface
- 사람이 보는 조작/관측 화면
- trace / replay / KPI / explainability 화면을 담는 표면
- 현재는 운영 UI 성격이 강한 adapter일 수 있음

#### Runtime infrastructure surface
- 배포, 서비스 운영, 모니터링 기반
- simulator core는 아니지만 runtime 운영층으로 필요

### 3. Adapter + Legacy 성격
#### Orchestration / assistant surface
- 자연어 질의, supervisor, streaming, tool routing, request/trace 구조 같은 자산을 포함할 수 있음
- simulator / digital twin core라기보다 운영 질의/제어용 orchestration 계층에 가깝다
- explainability / operator assistant / trace facade 쪽 자산으로 재해석될 수 있다

### 4. Archive
#### Historical backups
- active 개발 기준이 아닌 역사 보존용 산출물

## 도메인 기준 연결 방식
- canonical domain foundation이 용어/상태/이벤트/정책의 기준을 잡는다.
- execution / integration surface는 현실 시스템 접점으로 연결된다.
- operator / visualization surface는 사람이 보는 조작/관측 화면을 제공한다.
- runtime infrastructure surface는 운영 환경을 제공한다.
- orchestration / assistant surface는 설명/질의/trace 관문으로 재배치될 수 있다.

## 해석
### `canonical domain foundation`
- AMOS를 simulator / digital twin 방향으로 다시 세울 때의 기준 층위다.
- ontology, engine, scenarios, programs, roadmap의 active source of truth를 관리한다.

### `orchestration / assistant surface`
- 버릴 대상이 아니라 adapter + legacy 성격으로 재배치할 수 있는 층위다.
- ontology 기준 자체는 아니지만 trace / clarify / explainability / operator interaction 자산은 유효하다.

### `execution / integration surface`
- 현실 설비/운영 API 경계면으로서 가치가 있다.
- future digital twin에서는 실측 상태 유입, action 실행 경계, metric/log 동기화 지점이 될 가능성이 높다.

### `operator / visualization surface`
- 현재는 운영 UI 성격이 강하지만 replay viewer, event timeline, KPI dashboard, explainability console 후보가 될 수 있다.

### `runtime infrastructure surface`
- domain 설계의 중심은 아니지만 runtime 운영/배포/관측을 붙일 때 계속 필요한 기반이다.

## 원칙
- domain 프로젝트는 구현 저장소를 대체하지 않는다.
- 대신 여러 저장소가 공유해야 하는 의미 체계와 phase 계획을 관리한다.
- 구현 저장소의 우선순위는 core 적합도가 아니라 **연결 책임** 기준으로 다시 본다.
- 따라서 orchestration / assistant surface는 중요할 수는 있어도 중심은 아니다.
- domain은 공통 개념과 원칙을 정의하고, 각 구현 저장소는 그 개념을 자기 코드에 매핑한 문서를 자체적으로 유지한다.
- archive는 보존하되 active source of truth처럼 취급하지 않는다.
