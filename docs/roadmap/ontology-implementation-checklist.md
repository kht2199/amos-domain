# Ontology Implementation Checklist

이 문서는 archive에 있던 `amos-ontology-implementation-checklist.md` 의 살아있는 내용을 domain 프로젝트의 active 로드맵으로 승격한 버전이다.

## 목적
ontology를 문서로만 두지 않고, supervisor / clarify / trace / policy / retrieval에 점진적으로 연결한다.

이 문서는 domain 기준과 구현 정렬 순서를 정의한다.
저장소별 상세 매핑표 자체는 각 구현 저장소에서 관리한다.

## Phase 1 — 지금 바로 연결할 것
### 1. 용어와 최소 모델 고정
- `Request`, `Task`, `ToolCall`, `Policy`, `ClarifyRequest`, `ExecutionTrace` 용어를 현재 코드와 대조
- canonical 정의는 `../ontology/minimal-runtime-model.md`를 기준으로 삼는다
- supervisor / API 응답 / trace 이벤트에서 같은 의미를 같은 이름 또는 명시적 alias로 부르도록 정리
- 저장소별 개념 ↔ 코드 매핑 문서 작성 기준 확정

### 2. 최소 상태 집합 정리
- canonical 상태 집합의 기준 문서는 `../ontology/minimal-runtime-model.md`다.
- `pending`
- `clarifying`
- `executing`
- `completed`
- `blocked`
- `failed`

### 3. 저장소별 개념 ↔ 코드 매핑 문서 정리
- 각 구현 저장소(agent/backend/frontend 등)는 자신의 코드 구조 기준으로 매핑 문서를 유지한다.
- domain은 매핑표 자체의 저장소가 아니라, 어떤 개념을 맞춰야 하는지 기준만 제공한다.
- 매핑 문서에는 아래 항목을 포함한다.
  - domain concept
  - local class / schema / API / event name
  - 일치 여부
  - 누락/편차
  - 정리 예정 항목

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

## Phase 4 — Policy / Permission 연결
- tool risk tier 기준
- confirmation 필요 조건
- block 조건
- clarify 우선 조건
- `allow/deny/confirm/clarify` decision 레이어

## Phase 5 — Retrieval 구조화
- entity-aware retrieval 최소 도입
- source / trust level / tags / entity reference 메타데이터 전략
- retrieval 단계에서의 trust/policy 반영 검토

## Phase 6 — Clarify 패턴 정리
- 자주 빠지는 slot 목록화
- 옵션형 clarify 패턴 정리
- 대표 clarify 예시 축적

## 실행 순서 참고
실행 우선순위와 남은 작업 목록은 `implementation-roadmap.md`를 따른다.

이 체크리스트는
- 어떤 semantic alignment 축을 언제 정리할지
- Phase 1~6에서 무엇을 완료 기준으로 볼지
를 고정하는 문서로 유지한다.

## 현재 domain 문서 사용 순서
구현 저장소가 실제 읽는 순서와 self-check 질문은 `../programs/implementation-repo-reference-guide.md`를 따른다.

이 문서는 문서 reading guide가 아니라,
- 어떤 semantic alignment 축을 언제 정리할지
- Phase 1~6에서 무엇을 완료 기준으로 볼지
를 정리하는 roadmap / checklist 역할을 맡는다.

중요:
- domain은 의미 / 경계 / 판단 축 / 상태 canonical을 소유한다.
- 저장소별 class / field / API / event / state 이름은 각 구현 저장소가 소유한다.
- 이 체크리스트는 구현 세부안 설계서가 아니라, 구현 저장소가 self-check할 기준을 정리하는 문서다.
