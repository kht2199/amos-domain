# Docs Index

이 파일은 폴더 구조만 빠르게 보여주는 보조 인덱스다.
**문서 역할 분류와 읽는 순서는 루트 [README.md](../README.md)를 primary hub로 사용한다.**

검증 순서 원칙:
- 먼저 `ontology/`에서 개념/관계와 도메인/요청 상태 canonical을 확인한다.
- 그 다음 `engine/`, `scenarios/`, `programs/`, `roadmap/`처럼 ontology를 사용하는 문서를 본다.
- 구현 저장소 self-check는 마지막에 `programs/implementation-repo-reference-guide.md`와 각 저장소 로컬 문서에서 확인한다.

## 폴더
- `ontology/` — 반도체/AMOS 개념, 관계, 도메인 객체/요청 상태 기준, ownership/source-of-truth 규칙
- `engine/` — ontology를 실제 runtime / trace / policy / supervisor 축으로 사용하는 문서
- `scenarios/` — ontology/engine 의미를 바탕으로 보는 scenario skeleton
- `programs/` — ontology/engine 기준을 구현 저장소에 연결하는 가이드와 책임 지도
- `roadmap/` — ontology와 engine 기준을 실제 정렬/점검 순서로 푸는 실행 계획과 checklist
- `archive/` — history / transitional notes
