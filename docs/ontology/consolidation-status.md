# Ontology Consolidation Status

## 목표
반도체/AMOS ontology를 `/Users/htkim/IdeaProjects/amos/domain` 아래의 단일 기준으로 통합하고, 다른 위치의 active 중복 문서는 제거하거나 포인터로 바꾼다.

## 현재 canonical 문서
- `docs/ontology/common-semiconductor-amhs.md`
- `docs/ontology/phase1.md`
- `docs/engine/practical-principles.md`
- `docs/engine/phase1-engine.md`
- `docs/engine/runtime-observability.md`
- `docs/scenarios/phase1-scenarios.md`
- `docs/programs/program-map.md`
- `docs/roadmap/implementation-roadmap.md`

## 중복 정리 결과
### agent 저장소
기존 active 중복 문서:
- `/Users/htkim/IdeaProjects/amos/agent/docs/amos-ontology-phase1.md`
- `/Users/htkim/IdeaProjects/amos/agent/docs/amos-practical-principles.md`

현재 상태:
- 두 파일 모두 내용을 유지하지 않고, canonical domain 문서를 가리키는 포인터 문서로 전환함.
- `agent/README.md`, `agent/docs/GUIDE.md` 링크도 canonical domain 문서를 가리키도록 변경함.

### archive 문서
다음 파일은 아직 archive/reference로 남아 있다.
- `/Users/htkim/IdeaProjects/amos/agent/docs/archive/amos-ontology-draft-0.1.md`
- `/Users/htkim/IdeaProjects/amos/agent/docs/archive/amos-ontology-implementation-checklist.md`
- `/Users/htkim/IdeaProjects/amos/agent/docs/archive/sos-summary.md`

이 파일들은 active source of truth가 아니다.
필요한 개념만 선별해 domain 프로젝트로 승격한다.

### skill / Honcho
- skill은 빠른 참조, 해석 규칙, Honcho 안내를 제공한다.
- active 기준은 domain repo 문서가 우선이다.
- 과거 논의 맥락은 Honcho에서 찾는다.

## 다음 정리 대상
1. archive 문서에서 살릴 개념 선별
2. skill references와 domain canonical 문서의 중복/차이 점검
3. 코드 매핑 표 작성
4. 구현 저장소(agent/backend/frontend)와 ontology 연결 표 작성
