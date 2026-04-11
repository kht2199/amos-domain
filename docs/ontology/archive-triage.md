# Archive Triage

> 이 문서는 **governance / transitional** 문서다. primary domain canon 자체가 아니라, archive 문서의 승격/유지/폐기 판단을 관리한다.

## 목적
`agent/docs/archive/` 에 남아 있는 반도체/AMOS ontology 관련 문서를 계속 보관만 할지, active 문서로 승격할 개념이 있는지 판단한다.

## 판정 기준
- **승격**: 앞으로도 계속 쓰일 개념이며 canonical domain 문서에 반영할 가치가 있음
- **유지**: 과거 맥락/초안/역사용으로는 유효하지만 active source of truth로는 쓰지 않음
- **폐기 후보**: 내용이 거의 중복이고 역사적 가치도 낮아 나중에 제거 가능

## 파일별 판정
| 파일 | 판정 | 이유 | 조치 |
|---|---|---|---|
| `agent/docs/archive/amos-ontology-draft-0.1.md` | 유지 + 부분 승격 완료 | retrieval/policy/trace/clarify 중심 사고는 여전히 유효하지만, active 문서와 구현 로드맵에 대부분 흡수됨 | archive로 유지하되, 남은 전략 메모만 active 문서로 선별 승격 |
| `agent/docs/archive/amos-ontology-implementation-checklist.md` | 유지 + 부분 승격 완료 | 구현 체크리스트 성격은 살아 있으나 active roadmap로 옮기는 편이 맞음 | 핵심은 `docs/roadmap/ontology-implementation-checklist.md` 로 승격했고, 원본은 archive 유지 |
| `agent/docs/archive/sos-summary.md` | 유지 + 부분 승격 필요 | 구조적 지능, GraphRAG, ReBAC, meta-ontology 같은 장기 방향성 메모는 여전히 가치가 있음 | active 전략 문서로 핵심만 승격하고, 원본은 archive 유지 |

## 이번에 승격한/연결한 항목
### 이미 active로 승격된 것
- retrieval / policy / trace / clarify 중심 접근
- ontology를 runtime 동작과 연결해야 한다는 원칙
- supervisor / clarify / execution trace 구조화 체크리스트
- canonical / pointer / archive 분리 원칙

### 이번에 추가 승격한 것
- GraphRAG를 문서 검색이 아닌 entity/subgraph retrieval 확장으로 보는 관점
- ReBAC를 관계 기반 접근제어 후보로 보는 관점
- meta-ontology 축적이라는 장기 전략
- “챗봇”보다 “구조적 지식 시스템/행동 시스템”으로 본다는 framing

## 최종 판단
- archive 3개 문서는 지금 당장 삭제하지 않는다.
- 하지만 **active 판단 근거로 직접 참조하지 않는다.**
- 살아있는 개념만 domain 프로젝트 문서로 승격하고, archive 원문은 역사 보존용으로만 둔다.

## 다음 단계
1. archive 원문에 `superseded` 안내를 추가할지 검토
2. skill reference와 domain canonical의 역할 분리 유지
3. 구현 저장소별 개념 ↔ 코드 매핑 문서를 정리하고, domain에는 링크와 기준만 남긴다
