# Ontology Implementation Checklist

이 문서는 archive에 있던 `amos-ontology-implementation-checklist.md` 의 살아있는 내용을 domain 프로젝트의 active 로드맵으로 승격한 버전이다.

## 목적
ontology를 문서로만 두지 않고, supervisor / clarify / trace / policy / retrieval에 점진적으로 연결한다.

## Phase 1 — 지금 바로 연결할 것
### 1. 용어와 최소 모델 고정
- `Request`, `Task`, `ToolCall`, `Policy`, `ClarifyRequest`, `ExecutionTrace` 용어를 현재 코드와 대조
- supervisor / API 응답 / trace 이벤트에서 같은 의미를 같은 이름으로 부르도록 정리
- 현재 코드 매핑표 작성

### 2. 최소 상태 집합 정리
- `pending`
- `clarifying`
- `executing`
- `completed`
- `blocked`
- `failed`

### 3. 현재 코드 매핑표 만들기
- supervisor 출력 필드
- clarify 이벤트 구조
- tool 실행 결과 구조
- 이미 있는 필드 / 없는 필드 표 작성

## Phase 2 — Supervisor 구조 확장
- `intent`
- `entity_candidates`
- `missing_slots`
- `risk_level`
- `policy_precheck`
- `clarify_question`
- `clarify_options`

완료 기준:
- supervisor가 단순 next agent 선택을 넘어서 구조적 판단 결과를 낸다.

## Phase 3 — Execution Trace 구조화
- request intent
- entity candidates
- missing slots
- policy decision result
- tool call success/failure
- final state

완료 기준:
- 한 요청이 왜 답변/차단/clarify/실패로 끝났는지 trace만 보고 재구성 가능하다.

## Phase 4 — Retrieval 구조화
- entity-aware retrieval 최소 도입
- source / trust level / tags / entity reference 메타데이터 전략
- retrieval 단계에서의 trust/policy 반영 검토

## Phase 5 — Policy / Permission 연결
- tool risk tier 기준
- confirmation 필요 조건
- block 조건
- clarify 우선 조건
- `allow/deny/confirm/clarify` decision 레이어

## Phase 6 — Clarify 패턴 정리
- 자주 빠지는 slot 목록화
- 옵션형 clarify 패턴 정리
- 대표 clarify 예시 축적

## 우선순위 추천
### 바로 할 것
1. supervisor 출력 구조 확장
2. clarify를 missing slot 기반으로 정리
3. execution trace 최소 스키마 정의
4. permission decision 최소 레이어 추가

### 그다음 할 것
5. entity-aware retrieval 최소 도입
6. trust level / source metadata 정리
7. clarify 패턴 라이브러리화

### 나중에 할 것
8. meta-ontology 축적
9. simulator / digital twin 확장 구조 반영
