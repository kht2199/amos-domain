# Ontology Consolidation Status

> 이 문서는 **governance / transitional** 문서다. primary domain canon 자체가 아니라, canonical / pointer / archive 정리 상태를 추적한다.

## 목표
반도체/AMOS ontology를 이 저장소 아래의 단일 기준으로 통합하고, 다른 위치의 active 중복 문서는 제거하거나 포인터로 바꾼다.

## 현재 canonical domain 문서
- `common-semiconductor-amhs.md`
- `phase1.md`
- `../engine/practical-principles.md`
- `../engine/phase1-engine.md`
- `../scenarios/phase1-scenarios.md`
- `../programs/program-map.md`
- `../roadmap/implementation-roadmap.md`

## 현재 canonical alignment / semantic contract 문서
- `minimal-runtime-model.md`
- `../engine/supervisor-structure.md`
- `../engine/execution-trace-structure.md`
- `../engine/policy-permission-structure.md`
- `source-of-truth.md`

## 현재 active reference guide
- `../programs/implementation-repo-reference-guide.md`

## 현재 pointer 문서
- `../engine/runtime-observability.md` → 핵심 내용은 `../engine/phase1-engine.md`로 통합
- `../programs/project-classification.md` → 핵심 내용은 `../programs/program-map.md`로 통합

## 중복 정리 결과
### agent 저장소
기존 active 중복 문서:
- `../agent/docs/amos-ontology-phase1.md`
- `../agent/docs/amos-practical-principles.md`

현재 상태:
- 두 파일 모두 내용을 유지하지 않고, canonical domain 문서를 가리키는 포인터 문서로 전환함.
- `agent/README.md`, `agent/docs/GUIDE.md` 링크도 canonical domain 문서를 가리키도록 변경함.

### archive 문서
다음 파일은 아직 archive/reference로 남아 있다.
- `../agent/docs/archive/amos-ontology-draft-0.1.md`
- `../agent/docs/archive/amos-ontology-implementation-checklist.md`
- `../agent/docs/archive/sos-summary.md`

이 파일들은 active source of truth가 아니다.
필요한 개념만 선별해 domain 프로젝트로 승격한다.

### skill
- skill은 빠른 참조와 해석 규칙을 제공한다.
- active 기준은 domain repo 문서가 우선이다.

참고:
- canonical / pointer / governance 전체 문서 지도는 루트 `README.md`를 primary hub로 본다.
- 이 문서는 목록을 다시 정의하는 문서가 아니라, 현재 정리 상태와 남은 정리 대상을 추적하는 governance 문서다.

## 현재 남은 정리 대상
1. archive 문서에서 더 승격할 개념이 실제로 남았는지 재판단
2. skill references와 domain canonical 문서의 중복/차이 점검
3. 구현 저장소별 concept-to-code mapping 문서가 `minimal-runtime-model.md` 기준으로 계속 정렬되는지 점검
4. pointer 문서를 계속 유지할지, 충분히 안정화되면 archive로 내릴지 판단
