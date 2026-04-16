# Policy / Permission Structure

## 목적
이 문서는 AMOS Expansion에서 `Policy` 와 `Permission` 을 단순 승인 여부 체크가 아니라, **요청 실행 가능성에 대한 구조적 판단 계층**으로 정리할 때 domain 수준에서 무엇을 고정해야 하는지 설명한다.

중요:
- 이 문서는 **후속 policy / permission decision layer 확장 기준**을 설명한다.
- 다만 Foundation에서도 `allow / deny / confirm / clarify` 구분을 self-check 할 수 있도록 reference contract로 선참고한다.
- Foundation에서 우선 맞춰야 하는 범위와 순서는 `../roadmap/ontology-implementation-checklist.md`를 따른다.
- 이 문서는 특정 저장소의 enum, payload, middleware, approval API schema를 canonical로 고정하지 않는다.
- 이 문서의 역할은 어떤 정책 판단이 필요한지, 그 판단이 `Request` / `Task` / `Policy` / `ClarifyRequest` / `ExecutionTrace` / runtime state와 어떻게 연결되는지 정의하는 것이다.
- 구현 저장소는 자신의 API / state / event / audit log 구조에서 이를 표현하며, ownership boundary는 `../ontology/source-of-truth.md`를 따른다.

## 위치
`Policy / Permission` 계층은 AMOS에서 다음 위치를 가진다.
- 입력: `Request`, `Task`, action target, user intent, risk context
- 판단 축: 허용 가능성, 확인 필요성, 차단 사유, 추가 정보 필요 여부
- 출력: `allow`, `deny`, `confirm`, `clarify`
- 상태 영향: `executing`, `blocked`, `clarifying`
- 근거 기록: `ExecutionTrace`

즉 policy는 단순 보안 부속 기능이 아니라, **실행해도 되는가 / 바로 실행해도 되는가 / 사용자 확인이 더 필요한가** 를 판정하는 domain 판단 계층이다.

## domain이 고정하는 최소 판단 축

### 1. `risk_tier`
요청 또는 실행 대상이 가지는 위험도 구간.

대표 예:
- `low`
  - 일반 조회
  - 요약/설명
  - 운영 상태 확인
- `medium`
  - 범위가 넓은 조회
  - 해석 오류 시 운영 혼선을 유발할 수 있는 요청
  - 결과가 의사결정에 직접 영향을 줄 수 있는 요청
- `high`
  - 실제 action 실행
  - 안전/운영 영향이 큰 대상 제어
  - 승인 또는 명시적 확인이 필요한 요청

의미:
- `risk_tier`는 confirm 필요 여부, deny 가능성, trace 우선순위와 연결된다.
- exact level 수는 구현 저장소가 조정할 수 있지만, domain 관점에서는 위험도 축 자체가 있어야 한다.

### 2. `permission_context`
실행 주체가 어떤 권한/역할/맥락에서 요청하는지 표현하는 축.

예:
- read-only operator
- action-capable operator
- admin / maintainer
- external automation
- unknown / unauthenticated context

의미:
- 같은 action이라도 누가 어떤 맥락에서 수행하는지에 따라 허용 가능성이 달라질 수 있다.
- permission은 단순 사용자 role 하나가 아니라, 요청 채널/주체/운영 상황을 포함한 맥락으로 보는 것이 좋다.

### 3. `policy_precheck`
실행 전에 선행 확인해야 하는 정책 조건.

대표 예:
- approval required
- maintenance window only
- safety lock active
- target not controllable
- read-only mode
- missing approval reason
- ambiguous action target

의미:
- supervisor의 `policy_precheck`는 policy 계층으로 연결되는 도메인 입구다.
- 정보가 충분해도 정책 조건이 맞지 않으면 실행하면 안 된다.

### 4. `decision`
정책 판단의 최소 결과 집합.

canonical 결과:
- `allow`
- `deny`
- `confirm`
- `clarify`

의미:
- `allow`: 바로 실행 가능
- `deny`: 현재는 실행 불가, `blocked` 쪽
- `confirm`: 위험/영향 때문에 명시적 사용자 확인 필요
- `clarify`: 정책 판단 이전에 더 필요한 정보가 있음

중요:
- `confirm`과 `clarify`는 다르다.
- `confirm`은 정보는 충분하지만 의사 확인이 더 필요한 상태다.
- `clarify`는 정책 판단이나 실행 전에 정보가 더 필요한 상태다.

### 5. `decision_reason`
왜 그 판단이 나왔는지의 구조화 근거.

예:
- high-risk action target
- approval token missing
- read-only channel
- safety constraint violation
- missing equipment id
- ambiguous target among multiple equipments

