# Ontology Index

> 전체 문서 역할 분류와 읽는 순서는 루트 [README.md](../../README.md)를 primary hub로 사용한다.

## 이 폴더의 역할
반도체/AMOS 관련 의미 체계를 한곳에서 active하게 관리한다.

검증 순서:
1. 이 폴더에서 ontology 개념/관계/canonical 상태를 먼저 확인한다.
2. 그 다음 `../engine/`, `../scenarios/`, `../programs/`, `../roadmap/` 문서가 이 ontology를 어떻게 사용하는지 본다.
3. 구현 저장소별 self-check는 마지막에 `../programs/implementation-repo-reference-guide.md`와 각 저장소 로컬 문서를 본다.

## 문서
- `common-semiconductor-amhs.md` — 공통 반도체/AMHS 상위 개념
- `phase1.md` — 현재 Phase 1 ontology 기준
- `minimal-runtime-model.md` — Phase 1 공통 용어와 최소 상태 집합 canonical
- `source-of-truth.md` — 어떤 문서/코드/기억이 어떤 역할을 가지는지 정리
- `consolidation-status.md` — canonical / pointer / archive 정리 상태
- `skill-alignment.md` — skill과 domain canonical의 역할 분리 기준

## engine 쪽 핵심 연결
- `../engine/practical-principles.md` — runtime / trace / policy 실전 원칙
- `../engine/phase1-engine.md` — Phase 1 엔진 구조
- `../engine/supervisor-structure.md` — supervisor 구조 확장 기준 *(Phase 2 expansion reference)*
- `../engine/execution-trace-structure.md` — execution trace 구조화 기준 *(Phase 2 expansion reference)*
- `../engine/policy-permission-structure.md` — policy / permission 구조화 기준 *(Phase 2 expansion reference)*

## 원칙
- 공통 개념은 여기서 정의한다.
- 구현 프로젝트가 생겨도 핵심 용어는 여기 기준을 참조한다.
- 새로운 용어는 runtime/trace/policy/관찰 지점 중 최소 하나와 연결될 때만 승격한다.
- 저장소별 클래스/필드/API와의 구체적 매핑표는 각 구현 저장소에서 관리한다.
