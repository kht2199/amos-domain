# Phase 1 Scenarios

> 이 문서는 Phase 1의 **scenario skeleton** 을 유지하는 active 문서다.
> full simulator spec이나 replay engine 세부 설계 문서가 아니라, 지금 어떤 요청/실패/관찰 흐름을 먼저 예시화할지 정리한다.

## 목적
Phase 1에서는 시뮬레이터를 완성하지 않고도, 나중에 데이터를 쌓고 replay/synthetic generation으로 확장할 수 있게 시나리오 골격을 먼저 만든다.

중요:
- 지금 당장 맞춰야 하는 semantic alignment 우선순위는 `../roadmap/ontology-implementation-checklist.md`를 따른다.
- 이 문서는 시나리오 예시 축을 정리하는 보조 문서이며, 구현 저장소별 request/event/trace 샘플은 각 저장소의 로컬 문서가 소유한다.

## 기본 시나리오 유형
### 1. 정상 질의 시나리오
- carrier 위치 질의
- lot 상태 질의
- metric 조회
- system status 조회

### 2. clarify 시나리오
- carrier id 누락
- time range 누락
- 장비 범위 누락

### 3. 실패 시나리오
- tool timeout
- route mismatch
- no data
- approval required

### 4. 관찰 시나리오
- request log 상세 복기
- trace 기반 실패 원인 확인
- done/error 흐름 점검

## 이 문서에서 다음에 보강할 것
> Phase 1 전체 우선순위와 남은 작업의 기준은 `../roadmap/implementation-roadmap.md`를 따른다.

이 문서에서는 시나리오 skeleton을 보강하는 범위만 관리한다.
1. 각 시나리오 유형마다 최소 1개 이상 예시 request를 연결
2. clarify / blocked / failed 가 어떻게 구분되는지 request/trace 예시를 덧붙임
3. 구현 저장소에서 실제로 남기는 event / trace / request log 샘플 경로를 링크
4. replay/synthetic generation으로 넘어갈 때 어떤 시나리오가 재사용되는지 표시

## Phase 2로 넘길 시나리오
- synthetic fab traffic
- OHT congestion
- stocker/lift handoff
- alarm replay
- what-if prediction
