# Runtime / Observability

## 현재 핵심 관찰 단위
- request
- stream
- event
- trace
- request log

## 핵심 이벤트
- stream_start
- plan_update
- agent_start
- tool_call
- tool_result
- token
- clarify
- trace
- error
- done

## 왜 중요한가
이 흐름은 단순 UI 표시용이 아니라 다음 기반이 된다.
- replay
- debugging
- KPI
- routing 개선
- 사용자 pain point 분석

## Phase 1 관찰 목표
- first feedback latency 측정 가능
- clarify rate 측정 가능
- tool success/failure 추적 가능
- route correction과 failed path 복기 가능

## 이후 확장
- 사용자 행동 이벤트
- 감성/불만 taxonomy
- 오류 분류 체계
- 장기 보관 저장소
- real system sync event
