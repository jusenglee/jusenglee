# NTIS AI Chatbot / Search Engine — Redacted Overview Report

> **Scope**
> 본 문서는 NTIS AI 챗봇 프로젝트를 포트폴리오에 첨부하기 위해 작성한 공개용 요약 보고서입니다.
> 실제 운영 환경의 민감정보는 제거하고, 공개 가능한 설계 의사결정·구조·시험 결과만 정리했습니다.

---

## 1. Project Goal

이 프로젝트의 목표는 국가 R&D 정보 도메인에서 단순한 문서 질의응답형 RAG를 넘어,
**정확조회 / 탐색형 검색 / 관계형 질의**를 함께 처리할 수 있는 **검색엔진형 AI 응답 시스템**을 구축하는 것이었습니다.

초기 RAG는 “관련 문서를 찾아 답변을 생성하는 구조”에 가까웠지만,
서비스 요구사항이 구체화되면서 아래 조건을 동시에 만족해야 했습니다.

- **정확도**: 식별자성 질의, 상세조회, 사람/기관 기반 질의에서 오답을 줄일 것
- **처리성**: 짧고 모호한 질문, 목록형 질의, 관계형 질의까지 일관되게 처리할 것
- **운영성**: 왜 이 전략이 선택되었는지 추적 가능한 구조를 유지할 것

---

## 2. What Made the Problem Hard

NTIS 도메인에서는 동일한 “과제”를 보더라도
- 개별 시행 인스턴스,
- 동일 과제의 연도별 그룹,
- 관련 성과,
- 사람/기관 조건,
- nested 참여 정보
등이 뒤섞여 있어 일반 문서 검색보다 구조가 훨씬 복잡합니다.

이 때문에 모든 질의를 하나의 retrieval 규칙으로 처리하면,
탐색형 질의에서는 후보 누락이 발생하고,
정확조회 질의에서는 오염된 후보가 상위로 올라오는 문제가 생기기 쉬웠습니다.

---

## 3. Key Design Decisions

### 3.1. SEARCH / LOOKUP / JOIN 분리

질의를 다음 세 가지 모드로 분리했습니다.

- **SEARCH**: 탐색형 검색
  - 누락 방지를 우선
  - 하이브리드 검색으로 후보를 넓게 수집한 뒤 재정렬
- **LOOKUP**: 정확조회
  - 식별자, 사람/기관, 상세 조회를 정확히 찾는 모드
  - server-side filter를 적극 활용
- **JOIN**: 관계형 조회
  - 예: 과제 ↔ 성과
  - 2-hop 방식으로 연결 키를 확보한 뒤 후속 검색 수행

### 3.2. Planner → Contract → Executor

플래너는 질의마다 하나의 전략(Strategy)을 결정하고,
검증 계층은 그 전략이 규칙에 맞는지 확인하며,
실행 계층은 그 전략을 재해석하지 않고 그대로 수행하도록 분리했습니다.

이 구조를 통해
- 플래너 판단 오류인지,
- 검색 실행 문제인지,
- 재정렬/컨텍스트 구성 문제인지
운영 단위에서 구분하기 쉬워졌습니다.

### 3.3. Query Analysis 파이프라인 재구성

초기에는 정규식 기반 질의분석을 사용했으나,
엣지 케이스에서 치명적인 오판이 발생했습니다.
이후 LLM 플래너 기반 구조를 적용했지만,
질의분석과 답변 생성이 한 흐름에 섞이며 응답이 불안정해지는 문제가 있었습니다.

최종적으로는
**Query Analysis → Retrieval Execution → Answer Generation**
으로 파이프라인을 분리하고 체이닝으로 연결해,
분석의 결정성과 응답 품질을 함께 확보했습니다.

---

## 4. Publicly Shareable Evidence

### 4.1. Retrieval + Serving Integration Test

공개 가능한 시험 보고서 기준으로,
Qdrant 기반 검색과 Triton 기반 LLM을 결합한 초기 통합 검증 환경에서 다음을 확인했습니다.

- Qdrant retrieval latency: **평균 0.4초**
- Triton response latency: **평균 4.4초**
- End-to-end latency: **약 5.3초 / 질의**
- 키워드 확장, 재정렬/중복 제거, 토큰 클램핑 정상 동작

> 위 수치는 운영 본선이 아니라 **초기 통합 시험 환경 기준**입니다.

### 4.2. Embedding Model Comparison

동일 데이터셋 / 동일 GPU 환경에서 임베딩 모델을 비교한 결과,
`multilingual-e5-large`가 `bge-m3` 대비 더 짧은 전체 처리 시간과 높은 처리량을 보였습니다.

- multilingual-e5-large: **24.4분 / 76.8 docs/s**
- bge-m3: **28.9분 / 63.0 docs/s**

이 비교를 바탕으로 속도, 안정성, 다국어 대응 관점에서 기본 임베딩 모델 선택 근거를 마련했습니다.

---

## 5. My Contribution

이 프로젝트에서 저는 다음 영역을 중심으로 설계·구현·운영 고도화를 진행했습니다.

- 질의 유형 분석 및 SEARCH / LOOKUP / JOIN 전략 설계
- Planner–Contract–Executor 구조 정립
- 하이브리드 retrieval 및 rerank / filter 정책 고도화
- API / streaming response 구성
- 컨텍스트 구성 및 근거 표시 구조 정리
- 테스트 환경과 운영 환경 사이의 관측성(logs / metrics / quality gate) 정비

---

## 6. Attachment Guide

- 전체 구조 요약: [NTIS Overall Architecture](../assets/ntis/ntis-overall-architecture.svg)
- 데이터 적재 / 검색 / 서빙 전체 흐름: [Ingest → Retrieval → Serving](../assets/ntis/ntis-ingest-retrieval-serving.svg)
- 검색 전략 분리: [SEARCH / LOOKUP / JOIN Modes](../assets/ntis/ntis-retrieval-modes.svg)
