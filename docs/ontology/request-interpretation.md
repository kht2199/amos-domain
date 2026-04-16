# Request Interpretation

## 목적
이 문서는 자연어 사용자 요청을 어떤 의미 축으로 해석할지 정리한 active ontology 문서다.

중요:
- 이 문서는 반도체/AMHS 객체 ontology 자체보다 **요청을 어떤 축으로 읽을지**에 가깝다.
- 도메인 객체 ontology와 도메인 상태 축은 `foundation.md`가 계속 소유한다.
- request-side runtime vocabulary와 lifecycle 상태 canonical은 `../engine/request-lifecycle.md`가 소유한다.

## 위치
- 도메인 객체 ontology와 도메인 상태 축의 기반: `foundation.md`
- 자연어 요청 해석 축: 이 문서
- request lifecycle 기준: `../engine/request-lifecycle.md`
- supervisor 구조 확장 기준: `../engine/supervisor-structure.md`

## 사용자 질의 해석 기준
현재 foundation에서는 자연어 요청을 아래 의미 축으로 해석한다.

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
다음 정보가 비면 clarify가 필요한 후보로 본다.
- carrier id
- lot id
- equipment type
- equipment id/name
- metric time range
- log keyword / level / time window
- action 대상

## 경계
- 이 문서는 자연어 요청을 어떤 intent / entity / scope / missing-slot 축으로 해석할지 정의한다.
- 이 문서는 특정 저장소의 classifier, parser, prompt, router 구현을 고정하지 않는다.
- 반도체/AMHS 객체 자체의 의미와 관계는 `foundation.md`와 `semiconductor-amhs.md`가 소유한다.
- request 상태 canonical과 request-side vocabulary는 `../engine/request-lifecycle.md`가 소유한다.
- supervisor / trace / policy의 확장 구조는 engine 문서가 소유한다.
