# Program Map

## 이 폴더의 역할
도메인 문서와 실제 프로그램/서비스가 어떻게 대응되는지 정리한다.

## 먼저 보는 기준 문서
- `project-classification.md` — 저장소를 core / adapter / archive로 재분류한 기준표

## 현재 프로젝트 분류 요약

### 1. Core
#### AMOS Domain Foundation
경로: `/Users/htkim/IdeaProjects/amos/domain`
- ontology, engine, scenarios, roadmap의 active source of truth
- simulator → digital twin으로 가는 기준 저장소
- 다른 구현 저장소가 이 문서를 참조해 의미 체계를 맞춘다

### 2. Adapter
#### Backend
경로: `/Users/htkim/IdeaProjects/amos/backend`
- 실제 설비/운영 API와의 연결 지점
- carrier / lot / equipment / log / metric / status / action controller가 존재
- future digital twin에서 현실 데이터 sync 경계가 될 수 있음

#### Frontend
경로: `/Users/htkim/IdeaProjects/amos/frontend`
- React/Vite 기반 UI
- trace / replay / KPI / explainability 화면을 담을 수 있는 후보
- 현재는 운영 UI 성격이 강한 adapter

#### Infra
경로: `/Users/htkim/IdeaProjects/amos/infra`
- 배포, 서비스 운영, 모니터링 기반
- simulator core는 아니지만 runtime 운영층으로 필요

### 3. Adapter + Legacy 성격
#### AMOS Agent
경로: `/Users/htkim/IdeaProjects/amos/agent`
- 자연어 질의, supervisor, SSE, tool routing, request/trace 구조는 유효
- 하지만 simulator / digital twin core 저장소로 보기는 어렵다
- ontology 기준 저장소가 아니라 운영 질의/제어용 orchestration 계층에 가깝다
- 따라서 앞으로는 core가 아니라 explainability / operator assistant / trace facade 쪽 자산으로 재해석하는 것이 맞다

### 4. Archive
#### Migration backups
경로: `/Users/htkim/IdeaProjects/amos/.migration_backups/*`
- active 개발 기준이 아닌 역사 보존용 산출물

## 도메인 기준 연결 방식
- `domain`이 용어/상태/이벤트/정책의 기준을 잡는다.
- `backend`는 현실 시스템 접점으로 연결된다.
- `frontend`는 사람이 보는 조작/관측 화면을 제공한다.
- `infra`는 운영 환경을 제공한다.
- `agent`는 legacy 자산을 살려 설명/질의/trace 관문으로만 재배치한다.

## 원칙
- domain 프로젝트는 구현 저장소를 대체하지 않는다.
- 대신 여러 저장소가 공유해야 하는 의미 체계와 phase 계획을 관리한다.
- 구현 저장소의 우선순위는 core 적합도가 아니라 **연결 책임** 기준으로 다시 본다.
- 따라서 `agent`는 중요할 수는 있어도 중심은 아니다.
