# Common Semiconductor / AMHS Ontology

## 상위 분류
- `Carrier` — FOUP/FOSB/cassette 계열 운반 객체
- `Lot` — 공정 추적 단위
- `Transport` — OHT/OHS/AGV 등 이동체
- `Guideway` — rail/bridge/overhead track
- `VerticalTransfer` — lift/lifter/elevator-like module
- `Storage` — stocker/buffer/shelf-slot system
- `ToolFrontInterface` — EFEM/load port/aligner/mapper
- `ProcessTool` — 실제 공정 장비
- `Fab` — 제조 구역/공간 컨텍스트

## 반드시 구분할 개념
### FOUP / FOSB / Cassette
- 모두 carrier 계열이지만 운영 문맥이 다르다.
- fab 자동반송/도킹 관점에서는 FOUP 중심으로 본다.

### EFEM / Load Port
- EFEM은 상위 모듈이다.
- Load Port는 그 전면 도킹 인터페이스다.
- 즉 `EFEM ⊃ Load Port` 로 본다.

### Stocker / Slot / Access Port
- stocker는 저장 장치다.
- slot/shelf는 내부 저장 위치다.
- access port/handoff point는 외부 transport와 만나는 인터페이스다.
- slot과 port를 혼동하지 않는다.

### Lift / Vertical Transfer
- lift는 벤더 고유명일 수도 있지만, 여기서는 수직 이송 모듈 추상화로 먼저 본다.

## 관계 규칙
- Carrier is transported by Transport
- Transport runs on Guideway
- Carrier is stored in Stocker Slot
- Carrier docks at Load Port
- Load Port belongs to EFEM or Tool Front
- Vertical Transfer connects levels / handoff points

## 관측 규칙
문서/이미지/GLB/요구사항을 해석할 때는 항상 구분한다.
- `visible`
- `inferred`
- `unknown`
