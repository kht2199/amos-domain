# Request Lifecycle

## 목적
이 문서는 request-side runtime vocabulary와 lifecycle 상태 canonical을 정리하는 active engine 문서다.

중요:
- 이 문서는 ontology의 객체 관계를 새로 만들지 않는다.
- 자연어 요청의 해석 축은 `../ontology/request-interpretation.md`를 따른다.
- supervisor / trace / policy의 상세 확장 구조는 `supervisor-structure.md`, `execution-trace-structure.md`, `policy-permission-structure.md`를 따른다.

## 위치
- 도메인 객체/상태 기준: `../ontology/foundation.md`
- 자연어 요청 해석 축: `../ontology/request-interpretation.md`
- request-side runtime vocabulary와 상태 canonical: 이 문서

## 최소 용어
- `Request` — 사용자 질의 또는 실행 요청을 표현하는 최상위 작업 단위
- `ClarifyRequest` — 정보 부족/모호성 때문에 추가 입력을 요청하는 구조

참고:
- `Task`, `ToolCall`, `ExecutionTrace`, `Policy`는 agent 실행 구조와 더 직접 연결되므로 전용 engine 문서를 우선 따른다.

## 최소 상태 집합
- `pending`
- `clarifying`
- `executing`
- `completed`
- `blocked`
- `failed`

## 상태 의미
- `pending` — 요청이 수용되었지만 본격 실행 전인 상태
- `clarifying` — 추가 정보가 필요해 응답을 기다리는 상태
- `executing` — 내부 판단 또는 실제 실행이 진행 중인 상태
- `completed` — 요청 흐름이 정상 종료된 상태
- `blocked` — 정책/권한/외부 조건 때문에 진행 불가한 상태
- `failed` — 실행을 시도했지만 오류로 정상 종료하지 못한 상태

## 구분 원칙
- `clarifying`은 사용자 추가 정보가 오면 같은 요청 흐름을 이어갈 수 있다.
- `blocked`는 정보 부족이 아니라 정책/권한/운영 조건에 막힌 상태다.
- `failed`는 실행 시도 결과의 실패다.

## 경계
- 이 문서는 request-side runtime vocabulary와 상태 canonical만 소유한다.
- intent / target entity / target scope / missing-slot 해석 축은 `../ontology/request-interpretation.md`가 소유한다.
- supervisor 판단 결과 shape, trace payload, policy decision 구조는 각 전용 engine 문서가 소유한다.
- 특정 저장소의 state enum, event name, API shape는 각 구현 저장소가 소유한다.
