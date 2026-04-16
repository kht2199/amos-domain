# AMOS 실전 원칙

> 이 문서는 이제 AMOS 반도체/운영 모델의 active 실전 원칙 문서다.
> 기존 구현 저장소의 동명/유사 문서는 중복 방지를 위해 이 문서를 가리키는 포인터로만 유지한다.

이 문서는 AMOS를 문서용이 아니라 **실제로 잘 동작하게 만들기 위한 운영 원칙**을 정리한다.

## 0. 먼저 볼 문서
- runtime vocabulary / 상태 canonical: `../ontology/minimal-runtime-model.md`
- foundation 엔진 구조: `../phase/foundation-engine.md`
- active checklist: `../roadmap/ontology-implementation-checklist.md`
- 구현 저장소 참고 순서: `../programs/implementation-repo-reference-guide.md`
- supervisor 판단 축: `supervisor-structure.md` *(later expansion reference)*
- execution trace 구조: `execution-trace-structure.md` *(later expansion reference)*
- policy / permission 구조: `policy-permission-structure.md` *(later expansion reference)*

이 문서는 위 문서들을 실제로 운용할 때의 실전 원칙을 덧붙이는 위치다.

중요:
- 현재 foundation에서 지금 당장 맞춰야 하는 우선순위는 active checklist를 따른다.
- supervisor / trace / policy-permission 문서는 현재 active checklist를 보조하는 later expansion reference다.

## 1. 문서보다 런타임 정합성을 우선한다
- README/GUIDE에 있는 용어와 실제 코드 의미가 다르면 코드를 기준으로 문서를 즉시 맞춘다.
- ontology 문서는 예쁜 정의집이 아니라 runtime, schema, trace, policy와 실제로 연결되어야 한다.
- 단, 저장소별 클래스/필드/API/tool 매핑 상세표는 각 구현 저장소에서 관리한다.
- source of truth는 두 층으로 본다.
  1. 개념/원칙/상태 의미/불변식: domain active 문서
  2. 실제 클래스/필드/API/event 이름: 각 구현 저장소의 코드와 테스트
- archive 문서는 두 층 모두에서 직접 기준으로 삼지 않는다.

## 2. 질문 해석보다 실행 실패를 더 무겁게 본다
- 잘못된 친절한 답변보다 안전한 `clarifying` 전환이 낫다.
- `failed`가 발생했는데도 억지로 다음 task로 넘어가지 않는다.
- action 계열은 승인 없이 자동 실행하지 않는다.

## 3. Foundation은 최소 의미 체계에 집중한다
지금 당장 반드시 맞춰야 하는 축만 유지한다.
- entity: Carrier / Lot / Equipment / Fab / Request / Task / ExecutionTrace
- equipment type: AGV / STK / CNV / LFT
- state: pending / clarifying / executing / completed / blocked / failed
- event: stream_start / plan_update / agent_start / tool_call / tool_result / clarify / trace / error / done
- policy: completed/failed task 불변, 단일 active task, failed 이후 무리한 후속 실행 금지, approval guard

새 개념이 들어오면 먼저 묻는다.
- 지금 routing에 필요한가?
- trace에 남겨야 하는가?
- KPI 계산에 필요한가?
- 현재 foundation 범위를 넘는가?

## 4. 새 용어는 반드시 연결 지점을 가진다
새 용어를 정의할 때는 아래 5개 중 최소 1개를 같이 적는다.
- 어떤 agent가 쓰는가
- 어떤 tool과 연결되는가
- 어떤 schema field에 들어가는가
- 어떤 trace/event로 관찰되는가
- 어떤 KPI에 영향을 주는가

연결 지점이 없으면 아직 ontology 항목으로 올리지 않는다.

## 5. trace는 디버그 로그가 아니라 운영 증거다
- 왜 그 agent가 선택됐는지
- 왜 clarify가 발생했는지
- 왜 `blocked` 또는 `failed`로 멈췄는지
- 왜 approval이 필요했는지
를 나중에 복기할 수 있어야 한다.

따라서 trace message는 막연하게 쓰지 않는다.
- 나쁨: `처리함`
- 좋음: `carrier_agent completed after get_carrier success`

## 6. request log는 나중에 replay/KPI로 재사용 가능해야 한다
- SSE 이벤트는 화면 표시용으로만 만들지 않는다.
- request/event/trace는 이후 replay, 장애 분석, KPI 계산에 재활용 가능해야 한다.
- 이벤트 이름과 payload shape는 자주 흔들지 않는다.

## 7. archive 문서를 active 기준으로 착각하지 않는다
- `docs/archive/`는 참고/보관용이다.
- 현재 구현 방향은 active docs에서 관리한다.
- 기존 초안에서 좋은 내용이 있어도 active 문서로 옮겨온 뒤에만 기준으로 삼는다.

## 8. 실전에서 지키기 위한 체크리스트
변경 전에 확인:
- 이 변경이 ontology 용어를 바꾸는가?
- 바뀌면 README/GUIDE/active ontology 문서도 함께 맞췄는가?
- supervisor rule/test와 충돌하지 않는가?
- trace/event shape가 깨지지 않는가?

변경 후 확인:
- 관련 테스트가 있는가?
- request log에서 복기 가능한가?
- 사용자가 봤을 때 용어가 더 명확해졌는가?
- foundation 범위를 불필요하게 넓히지 않았는가?

## 9. 지향하는 결과
이 원칙의 목표는 세 가지다.
1. 사용자가 묻는 반도체/AMOS 개념이 문서와 코드에서 같은 뜻으로 읽힌다.
2. supervisor와 worker가 같은 의미 체계를 공유한다.
3. 나중에 simulator/digital twin으로 확장할 때도 현재 로그/trace/정책이 버려지지 않는다.
