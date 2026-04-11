# Source of Truth Rules

## 우선순위
### 1) 개념/원칙 기준
1. 이 프로젝트의 active 문서
2. 구현 프로젝트(예: agent/backend/frontend)의 관련 문서
3. Honcho/session 기록
4. archive 문서

### 2) 구현/매핑 기준
1. 각 구현 저장소의 실제 코드와 테스트
2. 각 구현 저장소의 관련 문서
3. 이 프로젝트의 active 문서
4. Honcho/session 기록
5. archive 문서

## 왜 이렇게 두는가
- repo 문서는 diff와 review가 가능하다.
- 코드는 실제 동작을 결정한다.
- domain 문서는 개념과 원칙의 기준을 고정한다.
- 저장소별 개념 ↔ 코드 매핑은 각 구현 저장소가 가장 정확하게 관리할 수 있다.
- Honcho는 대화 맥락을 회수하는 데 강하지만, 기준 문서를 대체하면 안 된다.
- archive는 참고/역사 보존용이다.

## 운영 규칙
- 오늘 논의는 Honcho에 남아 있어도, 결정 사항은 active 문서에 반영해야 한다.
- 과거 대화를 찾고 싶으면 Honcho를 검색한다.
- 새로운 phase 결론은 roadmap 문서에 먼저 적고, 필요 시 ontology/engine/scenario 문서에 승격한다.
- domain은 “무엇을 의미하는가”를 정의하고, 구현 저장소는 “그 의미가 코드에서 어디에 놓이는가”를 정의한다.
