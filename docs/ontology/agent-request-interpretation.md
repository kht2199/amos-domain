# Agent Request Interpretation Ontology

## 목적
이 문서는 자연어 사용자 요청을 agent/orchestration 관점에서 어떤 의미 축으로 해석할지 정리한 active ontology 문서다.

중요:
- 이 문서는 반도체/AMHS 객체 ontology 자체보다 **agent가 요청을 어떻게 해석하는가**에 더 가깝다.
- 따라서 `Request`, `ClarifyRequest`, request lifecycle 상태, supervisor 판단 축과 직접 연결된다.
- 반도체/AMHS 객체 ontology와 도메인 상태 축은 `foundation-ontology.md`가 계속 소유한다.

## 위치
- 도메인 객체 ontology와 도메인 상태 축의 기반: `foundation-ontology.md`
- 자연어 요청 해석, clarify 분기, request lifecycle 기준: 이 문서
- supervisor 구조 확장 기준: `../engine/supervisor-structure.md`
- execution trace 구조화 기준: `../engine/execution-trace-structure.md`
- policy / permission 구조 기준: `../engine/policy-permission-structure.md`

## 사용자 질의 해석 기준
현재 foundation에서는 자연어 요청을 아래 ontology 축으로 해석한다.

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

## Request-side runtime vocabulary and state canonical
### 최소 용어
- `Request` — 사용자 질의 또는 실행 요청을 표현하는 최상위 작업 단위
- `ClarifyRequest` — 정보 부족/모호성 때문에 추가 입력을 요청하는 구조

참고:
- `Task`, `ToolCall`, `ExecutionTrace`, `Policy`는 agent 실행 구조와 더 직접 연결되므로 engine 문서를 우선 따른다.

### 최소 상태 집합
- `pending`
- `clarifying`
- `executing`
- `completed`
- `blocked`
- `failed`

### 상태 의미
- `pending` — 요청이 수용되었지만 본격 실행 전인 상태
- `clarifying` — 추가 정보가 필요해 응답을 기다리는 상태
- `executing` — 내부 판단 또는 실제 실행이 진행 중인 상태
- `completed` — 요청 흐름이 정상 종료된 상태
- `blocked` — 정책/권한/외부 조건 때문에 진행 불가한 상태
- `failed` — 실행을 시도했지만 오류로 정상 종료하지 못한 상태

### 구분 원칙
- `clarifying`은 사용자 추가 정보가 오면 같은 요청 흐름을 이어갈 수 있다.
- `blocked`는 정보 부족이 아니라 정책/권한/운영 조건에 막힌 상태다.
- `failed`는 실행 시도 결과의 실패다.

## 경계
- 이 문서는 자연어 요청을 어떤 intent/entity/scope/missing-slot 축으로 해석할지 정의한다.
- 이 문서는 특정 저장소의 classifier, parser, prompt, router 구현을 고정하지 않는다.
- 반도체/AMHS 객체 자체의 의미와 관계는 `foundation-ontology.md`와 `semiconductor-amhs.md`가 소유한다.
- supervisor / trace / policy의 확장 구조는 engine 문서가 소유한다.
