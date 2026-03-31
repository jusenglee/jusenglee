<p align="right">
  <strong>한국어</strong> | <a href="./README.md">English</a>
</p>

# 안녕하세요, 이승주입니다 👋

**LLM Serving & RAG Systems Engineer**  
FastAPI, Qdrant, Triton/vLLM 기반의 **RAG·검색·LLM 서빙 시스템**을 설계하고 구현합니다.  
백엔드 개발 경험을 바탕으로 **AI 엔지니어링(서빙·검색·운영)** 영역으로 확장해온 개발자입니다.

[Blog / Notes](https://www.notion.so/JusengLee-1753a475fe0080a3b62eef1af1688e19?source=copy_link)

---

## 소개

- 도메인 특화 **RAG 검색 시스템**과 **LLM 서빙 인프라**를 설계·구현·운영합니다.
- **검색 품질 / 응답 지연(TTFT 포함) / 운영 안정성**을 “서비스 관점”에서 함께 최적화하는 것을 중요하게 생각합니다.
- 관심 분야는 **LLM Serving**, **Hybrid Retrieval**, **AI Backend**, **Data Pipeline**, **Observability**입니다.

---

## 주요 분야

### LLM 서빙 및 검색
- Triton Inference Server / vLLM 기반 LLM 서빙(온프레미스 GPU 환경 포함)
- Qdrant 기반 Hybrid Retrieval (Dense + Sparse / BM25)
- RAG 파이프라인 설계 및 재정렬(Reranking / RRF)
- 대규모 컨텍스트 처리 및 추론 성능 최적화(TTFT/처리량/메모리 중심)

### AI 백엔드 및 데이터 파이프라인
- FastAPI 기반 AI 백엔드 및 REST API 개발
- Streaming API / SSE 응답 처리
- Oracle → Embedding → Vector DB 적재 파이프라인 설계(증분/체크포인트/병렬화)
- Redis 기반 상태 관리 및 서비스 연계

### 인프라 및 관측성
- Docker / Linux 기반 서비스 배포 및 운영
- Prometheus / Grafana 기반 모니터링
- GPU 메트릭 기반 병목 분석 및 성능 점검
- 운영 로그 기반 장애 추적 및 안정화

---

## 기술 스택

- **Languages**: Python, Java
- **Backend / API**: FastAPI, Spring Boot, REST API, SSE
- **LLM / Retrieval**: Triton Inference Server, vLLM, Qdrant, LangChain, LangGraph
- **Data / Infra**: Oracle, Redis, Docker, Linux
- **Observability**: Prometheus, Grafana

---

## 주요 공개 저장소

> 일부 주요 프로젝트는 회사 보안 정책상 소스 코드를 공개할 수 없습니다.  
> 대신 공개 가능한 실험 저장소와 함께, 실제 운영 환경에서 수행한 프로젝트의 아키텍처·역할·기술적 기여를 케이스 스터디로 정리했습니다.

### RAG / Retrieval
- [llm_article_rag_test](https://github.com/jusenglee/llm_article_rag_test)  
  RAG 구조 실험, 검색 품질 검토, 재정렬 아이디어 테스트

### Document Processing
- [PDF_Extraction_Web](https://github.com/jusenglee/PDF_Extraction_Web)  
  PDF 추출 및 처리 흐름, 웹 기반 문서 처리 실험

### Data Pipeline
- [oracle_to_qdrant__pipe](https://github.com/jusenglee/oracle_to_qdrant__pipe)  
  Oracle 데이터 전처리, 임베딩, Vector DB 적재 파이프라인 구현

---

## 주요 프로젝트(비공개 포함)

### 국가 R&D 정보 도메인 RAG 검색 및 응답 시스템 *(비공개 / 회사 프로젝트)*
- 국가 연구개발 정보 도메인에 특화된 RAG 검색 및 질의응답 시스템 설계·구현
- **SEARCH / LOOKUP / JOIN** 전략 분리 기반의 검색 아키텍처 설계
- Qdrant 기반 Hybrid Retrieval 및 재정렬 로직 구현
- FastAPI, LangGraph, Triton/vLLM, Redis 연계한 운영형 AI 백엔드 구성
- 검색 품질, 응답 지연, 운영 안정성을 중심으로 성능 개선 및 구조 고도화 수행

### 모두의 R&D 질의의도 분석 및 분류 시스템 *(일부 비공개)*
- 사용자 게시글/질의 기반 **LLM 의도 분석·분류 파이프라인** 설계·구현
- 온프레미스 **H100 2GPU** 환경에서 **Gemma 3 포함 복수 모델 추론 오케스트레이션/서빙** 설계·구현
- 질의 입력 → 분석 → 결과 반환까지 **E2E 흐름을 API로 구성**하여 서비스 연계
- 운영 안정성을 고려해 서버 구조를 정비하고, **프롬프트·파이프라인·서빙 구조를 반복 개선**하여 응답 일관성과 안정성 강화
  
### Oracle → Embedding → Vector DB 적재 파이프라인 *(비공개 / 회사 프로젝트)*
- Oracle 데이터 소스 기반 전처리, 임베딩, Vector DB 적재 파이프라인 설계 및 구현
- 증분 적재, 체크포인트 관리, 임베딩 병렬화, 적재 안정성 개선 작업 수행
- 대용량 문서 및 메타데이터 인덱싱 자동화를 통해 검색 시스템 운영 기반 구축

### 학술 데이터 플랫폼 AI 요약 기능 구축 *(비공개 / 회사 프로젝트)*
- 대규모 학술 콘텐츠 플랫폼에 AI 초록 요약 기능 설계 및 적용
- LLM API 연동, 결과 저장, 요청 제어, 운영 로그 처리 등 서비스 기능 구현
- 기존 백엔드 서비스에 AI 기능을 통합하며 제품화 관점의 운영 경험 확보

---

## 관심사

- 안정적인 LLM 서빙 인프라 구축(온프레미스/컨테이너 운영 포함)
- 도메인 특화 검색 품질 개선(Hybrid Retrieval + Rerank)
- GPU 기반 추론 시스템의 성능 측정 및 최적화(TTFT/처리량/메모리)
- 운영 환경에서의 모니터링, 관측성, 장애 대응

---

<p align="center">
  방문해주셔서 감사합니다.
</p>
