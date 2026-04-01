# AI Backend & Serving Engineer Portfolio

정확할 뿐만 아니라 **설명 가능(Explainable)**하고, 고성능 하드웨어의 자원을 한계까지 활용하여 **안정적으로 운영 가능(Operable)**한 AI 시스템을 설계합니다. 단순한 LLM API 연동을 넘어, GPU VRAM 최적화부터 프롬프트 파이프라인의 에러 핸들링까지 프로덕션 레벨의 챌린지를 해결하는 데 집중합니다.

## 💡 Engineering Principles
1. **Fail-close over Silent Rewrite:** 할루시네이션을 막기 위해 LLM이 억지로 답을 지어내게 두지 않습니다. 파이프라인 단계별로 'Contract(계약)'를 설정하여 조건을 만족하지 못하면 명시적으로 실패(Fail-close)하도록 설계합니다.
2. **Resource-Aware Architecture:** 컨텍스트 길이가 길어질수록 기하급수적으로 증가하는 TTFT(첫 토큰 생성 지연)를 막기 위해, 무작정 문서를 주입하지 않고 의도에 따라 `SEARCH / LOOKUP / JOIN` 모드를 분리하여 인프라 병목을 애플리케이션 아키텍처로 풀어냅니다.
3. **Hardware & Serving Optimization:** H200 등 고성능 GPU 환경에서 vLLM, Triton Inference Server를 활용해 모델 양자화(INT4/FP8)에 따른 Throughput과 Latency 트레이드오프를 벤치마킹하고 최적의 서빙 환경을 구축합니다.

## 🚀 Key Highlights & Projects

* **[LLM Serving & Infrastructure Optimization](./infrastructure/llm-serving-optimization.md)** * H200 1대 환경에서 Solar-102B 모델 서빙을 위한 vLLM 양자화(FP8 vs INT4) 벤치마크 및 NCCL/컨테이너 파라미터 튜닝.
  * 프롬프트 길이에 따른 TTFT 병목 프로파일링.
* **[NTIS RAG System: Contract-based Architecture](./projects/ntis-rag-system.md)**
  * 의도 분류(Planner)와 검색 모드 분리를 통해 신뢰할 수 있는 답변을 생성하는 사내 RAG 챗봇 구축.
* **[Scholarly AI Pipeline (400k+ Documents)](./projects/scholarly-ai-summarization.md)**
  * 40만 건 이상의 논문 데이터를 처리하는 배치 파이프라인 구축 및 Result Store(캐싱) 도입을 통한 인프라 비용 절감.
* **[Oracle to Qdrant Migration](./projects/oracle-to-qdrant-pipeline.md)**
  * 레거시 RDBMS(Oracle)에서 Vector DB(Qdrant)로의 데이터 동기화 파이프라인 구축.
