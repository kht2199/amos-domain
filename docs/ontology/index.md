# Ontology Index

## 이 폴더의 역할
반도체/AMOS 관련 의미 체계와 ontology 기준 문서를 이 폴더 안에서 active하게 관리한다.

이 인덱스는 **ontology 폴더 내부 기준**만 빠르게 보여준다.
폴더 바깥 문서 읽기 순서나 구현 저장소 self-check 안내는 여기서 소유하지 않는다.

## 이 폴더 안의 확인 순서
1. `semiconductor-amhs.md`
2. `foundation.md`
3. `request-interpretation.md`

## 문서
- `semiconductor-amhs.md` — 반도체/AMHS 상위 개념
- `foundation.md` — 현재 foundation 기준의 도메인 객체/상태 축
- `request-interpretation.md` — 자연어 요청의 intent / target / scope / missing-slot 해석 축

## 원칙
- 공통 개념은 여기서 정의한다.
- 구현 프로젝트가 생겨도 핵심 용어는 여기 기준을 참조한다.
- 새로운 용어는 ontology 관계 또는 별도 agent/engine 문서의 canonical 축에 연결될 때만 승격한다.
- 저장소별 클래스/필드/API와의 구체적 매핑표는 각 구현 저장소에서 관리한다.
