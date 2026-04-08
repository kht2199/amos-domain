# Phase 1 Scenarios

## 목적
Phase 1에서는 시뮬레이터를 완성하지 않고도, 나중에 데이터를 쌓고 replay/synthetic generation으로 확장할 수 있게 시나리오 골격을 먼저 만든다.

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

## Phase 2로 넘길 시나리오
- synthetic fab traffic
- OHT congestion
- stocker/lift handoff
- alarm replay
- what-if prediction
