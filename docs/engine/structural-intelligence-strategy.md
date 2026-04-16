# Structural Intelligence Strategy

> 이 문서는 **secondary strategy** 문서다. near-term active checklist나 canonical runtime contract를 직접 대체하지 않으며, 전체 문서 지도는 루트 [README.md](../../README.md)를 따른다.

이 문서는 legacy structural strategy archive에 있던 살아있는 전략 개념을 active 문서로 승격한 것이다.

## 핵심 메시지
AMOS는 단순한 문서 검색 챗봇이 아니라,
**도메인 엔티티·관계·정책·행동을 연결하는 구조적 지식/행동 시스템**으로 발전해야 한다.

## 왜 필요한가
단순 chunk RAG만으로는 다음이 약하다.
- 엔티티 관계 설명
- 정책/권한 일관성
- 실행 가능한 액션 연결
- 실패/clarify/trace 복기

따라서 ontology는 문서 분류표가 아니라 다음과 연결되어야 한다.
- retrieval
- policy / permission
- clarify
- execution trace
- runtime state

## 전략 축
### 1. GraphRAG 방향
장기적으로 retrieval은 아래 3층으로 간다.
- chunk retrieval
- entity retrieval
- subgraph retrieval

즉 문서 검색만이 아니라,
- 어떤 entity가 등장하는지
- 어떤 관계가 있는지
- 어떤 subgraph가 현재 질의에 relevant한지
를 함께 다뤄야 한다.

### 2. ReBAC 방향
권한은 단순 role만으로 끝나지 않는다.
관계 기반 접근제어 후보를 열어둔다.

예:
- 사용자가 특정 workspace/member 관계 안에 있는가?
- 현재 session/request가 해당 범위에서 허용되는가?
- 특정 artifact/tool/action이 관계 그래프상 노출/실행 가능한가?

## 3. Meta-ontology 축적
장기적으로 아래 패턴을 누적 자산으로 본다.
- 반복 질의 패턴
- 반복 clarify 유형
- 승인/거절 패턴
- 실패/복구 패턴
- supervisor handoff 패턴

이 축적은 단순 FAQ보다 더 큰 가치를 가진다.

## 4. Domain-first 에이전트
에이전트 흐름은 장기적으로 다음 순서를 따른다.
1. intent 분류
2. entity candidate 추출
3. 관련 문서 + entity + subgraph 검색
4. 정책/권한 확인
5. 행동 계획 생성
6. 실행 또는 clarify
7. trace 구조화 기록

## 현재 active 적용 원칙
Phase 1에서는 위 전략을 전부 구현하지 않는다.
대신 아래를 우선한다.
- missing slot 기반 clarify
- trace 구조화
- permission decision 최소 레이어
- entity-aware retrieval의 최소 도입 여지 확보

## 하지 않을 것
- 처음부터 거대한 ontology graph 완성 시도
- archive 전체를 active 문서로 되돌리기
- 문서만 늘리고 코드/trace 연결이 없는 설계
