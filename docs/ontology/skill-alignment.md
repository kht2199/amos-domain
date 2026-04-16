# Skill ↔ Domain Alignment

> 이 문서는 **governance / transitional** 문서다. primary domain canon 자체가 아니라, skill과 domain canonical의 역할 분리를 설명한다.

## 목적
`semiconductor-amhs-ontology` 스킬과 domain canonical 문서의 역할을 분리하고, 중복/차이를 관리한다.

## 역할 분리
### domain canonical 문서
위치:
- `common-semiconductor-amhs.md`
- `foundation-ontology.md`
- `minimal-runtime-model.md`
- `../engine/practical-principles.md`
- `../phase/foundation-engine.md`
- `../engine/supervisor-structure.md`
- `../engine/execution-trace-structure.md`
- `../engine/policy-permission-structure.md`

역할:
- 현재 active source of truth
- 단계별 설계 기준
- runtime / trace / policy / roadmap와의 직접 연결

참고:
- `../engine/runtime-observability.md` 는 active canonical이 아니라 `../phase/foundation-engine.md`를 가리키는 pointer 문서다.

### active reference guide
위치:
- `../programs/implementation-repo-reference-guide.md`

역할:
- canonical 문서를 구현 저장소가 어떤 순서로 읽고 self-check 할지 안내
- semantic contract 자체를 새로 정의하지 않음

### skill
위치:
- `~/.hermes/skills/research/semiconductor-amhs-ontology/`

역할:
- 빠른 참조 허브
- 벤더 중립적 개념 요약
- 이미지/GLB/요구사항 해석 규칙
- 프로젝트 문서로 들어가는 입구

## 중복/차이 표
| 항목 | domain에 둘 것 | skill에 둘 것 | 비고 |
|---|---|---|---|
| foundation ontology | 예 | 아니오(링크/요약만) | active 기준은 domain 우선 |
| runtime state / trace / policy 연결 | 예 | 최소 설명만 | 구현 연결은 domain이 담당 |
| 공통 AMHS 용어표 | 예 | 예(축약본) | skill은 빠른 참조용 축약 허용 |
| 이미지 판독 규칙 | 아니오(필요 최소만) | 예 | skill 쪽 강점 |
| editor 매핑 규칙 | 프로젝트 성격에 따라 일부 | 예 | GLB/editor 작업용 규칙은 skill 유지 가치 큼 |
| Daifuku/Muratec 같은 벤더 읽기 팁 | 선택 | 예 | domain canonical의 중심은 벤더 중립이 좋음 |

## 현재 판단
### skill에 남길 것
- `references/ontology.md` 의 빠른 용어표
- `references/image-reading.md` 의 시각 판독 규칙
- `references/editor-mapping.md` 의 GLB/editor 매핑 규칙

### domain으로 승격/유지할 것
- ontology의 active 기준
- minimal runtime vocabulary / state canonical
- stage roadmap
- trace/policy/runtime 연결
- archive triage / consolidation status

### skill에서 직접 소유하지 않을 것
- 프로젝트별 foundation 상세 내용
- 구현 저장소와의 코드 매핑 상세표
- active roadmap/checklist 원문
- 이런 매핑 상세표의 정식 소유자는 domain이 아니라 각 구현 저장소다.

## 운영 규칙
1. domain 문서가 바뀌면 skill은 링크/요약만 갱신한다.
2. skill reference가 domain canonical과 충돌하면 domain을 우선한다.
3. skill은 project source of truth가 아니라 해석 허브다.
4. 프로젝트가 더 커지면 skill은 더 짧아지고, domain 문서는 더 풍부해져도 된다.
5. canonical / reference guide / governance 전체 문서 지도는 루트 `README.md`를 primary hub로 본다.
