# AMOS Domain - Development Notes

## 작업 범위 원칙

- 이 `domain` 저장소 작업에서는 ontology / engine / scenarios / programs / roadmap 문서를 active source-of-truth로 다룬다.
- 사용자가 명시적으로 요청하지 않으면 `agent`, `backend`, `frontend`, `glb-editor` 같은 구현 저장소의 코드나 문서를 함께 수정하지 않는다.
- 구현 저장소의 클래스명, 필드명, API 경로, 이벤트 이름은 여기서 canonical로 고정하지 않는다.
- 구현 세부 매핑은 각 저장소의 로컬 문서가 소유하고, 이 저장소는 개념/원칙/경계/상태 의미를 소유한다.
- 이 저장소의 역할은 구현 저장소별 세부 구현안을 대신 정하는 것이 아니라, 각 저장소가 self-check 할 수 있도록 reference 자료, 읽는 순서, 점검 기준을 명확히 제공하는 것이다.
- 구현 저장소는 domain 문서를 참고하되, 실제 반영 방식은 각 저장소가 자신의 코드 / 테스트 / concept-to-code mapping 문서에서 자율적으로 결정한다.

## 구현 저장소 안내 원칙

- 구현 저장소에 공통으로 안내할 우선 reference는 아래 문서들이다.
  - `docs/programs/implementation-repo-reference-guide.md`
  - `docs/ontology/minimal-runtime-model.md`
  - `docs/roadmap/ontology-implementation-checklist.md`
  - `docs/engine/supervisor-structure.md`
  - `docs/engine/execution-trace-structure.md`
  - `docs/engine/policy-permission-structure.md`
- 위 3개 engine 문서는 Phase 2 full expansion reference이며, Phase 1에서는 self-check용 선참고 문서로 본다.
- domain 문서는 canonical 의미와 판단 축을 제공하고, 저장소별 `docs/concept-code-mapping.md` 및 실제 코드/테스트가 로컬 구현 기준을 소유한다.
- domain 저장소에는 저장소별 구현 매핑 표를 복제하지 말고, 필요한 경우 “무엇을 참고해야 하는지”와 “어떤 기준으로 보면 되는지”만 남긴다.

## 문서 정리 원칙

- `docs/ontology/minimal-runtime-model.md` 를 Phase 1 runtime vocabulary / 상태 canonical 기준으로 우선 참조한다.
- `docs/roadmap/ontology-implementation-checklist.md` 는 Phase 1 active checklist이고, `implementation-repo-reference-guide.md` 는 읽는 순서와 질문을 안내하는 guide다.
- `phase1.md` 는 큰 그림과 관계를, `phase1-engine.md` 는 엔진 구조와 observability 관점을 담당한다.
- Honcho 같은 agent 운영용 기억/맥락 회수 메커니즘은 canonical docs에 퍼뜨리지 말고, 필요하면 `AGENTS.md` 같은 작업 규칙 문서에서만 다룬다.
- 이미 통합된 내용은 중복 문서로 다시 늘리지 말고 pointer 문서 또는 링크로 정리한다.
- 저장소별 concept-to-code mapping 표는 이 저장소에 복제하지 말고, 필요한 경우 링크와 점검 기준만 남긴다.

## Archived docs handling

- `docs/archive/` 또는 archive 성격 문서는 normal development의 active 기준으로 사용하지 않는다.
- archive 문서는 역사 보존/맥락 확인용으로만 보고, 살아있는 개념만 active 문서로 승격한다.
- pointer 문서는 active canonical이 아니며, 이미 통합된 기준 문서를 안내하는 역할만 수행한다.
