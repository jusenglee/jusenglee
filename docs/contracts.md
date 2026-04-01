# Contracts & Design Rules

이 문서는 포트폴리오에 포함된 시스템들에서 공통적으로 중요했던 **계약(Contract)** 과 **설계 규칙**을 정리한 문서입니다.

---

## 1. NTIS RAG Retrieval Contract

NTIS AI 챗봇에서는 retrieval 전략이 중간 단계에서 임의로 바뀌지 않도록, 모드와 책임을 명확히 분리했습니다.

### SEARCH
- 목적: 탐색형 검색
- 우선순위: **누락 방지(Recall-first)**
- 특징:
  - 후보를 넓게 수집
  - 이후 재정렬과 결과 검증으로 정제
  - 사람/기관 hard must 필터를 SEARCH에 강제하지 않음

### LOOKUP
- 목적: 정확조회
- 우선순위: **정확성 / 재현성**
- 특징:
  - id, 상세조회, 사람·기관 조회에 적합
  - server-side must filter 사용 가능
  - broad search로 조용히 우회하지 않음

### JOIN
- 목적: 관계형 조회
- 우선순위: **조인키 확보 후 2-hop 검색**
- 특징:
  - 과제 ↔ 성과 같은 관계 질의를 처리
  - join key와 relation이 유효하지 않으면 fail-close
  - empty join을 SEARCH로 조용히 강등하지 않음

### Additional Rules
- Planner는 질의마다 **단 하나의 Strategy**를 출력
- Execution은 Strategy를 **재해석하지 않음**
- Canonical evidence는 raw retrieval payload와 answer text의 중간 산출물로 유지

→ [NTIS AI 챗봇 케이스 스터디](../case-studies/ntis-rag-chatbot.md)

---

## 2. Intent Classification Output Contract

모두의 R&D 프로젝트에서는 자유 형식 사용자 의견을 단순 자연어 응답으로 끝내지 않고, 아래와 같은 **정형 출력 계약**으로 만들었습니다.

### Step 1. Text Refinement
- 문맥 유지
- 맞춤법 및 표현 교정
- 요청된 말투에 맞게 정리

### Step 2. Structured Summary
다음 4개 필드를 기준으로 요약:
- 핵심 제안
- 문제점
- 제안사항
- 기대효과

### Step 3. Topic Classification
예시 토픽 축:
- 연구비·정산 제도 개선
- 평가·선정·성과제도 개선
- 정보 접근·알림 시스템
- 창업기업·특수기관 지원
- 인력 기준 및 연구자 자율성

이렇게 해야 결과가 실무 검토·집계·후속 분류에 바로 연결됩니다.

→ [모두의 R&D 케이스 스터디](../case-studies/everyones-rnd-intent-classification.md)

---

## 3. Serving Contract

운영형 AI 시스템에서는 모델 호출보다 **서빙 계약**이 중요하다고 봅니다.

### Common Rules
- API 입력 / 출력 포맷은 고정
- 모델은 교체 가능하지만 API와 결과 구조는 쉽게 흔들리지 않게 설계
- 로그 / 메트릭 / 재시작 가능성이 기본 전제
- 비정상 응답은 보수적으로 처리(fallback / safe response)

### On-prem Serving Notes
- Triton model repository 구조 유지
- `config.pbtxt`와 실행 스크립트는 배포 기준선 역할
- 모델 업데이트와 재시작 흐름은 문서화 필요
- metrics / logs로 병목 지점을 식별 가능해야 함

---

## 4. Portfolio-Level Principles

### Retrieval before Narration
답변을 잘 쓰기 전에, **무엇을 어떻게 찾았는지**가 먼저 안정되어야 합니다.

### Structured Outputs over Unbounded Text
서비스 기능으로 이어지려면 자유 텍스트보다 **형식이 안정적인 출력**이 더 가치 있습니다.

### Fail-close over Silent Rewrite
잘못된 전략이나 불완전한 입력을 조용히 다른 전략으로 바꾸기보다,  
명시적으로 보수적으로 처리하는 쪽을 선호합니다.

### Evidence Separation
raw data / canonical evidence / answer text를 같은 것으로 취급하지 않습니다.