의미:
- deny/confirm/clarify가 왜 나왔는지 trace와 UI에서 재구성 가능해야 한다.
- 단순 free-text만 남기기보다 구조화 가능성을 염두에 두어야 한다.

### 6. `confirmation_requirement`
명시적 확인이 왜 필요한지 표현하는 축.

예:
- 실제 제어 action
- 영향 범위가 넓음
- 복구 비용이 큼
- 사용자 의도를 재확인해야 함

의미:
- `confirm`은 단순 UX 버튼 문제가 아니라, domain 차원의 안전/운영 통제 규칙과 연결된다.
- 향후 confirm 패턴을 library화할 때 기준이 된다.

### 7. `block_condition`
실행을 현재 진행할 수 없게 만드는 조건.

예:
- permission 부족
- approval 미충족
- maintenance / safety restriction
- disallowed target state
- organizational policy violation

의미:
- `blocked` 상태를 explainability 가능하게 만든다.
- 단순 실패(`failed`)와 구분되는 핵심 근거다.

## 최소 policy decision semantics
Expansion에서 policy / permission 계층은 로컬 구현 이름과 무관하게 최소한 아래 의미를 재구성할 수 있어야 한다.

### 필수 의미 축
- `risk_tier`
  - 요청/행위의 위험도 평가 결과
- `permission_context`
  - 어떤 권한/채널/주체 맥락에서 판단했는지
- `policy_precheck`
  - 어떤 정책 조건을 확인했는지
- `decision`
  - `allow / deny / confirm / clarify` 중 최종 판단
- `decision_reason`
  - 왜 그런 판단이 나왔는지에 대한 핵심 근거
- `block_conditions`
  - deny인 경우 어떤 제약이 실행을 막았는지
- `confirmation_requirement`
  - confirm인 경우 왜 확인이 필요한지
- `clarify_requirements`
  - clarify인 경우 어떤 정보가 더 필요한지
- `trace_reason` 또는 동등 의미 구조
  - trace/audit에 남길 요약 근거

### 선택 가능 필드
- `policy_actions`
  - approval 요청, confirm 대기, escalation 등 후속 액션
- `expires_at`
  - confirm/approval 유효 시간
- `approver_scope`
  - 어떤 승인 주체가 필요한지
- `notes`
  - 구현 저장소의 내부 메모

중요:
- domain은 위 필드명 자체를 강제하지 않는다.
- 아래 항목은 단일 JSON/object/schema를 강제하는 계약이 아니라 semantic checklist다.
- 구현 저장소는 위 의미를 로컬 decision result, middleware result, audit event 등 어디엔가 담거나, 여러 구조를 통해 재구성 가능하게 만들면 된다.
- policy 판단은 단순 boolean 허용 여부가 아니라, explainability 가능한 decision layer여야 한다.

## 최소 판단 흐름
Expansion에서 policy / permission 판단은 아래 흐름을 따른다.

1. `Request` 해석
2. `Task` 또는 action candidate 도출
3. `risk_tier` 평가
4. `permission_context` 확인
5. `policy_precheck` 적용
6. `decision` 산출
7. 아래 중 하나로 분기
   - `allow` → `executing`
   - `confirm` → 사용자 확인 대기 또는 confirm UI 생성
   - `clarify` → `ClarifyRequest` 생성, `clarifying`
   - `deny` → `blocked`
8. 위 판단 결과를 policy decision semantics에 맞춰 기록
9. 핵심 근거를 `ExecutionTrace` 또는 audit log에 남김

## 상태와의 연결
policy / permission 판단은 최소 runtime state와 직접 연결된다.

- `pending`
  - 아직 실행 가능성 판단 전
  - policy output이 아직 확정되지 않았거나 precheck 전 단계다.
- `clarifying`
  - 정책 판단 또는 실행 전에 정보가 더 필요함
  - 이때 output에는 `decision=clarify`에 해당하는 의미와 `clarify_requirements`가 남아야 한다.
- `executing`
  - `allow` 또는 완료된 `confirm` 이후 실제 실행 중
  - 이때 output에는 `decision=allow` 또는 confirm 완료 이력이 연결돼야 한다.
- `blocked`
  - `deny` 또는 미충족 정책 조건 때문에 진행 불가
  - 이때 output에는 `block_conditions`와 `decision_reason`이 함께 남아야 한다.
- `failed`
  - 정책 위반이 아니라 시스템/의존성/실행 오류
  - failed는 deny와 다르게 policy layer 외부 오류라는 점이 구분돼야 한다.
