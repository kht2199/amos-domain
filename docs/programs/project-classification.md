# Project Classification — core / adapter / archive

## 목적
`/Users/htkim/IdeaProjects/amos` 아래 프로젝트를
시뮬레이션 / 디지털 트윈 관점에서 다시 분류한다.

이 분류의 목적은 단순 정리가 아니라,
앞으로 **무엇을 중심으로 키우고**, **무엇을 연결 레이어로 낮추고**, **무엇을 보관 대상으로 둘지**를 명확히 하는 것이다.

## 분류 기준

### 1. core
다음 조건을 만족하면 core로 본다.
- simulator / digital twin의 개념 중심을 직접 담는다.
- ontology / state / event / policy / scenario / replay / KPI / prediction 같은 핵심 축을 관리한다.
- 다른 저장소가 이 프로젝트를 기준으로 의미 체계를 맞춘다.
- 장기적으로 가장 먼저 확장되어야 한다.

### 2. adapter
다음 조건을 만족하면 adapter로 본다.
- core를 직접 구현하지는 않지만, core를 외부 시스템/사용자/운영 환경과 연결한다.
- UI, API facade, 기존 운영 시스템 연동, 배포/관측 같은 역할을 담당한다.
- core 없이 단독으로는 방향성이 약하지만, 연결층으로는 가치가 있다.

### 3. archive / legacy
다음 조건을 만족하면 archive 또는 legacy로 본다.
- 현재 목표인 simulator / digital twin과 중심축이 어긋난다.
- 일부 자산은 참고할 가치가 있지만, 더 이상 주 설계 기준이 아니다.
- 필요시 일부만 추출하고, 전체 프로젝트를 중심으로 계속 키우지는 않는다.

## 현재 프로젝트 분류

| 프로젝트 | 분류 | 이유 | 앞으로의 역할 |
|---|---|---|---|
| `domain` | **core** | ontology, engine, scenario, programs, roadmap를 active source of truth로 관리한다. simulator → digital twin 확장 방향의 기준점이다. | 주 기준 프로젝트 |
| `agent` | **adapter + legacy 성격** | 자연어 질의, supervisor, SSE, tool routing, request/trace 구조는 유효하지만 본질은 운영 질의/제어용 AI orchestration 계층이다. simulator core는 아니다. | explainability / ops assistant / trace UI 연계 후보 |
| `backend` | **adapter** | 설비/운영 API, controller, action, metric, status, log 연결점이다. 현실 시스템과의 접점이므로 digital twin sync 측면에서는 중요하지만 core ontology를 담는 곳은 아니다. | real-world integration / sync boundary |
| `frontend` | **adapter** | React/Vite UI, generated API hooks, mock workflow 중심이다. simulator core는 아니지만 trace/replay/KPI/explainability 화면의 그릇이 될 수 있다. | operator UI / replay viewer / observability UI |
| `infra` | **adapter** | 배포, Nginx, app server, LLM server, monitoring 운영 기반이다. 도메인 코어는 아니지만 서비스 운영에는 필요하다. | deployment / monitoring / runtime ops |
| `.migration_backups/*` | **archive** | 마이그레이션 전후 백업 산출물이다. 현재 active 개발 기준으로 삼지 않는다. | 역사 보존용 |

## 해석

### `domain`
이제 `domain`은 단순 문서 폴더가 아니라,
AMOS를 simulator / digital twin 방향으로 다시 세울 때의 **기준 저장소**다.

### `agent`
`agent`는 버릴 대상이라기보다,
**core에서 adapter로 격하해서 다뤄야 하는 프로젝트**다.

즉:
- ontology 기준 저장소는 아님
- simulator engine 저장소도 아님
- 하지만 trace / clarify / explainability / operator interaction 자산은 유효

### `backend`
`backend`는 현실 설비/운영 API 경계면으로서 가치가 있다.
특히 future digital twin에서:
- 실측 상태 유입
- action 실행 경계
- metric/log 동기화
- approval/제어 정책 반영
같은 지점으로 남을 가능성이 높다.

### `frontend`
`frontend`는 지금은 일반 운영 UI 성격이 강하지만,
앞으로는 아래 쪽으로 재배치 가능하다.
- replay viewer
- event timeline
- KPI dashboard
- explainability console
- scenario 비교 화면

### `infra`
`infra`는 domain 설계와는 거리가 있지만,
실서비스 단계에서 twin runtime / observability / deployment를 붙일 때는 계속 필요하다.

## 권장 운영 원칙
1. **core 판단은 `domain` 기준으로 한다.**
2. `agent/backend/frontend/infra`는 domain을 참조하는 구현/운영 저장소로 본다.
3. 레거시 프로젝트에서 살아있는 자산만 뽑아 core 설계에 연결한다.
4. 앞으로 새 저장소를 만들면 이름부터 역할이 드러나게 분리한다.
   - 예: `sim-core`, `twin-runtime`, `scenario-lab`, `ops-console`
5. archive는 보존하되, active source of truth처럼 취급하지 않는다.

## 현재 결론
- **Core**: `domain`
- **Adapter**: `backend`, `frontend`, `infra`
- **Adapter + legacy 성격**: `agent`
- **Archive**: `.migration_backups/*`

즉 현재 AMOS의 방향을 simulator / digital twin으로 재정렬할 때,
`domain`을 중심축으로 삼고 나머지는 주변 구현/연결 계층으로 재배치하는 것이 가장 자연스럽다.
