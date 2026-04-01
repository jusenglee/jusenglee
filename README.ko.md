<p align="right">
  <a href="./README.md">English</a> | <strong>한국어</strong>
</p>

# 이승주 포트폴리오 👋

**Full-Stack Based LLM Serving & RAG Systems Engineer**

프론트엔드부터 백엔드, DB까지의 **End-to-End 흐름을 이해하는 풀스택 개발자 출신**으로,  
현재는 온프레미스 GPU 환경 기반의 **LLM 서빙 최적화, 대규모 데이터 파이프라인, RAG 시스템 구축**에 집중하고 있습니다.  
단순한 모델 서빙을 넘어 서비스 전체의 병목 현상(Latency)을 해결하고, 설명 가능하며 운영 안정성이 높은 AI 아키텍처를 지향합니다.

[Blog / Notes](https://www.notion.so/JusengLee-1753a475fe0080a3b62eef1af1688e19?source=copy_link)

---

## 💡 핵심 역량 (Core Strengths)

- **AI 서빙 및 최적화**: Triton Inference Server 및 vLLM을 활용한 온프레미스 LLM 서빙 환경 구축 및 동적 배치(Dynamic Batching)를 통한 Throughput 최적화.
- **검색 및 RAG 아키텍처**: SEARCH / LOOKUP / JOIN 전략 분리를 통한 도메인 특화 하이브리드 검색 및 대규모 지식 기반 RAG 고도화.
- **데이터 파이프라인**: 대량의 트랜잭션 데이터를 정규화하여 Vector DB(Qdrant)로 안정적으로 이관하는 증분 적재 파이프라인 설계 및 GPU OOM 제어.
- **Observability & MLOps**: Prometheus, Grafana를 이용한 서빙 메트릭 모니터링 및 런북(Runbook) 기반의 안정적 장애 대응.

---

## 🛠 기술 스택 (Tech Stack)

- **AI / LLM Serving**: Triton Inference Server, vLLM, GGUF, LangChain, LangGraph
- **Backend / API**: Python, FastAPI, Java, Spring Boot, REST API, SSE(Server-Sent Events)
- **Vector DB / DB**: Qdrant, Oracle, Redis
- **Infra / Ops**: Docker, Linux, Prometheus, Grafana, Git

---

## 📁 대표 프로젝트 (Featured Projects)

### 1. NTIS AI 챗봇 — 검색엔진형 RAG 고도화
국가 R&D 정보 도메인 챗봇을 단순 RAG를 넘어 **검색엔진형 구조**로 고도화한 프로젝트입니다.
- **핵심 키워드**: `Planner–Contract–Executor`, `SEARCH / LOOKUP / JOIN`, 하이브리드 검색, Reranking, SSE 응답.
- **엔지니어링 성과**: 질의 분석과 검색 실행의 파이프라인 분리를 통해 응답의 결정성을 확보하고 Qdrant와 Triton 서빙 연동을 통한 실시간 지식 추론 구현.
- [상세 보기](./case-studies/ntis-rag-chatbot.md)

### 2. 모두의 R&D — 질의의도 분석 및 분류 시스템
자유 형식의 사용자 의견을 대상으로 **교정 → 구조화 요약 → 의도/주제 분류**로 이어지는 정형 출력 파이프라인을 구축했습니다.
- **핵심 키워드**: Gemma 3, GGUF, Triton Inference Server, 정형 출력 보장.
- **엔지니어링 성과**: 모델 출력이 흔들려 시스템 에러가 나는 것을 방지하기 위해 엄격한 Output Contract를 수립하고, Triton을 통해 GPU 자원 활용을 극대화함.
- [상세 보기](./case-studies/everyones-rnd-intent-classification.md)

### 3. Oracle ➡️ Vector DB 적재 파이프라인
Oracle RDB의 업무 데이터를 전처리하고 임베딩하여 Qdrant Vector DB에 적재하는 안정적인 운영형 파이프라인입니다.
- **핵심 키워드**: 임베딩 배치 튜닝, 증분 적재, 체크포인트 관리, GPU OOM 방지.
- **엔지니어링 성과**: 대량 데이터를 처리할 때 GPU 메모리 한계를 고려하여 최적의 배치 사이즈를 도출하고, 실패 시 복구 가능한 체크포인트 시스템을 설계함.
- [상세 보기](./case-studies/oracle-to-qdrant-pipeline.md)

### 4. Scholarly OA AI 요약 플랫폼 연동
대형 논문 플랫폼에 40만 건 규모의 논문 데이터 요약 기능을 제공하기 위한 백엔드 및 서빙 인프라입니다.
- **핵심 키워드**: 대규모 데이터 요약, 결과 저장(Caching), API Rate Limit, 요청 제어.
- **엔지니어링 성과**: 매번 실시간 생성을 고집하지 않고 추론 결과를 캐싱하여 GPU 자원을 절약하고 대규모 트래픽을 안정적으로 수용할 수 있는 아키텍처 설계.
- [상세 보기](./case-studies/scholarly-ai-summarization.md)

---

## 📚 공통 문서

- [Architecture (아키텍처)](./docs/architecture.md)
- [Benchmarks & Public Metrics (성능 지표)](./docs/benchmarks.md)
- [Contracts & Design Rules (설계 규칙)](./docs/contracts.md)
- [Runbook & Operations Notes (운영 런북)](./docs/runbook.md)
