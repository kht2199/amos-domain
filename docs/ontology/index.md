# Ontology Index

## 이 폴더의 역할
반도체/AMOS 관련 의미 체계를 한곳에서 active하게 관리한다.

## 문서
- `common-semiconductor-amhs.md` — 공통 반도체/AMHS 상위 개념
- `phase1.md` — 현재 Phase 1 ontology 기준
- `source-of-truth.md` — 어떤 문서/코드/기억이 어떤 역할을 가지는지 정리
- `consolidation-status.md` — canonical / pointer / archive 정리 상태
- `archive-triage.md` — archive 문서의 승격/유지/폐기 판단
- `skill-alignment.md` — skill과 domain canonical의 역할 분리 기준

## 원칙
- 공통 개념은 여기서 정의한다.
- 구현 프로젝트가 생겨도 핵심 용어는 여기 기준을 참조한다.
- 새로운 용어는 runtime/trace/policy/tool mapping 중 최소 하나와 연결될 때만 승격한다.
