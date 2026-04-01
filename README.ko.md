<p align="right">
  <a href="./README.md">English</a> | <strong>한국어</strong>
</p>

# 이승주 포트폴리오 👋

**LLM Serving & RAG Systems Engineer**

FastAPI, Qdrant, Triton/vLLM, 온프레미스 GPU 환경을 기반으로  
**RAG · 검색 · LLM 서빙 시스템**을 설계하고 구현합니다.  
백엔드 개발 경험을 바탕으로 **AI 엔지니어링(서빙 · 검색 · 운영)** 영역으로 확장해왔으며,  
정확도뿐 아니라 **설명 가능성, 운영 가능성, 서비스 품질**을 함께 고려하는 개발을 지향합니다.

[Blog / Notes](https://www.notion.so/JusengLee-1753a475fe0080a3b62eef1af1688e19?source=copy_link)

---

## 소개

- 도메인 특화 **RAG 검색 시스템**과 **LLM 서빙 인프라**를 설계·구현·운영합니다.
- **검색 품질 / 응답 지연 / 운영 안정성**을 함께 최적화하는 것을 중요하게 생각합니다.
- 관심 분야는 **LLM Serving, Hybrid Retrieval, AI Backend, Data Pipeline, Observability**입니다.

---

## 대표 프로젝트

### 1. NTIS AI 챗봇 — 검색엔진형 RAG 고도화
국가 R&D 정보 도메인 챗봇을 단순 RAG를 넘어 **검색엔진형 구조**로 고도화한 프로젝트입니다.  
핵심 키워드는 **SEARCH / LOOKUP / JOIN**, planner–contract–executor 구조, 하이브리드 검색, 재정렬, SSE 응답, 근거 중심 답변입니다.  
→ [상세 보기](./case-studies/ntis-rag-chatbot.md)

### 2. 모두의 R&D — 질의의도 분석 및 분류 시스템
자유 형식의 사용자 의견을 **교정 → 구조화 요약 → 주제 분류**로 정리하는 LLM 시스템입니다.  
온프레미스 환경에서 **Gemma 3, Triton, 복수 모델 오케스트레이션, API 연계**까지 포함해 설계했습니다.  
→ [상세 보기](./case-studies/everyones-rnd-intent-classification.md)

### 3. Oracle → Embedding → Vector DB Pipeline
Oracle 기반 원천 데이터를 벡터 검색 가능한 데이터로 바꾸는 적재 파이프라인입니다.  
전처리, 체크포인트, 증분 적재, 임베딩 모델 평가, Qdrant 적재 안정화가 핵심이었습니다.  
→ [상세 보기](./case-studies/oracle-to-qdrant-pipeline.md)

### 4. 대형 논문 OA 플랫폼 AI 요약 서비스
약 **40만 건 규모의 논문 데이터**를 대상으로 AI 초록 요약 기능을 구축한 프로젝트입니다.  
GPU 추론 환경, FastAPI 연동, 결과 저장, 요청 제어, 서비스 연계를 포함한 제품화 경험을 담았습니다.  
→ [상세 보기](./case-studies/scholarly-ai-summarization.md)

---

## 공개 저장소

### RAG / Retrieval
- [llm_article_rag_test](https://github.com/jusenglee/llm_article_rag_test)  
  RAG 구조 실험, 검색 품질 검토, 재정렬 아이디어 테스트  
  → [프로젝트 노트](./projects/llm_article_rag_test.md)

### Document Processing
- [PDF_Extraction_Web](https://github.com/jusenglee/PDF_Extraction_Web)  
  PDF 추출 및 웹 기반 문서 처리 흐름 실험  
  → [프로젝트 노트](./projects/PDF_Extraction_Web.md)

### Data Pipeline
- [oracle_to_qdrant__pipe](https://github.com/jusenglee/oracle_to_qdrant__pipe)  
  Oracle 데이터 전처리, 임베딩, Vector DB 적재 파이프라인 구현  
  → [프로젝트 노트](./projects/oracle_to_qdrant__pipe.md)

---

## 공통 문서

- [Architecture](./docs/architecture.md)
- [Benchmarks & Public Metrics](./docs/benchmarks.md)
- [Contracts & Design Rules](./docs/contracts.md)
- [Runbook & Operations Notes](./docs/runbook.md)

---

## 기술 스택

- **Languages**: Python, Java
- **Backend / API**: FastAPI, Spring Boot, REST API, SSE
- **LLM / Retrieval**: Triton Inference Server, vLLM, Qdrant, LangChain, LangGraph
- **Data / Infra**: Oracle, Redis, Docker, Linux
- **Observability**: Prometheus, Grafana, structured logs, quality gates

---

## 내가 중요하게 생각하는 것

- 안정적인 LLM 서빙 인프라
- 도메인 특화 검색 품질
- GPU 기반 추론 성능 최적화
- 운영 환경에서의 모니터링, 관측성, 장애 대응

---

## 저장소 구조

```text
portfolio/
├─ README.md
├─ README.ko.md
├─ assets/
├─ case-studies/
├─ docs/
└─ projects/
```

> 회사 보안 정책상 실제 서비스 코드와 일부 내부 산출물은 공개하지 않았습니다.  
> 대신 이 저장소에서는 **아키텍처, 설계 의사결정, 공개 가능한 근거, 기술적 기여**를 중심으로 정리합니다.
