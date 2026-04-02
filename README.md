# AI Backend & Serving Engineer

SI 풀스택(Java/Spring) 개발자로 커리어를 시작하여, 엔터프라이즈 환경에서의 트랜잭션 안정성과 대용량 데이터 처리 역량을 쌓았습니다. 이후 사내 AI 프로젝트를 주도하며 AI 서빙 엔지니어로 전향하였으며, 현재는 기존 웹 서버 아키텍처와 AI 추론 환경(GPU) 간의 통합 및 최적화에 주력하고 있습니다.

단순한 LLM API 연동을 지양하고, 하드웨어(GPU VRAM, 커뮤니케이션 토폴로지) 수준의 이해를 바탕으로 서빙 병목을 해결하며, 레거시 시스템과 최신 AI 파이프라인을 안정적으로 연결하는 프로덕션 레벨의 엔지니어링을 지향합니다.

## Engineering Principles

1. **명시적 실패 선호 (Fail-close over Silent Rewrite)**
   백엔드 시스템의 엄격한 예외 처리 철학을 RAG 및 LLM 생성 파이프라인에 적용합니다. 모델의 할루시네이션이나 잘못된 추론을 방지하기 위해 파이프라인 각 단계에 'Contract(계약)' 검증 계층을 구현하며, 조건 불일치 시 시스템이 조용히 우회하지 않고 명시적으로 에러를 반환하여 데이터 무결성을 보장합니다.

2. **자원 인지형 아키텍처 (Resource-Aware Architecture)**
   LLM 컨텍스트 길이에 비례하여 기하급수적으로 증가하는 TTFT(Time To First Token) 지연 문제를 애플리케이션 아키텍처로 통제합니다. 질문 의도에 따라 검색 모드(SEARCH / LOOKUP / JOIN)를 엄격히 분리하여, 불필요한 컨텍스트 주입을 차단하고 GPU 인프라 병목을 사전에 완화합니다.

3. **하드웨어 및 서빙 최적화 (Hardware & Serving Optimization)**
   제한된 GPU(H200, A40 등) 자원 내에서 최적의 Throughput과 Latency를 확보합니다. vLLM 및 Triton Inference Server를 활용한 모델 양자화(INT4/FP8) 벤치마킹, Dynamic Batching 적용, 그리고 PCIe ACS 통신 병목 등 인프라/네트워크 레벨의 트러블슈팅을 기반으로 서빙 환경을 구축합니다.

## Key Highlights & Projects

* **[Troubleshooting & War Stories](./infrastructure/war-stories-runbook.md)**
  * 프론트엔드 파라미터 조작에 의한 LLM 자원 어뷰징 방어 및 Contract Layer 설계 적용 사례
  * 다중 GPU(Tensor Parallelism) 환경에서 PCIe ACS 설정에 따른 P2P 통신 병목 디버깅 사례
* **[LLM Serving & Infrastructure Optimization](./infrastructure/llm-serving-optimization.md)**
  * H200 단일 노드 환경 내 Solar-102B 모델 서빙을 위한 양자화(FP8 vs INT4) 성능 벤치마크 및 NCCL/컨테이너 튜닝
  * 프롬프트 길이에 따른 TTFT 병목 구간 프로파일링 및 데이터 기반 아키텍처 개선
* **[NTIS RAG System: Contract-based Architecture](./projects/ntis-rag-system.md)**
  * 의도 분류(Planner) 및 검색 모드 분리 알고리즘을 적용한 사내 RAG 기반 질의응답 시스템 설계 및 구축
* **[AccessON scholarly summarization](./projects/scholarly-ai-summarization.md)**
  * 40만 건 이상의 대규모 논문 데이터 처리를 위한 AI 요약 배치 파이프라인 및 Result Store(캐싱) 구축
* **[Oracle to Qdrant Migration](./projects/oracle-to-qdrant-pipeline.md)**
  * 레거시 RDBMS(Oracle)에서 Vector DB(Qdrant)로의 데이터 전처리 및 동기화 파이프라인 구축
