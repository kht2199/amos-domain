# Ontology Index

## 이 폴더의 역할
반도체/AMOS 관련 의미 체계와 ontology 기준을 이 폴더 안에서 active하게 관리한다.

이 인덱스는 **ontology 폴더 내부 기준**만 빠르게 보여준다.
폴더 바깥 문서 읽기 순서나 구현 저장소 self-check 안내는 여기서 소유하지 않는다.

## 이 폴더 안의 확인 순서
1. `common-semiconductor-amhs.md`에서 공통 반도체/AMHS 상위 개념을 먼저 확인한다.
2. `../phase/foundation-ontology.md`에서 현재 foundation ontology 범위, 관계, 해석 축을 확인한다.
3. `minimal-runtime-model.md`에서 공통 runtime vocabulary와 최소 상태 canonical을 확인한다.
4. `source-of-truth.md`에서 ontology 문서와 다른 층위 문서의 ownership 경계를 확인한다.
5. 필요할 때만 `consolidation-status.md`, `skill-alignment.md`로 정리 상태와 governance 메모를 확인한다.

## 문서
- `common-semiconductor-amhs.md` — 공통 반도체/AMHS 상위 개념
- `../phase/foundation-ontology.md` — 현재 foundation ontology 기준
- `minimal-runtime-model.md` — foundation 공통 용어와 최소 상태 집합 canonical
- `source-of-truth.md` — ontology 문서와 타 문서 층위의 ownership / source-of-truth 규칙
- `consolidation-status.md` — canonical / pointer / archive 정리 상태
- `skill-alignment.md` — skill과 domain canonical의 역할 분리 기준

## 원칙
- 공통 개념은 여기서 정의한다.
- 구현 프로젝트가 생겨도 핵심 용어는 여기 기준을 참조한다.
- 새로운 용어는 ontology 관계와 최소 runtime vocabulary에 연결될 때만 승격한다.
- 저장소별 클래스/필드/API와의 구체적 매핑표는 각 구현 저장소에서 관리한다.