- `completed`
  - 필요한 정책 판단과 실행이 정상 종료됨
  - completed에는 최종 허용 판단과 실행 종료가 함께 복기 가능해야 한다.

## 최소 상태 전이 기준
policy / permission 계층은 적어도 아래 전이 기준을 설명 가능해야 한다.

- `pending → clarifying`
  - 정책 판단 전에 필요한 정보가 비어 있다.
- `pending → blocked`
  - 정보는 충분하지만 정책/권한/승인 조건이 충족되지 않는다.
- `pending → executing`
  - low/medium risk이고 필요한 정책 조건을 충족해 바로 실행 가능하다.
- `pending → confirm_wait`
  - 정보는 충분하지만 high-risk 또는 운영 영향 때문에 사용자 확인이 더 필요하다.
- `confirm_wait → executing`
  - 필요한 확인/승인이 들어왔다.
- `confirm_wait → blocked`
  - 확인 거절 또는 승인 미충족으로 진행 불가가 확정되었다.
- `clarifying → executing`
  - 추가 정보가 들어와 allow 판단이 가능해졌다.
- `clarifying → blocked`
  - 추가 정보가 들어와도 deny 조건이 해소되지 않았다.
- `executing → completed`
  - 정책적으로 허용된 실행이 정상 종료되었다.
- `executing → failed`
  - 실행/의존성 오류로 흐름이 깨졌다.

중요:
- `confirm_wait` 같은 세부 상태명은 구현 저장소가 다르게 둘 수 있다.
- 하지만 confirm이 clarify와 다른 독립 decision layer라는 의미는 domain 차원에서 남아야 한다.

## clarify / confirm / blocked / failed 구분

### `clarify`
다음이 맞으면 `clarify`가 우선이다.
- 정책 판단 전에 필요한 정보가 빠져 있다.
- 대상이나 범위가 모호하다.
- 추가 입력이 들어오면 다시 policy 판단을 계속할 수 있다.

### `confirm`
다음이 맞으면 `confirm`이 우선이다.
- 정보는 충분하다.
- 실행 가능하지만 위험/영향 때문에 명시적 사용자 확인이 더 필요하다.
- 확인이 들어오면 같은 흐름을 계속 실행할 수 있다.

### `blocked`
다음이 맞으면 `blocked`가 우선이다.
- 현재 정책/권한/안전 제약상 실행 불가다.
- 추가 정보가 아니라 승인/권한/운영 조건 변화가 필요하다.
- 지금 진행하면 안 되는 이유가 명확하다.

### `failed`
다음이 맞으면 `failed`로 본다.
- 정책 판단 이후 또는 무관하게 시스템 오류가 발생했다.
- dependency/tool/service 문제로 정상 진행이 깨졌다.
- 실행 불가 이유가 policy가 아니라 error다.

## supervisor와의 연결
supervisor는 policy 판단의 첫 관문을 연다.

최소한 아래가 연결되어야 한다.
- `intent`
- `risk_level` 또는 `risk_tier`
- `missing_slots`
- `policy_precheck`
- `clarify_question` / `clarify_options`

즉 supervisor가 구조적으로 확장될수록 policy 판단도 별도 축으로 정리되어야 한다.

## execution trace와의 연결
`ExecutionTrace`는 최소한 아래를 남길 수 있어야 한다.
- 어떤 `risk_tier`로 평가했는가
- 어떤 `permission_context`를 봤는가
- 어떤 `policy_precheck`가 적용됐는가
- 최종 `decision`이 무엇이었는가
- `decision_reason`이 무엇이었는가
- 왜 `clarifying` / `blocked` / `executing` 으로 갔는가

즉 policy 판단은 trace에서 설명 가능해야 한다.

## 구현 저장소에 대한 기대
각 구현 저장소는 concept-to-code mapping 또는 로컬 설계 문서에서 최소한 아래를 명시한다.

1. policy 판단이 로컬에서 어떤 state / event / API / middleware / approval flow로 표현되는지
2. `allow / deny / confirm / clarify`가 어떤 로컬 이름으로 구현되는지
3. `confirm`과 `clarify`를 UI / API / trace에서 어떻게 구분하는지
4. `blocked`와 `failed`를 어떤 규칙으로 분리하는지

## 비목표
이 문서는 아래를 직접 고정하지 않는다.
- 특정 approval API schema
- 특정 role enum
- 특정 auth provider
- 특정 event payload 구조
- 특정 UI confirm modal 설계

이 문서의 목적은 구현 강제가 아니라, **AMOS에서 정책/권한 판단이 무엇을 설명해야 하는지의 공통 의미 구조**를 고정하는 것이다.
