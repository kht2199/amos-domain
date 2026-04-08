# Phase 1 Engine

## 목표
Phase 1에서는 simulator 전체를 만드는 것이 아니라, 이후 simulator/digital twin이 올라갈 수 있는 최소 엔진 개념을 먼저 정리한다.

## 핵심 엔진 축
### 1. Request Engine
- 사용자 질의를 request 단위로 받는다.
- tracking_id 기준으로 요청을 식별한다.
- request는 plan step과 trace를 생성한다.

### 2. Routing Engine
- supervisor가 intent / target / missing slots를 보고 worker를 선택한다.
- clarify가 필요한 경우 즉시 멈추고 재질문한다.

### 3. Execution Engine
- worker agent가 tool 호출을 수행한다.
- tool call / tool result를 구조화 이벤트로 남긴다.

### 4. Observation Engine
- stream_start → plan_update → agent_start → tool_call/tool_result → trace → done 흐름을 관찰 가능하게 남긴다.
- replay와 KPI 계산이 가능하도록 이벤트 모양을 안정적으로 유지한다.

### 5. Policy Engine
- approval guard
- single in_progress step
- failed 시 END
- clarify 우선 정책

## Phase 1 비포함
- full discrete-event simulation
- OHT/bridge/rail topology solver
- real-time data sync
- fast predictor model serving

## 이후 확장 연결
Phase 1 엔진은 나중에 아래로 확장된다.
- scenario generator
- replay engine
- synthetic data pipeline
- full simulator
- digital twin sync adapter
