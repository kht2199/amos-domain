# AMOS Domain Foundation

AMOS의 반도체/AMHS 온톨로지, 엔진 개념, 시나리오, 프로그램 구성, 로드맵을 한곳에서 관리하는 독립 문서 프로젝트다.

## 목적
- 반도체/AMOS 관련 ontology를 한곳에서 active하게 관리한다.
- simulator → digital twin으로 확장 가능한 구조를 미리 잡는다.
- 프로젝트별 구현 문서와 공통 도메인 문서를 분리하되, 연결은 유지한다.
- 실전 원칙을 문서 수준이 아니라 runtime/trace/policy 기준으로 유지한다.

## 디렉토리 구조
- `docs/ontology/` — 공통 반도체/AMHS 개념, Phase 1 ontology, source-of-truth 기준
- `docs/engine/` — runtime 의미 체계, trace/event/policy, 실전 원칙
- `docs/scenarios/` — synthetic scenario, replay, 장애/오류 주입 방향
- `docs/programs/` — 어떤 프로그램/서비스/저장소가 어떤 책임을 갖는지 정리
- `docs/roadmap/` — phase별 구현 순서, 오늘 논의 요약, 다음 단계

## 읽는 순서
1. `docs/ontology/index.md`
2. `docs/ontology/phase1.md`
3. `docs/engine/practical-principles.md`
4. `docs/engine/runtime-observability.md`
5. `docs/roadmap/implementation-roadmap.md`

## source of truth 원칙
- 이 프로젝트의 문서가 ontology/engine/scenario/roadmap의 **active source of truth** 다.
- Honcho 기록은 오늘 논의나 과거 맥락을 다시 찾는 **보조 기억 장치** 다.
- 즉, **결정사항은 repo 문서**, **대화 맥락 회수는 Honcho** 로 역할을 나눈다.

## 관련 저장소
- `amos-domain` — 반도체/AMHS ontology, engine, scenario, roadmap의 기준 문서 저장소
- `amos-agent` — 운영 질의/설명/도구 orchestration 성격의 agent 저장소
- `amos-frontend` — UI/시각화 및 프론트엔드 구현 저장소

## 사용 원칙
- ontology/engine/scenario/roadmap의 기준 변경은 먼저 이 저장소에서 수정한다.
- 다른 저장소에는 가능하면 요약보다 **포인터 링크** 를 두고 중복 복제를 피한다.
- archive 성격 문서는 active source of truth와 분리해서 관리한다.
- 구현 저장소의 코드 구조가 바뀌더라도 도메인 의미 체계는 여기서 먼저 정리한다.

## 현재 초점
- Phase 1 ontology 정리
- runtime/trace/policy 의미 체계 정리
- simulator → digital twin 확장을 위한 roadmap 정리
- 여러 저장소(agent/frontend/backend/infra)와의 역할 경계 명확화
