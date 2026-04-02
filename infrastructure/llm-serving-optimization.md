# LLM Serving & Infrastructure Optimization

고성능 GPU(H200) 자원을 효율적으로 활용하고 서비스 응답 지연을 최소화하기 위해 수행한 vLLM 서빙 파라미터 튜닝 및 인프라 병목 구간 프로파일링 기록입니다.

## 1. Solar-102B 양자화(Quantization) 서빙 벤치마크
H200(141GB) 1대 노드 환경에서 거대 언어 모델(Solar-102B)을 안정적으로 서빙하기 위해, **INT4와 FP8 양자화 모델의 성능(Throughput vs Latency) 트레이드오프를 벤치마크**했습니다.

### 테스트 환경 및 튜닝 파라미터
분산 추론 및 고부하(High-Concurrency) 상황에서의 병목을 완화하기 위해 Docker 및 vLLM 파라미터를 최적화했습니다.
* **Shared Memory & IPC**: 컨테이너 간 통신 오버헤드를 줄이기 위해 `--ipc=host --shm-size=64g` 할당
* **NCCL Optimization**: `NCCL_CUMEM_ENABLE=1`, `NCCL_ASYNC_ERROR_HANDLING=1` 등을 적용하여 GPU 간 통신 안정성 확보
* **vLLM Settings**: `max_tokens=8192`, `n_per_worker=80` 설정으로 Concurrent Requests(640개) 상황 대비

### 벤치마크 결과 요약 (Concurrent Requests = 640)
| Model Version | VRAM Usage | Median TTFT (ms) | p50 Latency (ms) | Token/s (p50) | ITL p95 (ms) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Solar-100B (FP8)** | 131.8 GB | 13,200 ms | 17,838 ms | 61.41 | 17.06 ms |
| **Solar-100B (INT4)** | 131.8 GB | 14,699 ms | 17,250 ms | 42.01 | 11.78 ms |

* **Insight**: FP8 환경이 INT4 대비 전체적인 토큰 생성 속도(Tok/s)는 우수했으나, 토큰 간 지연 시간(Inter-Token Latency, ITL) 측면에서는 INT4가 더 일관된 안정성을 보여주었습니다. 서비스의 핵심 요구사항(빠른 최초 응답 vs 안정적인 스트리밍 출력)에 맞춰 최적의 모델 포맷을 선택할 수 있는 데이터 기반을 마련했습니다.

---

## 2. 프롬프트 길이에 따른 RAG 파이프라인 병목 프로파일링
'검색된 다량의 문서를 컨텍스트로 일괄 주입할 경우의 효용성'을 검증하기 위해, RAG 시스템 내 프롬프트 길이가 응답 지연에 미치는 영향을 직접 프로파일링했습니다.

### Context Length vs TTFT (Gemma3-27b 기준)
| Prompt Token Range | RAG Search (s) | Model TTFT (ms) | Non-Gen Overhead |
| :--- | :--- | :--- | :--- |
| **3k – 5k** | 0.33s | 2,211 ms | 337 ms |
| **5k – 7k** | 0.21s | 6,312 ms | 1,112 ms |
| **7k – 9k** | 0.13s | 7,463 ms | 622 ms |
| **9k – 11k** | 0.25s | 5,387 ms | 1,200 ms |

* **Architectural Decision (Why SEARCH/LOOKUP/JOIN?):**
  데이터 프로파일링 결과, 컨텍스트 길이가 5k를 초과하는 구간부터 **TTFT(첫 토큰 지연 시간)가 약 2.8배(2,211ms -> 6,312ms) 급증**하는 하드웨어적 병목을 확인했습니다. 
  이러한 **인프라 레벨의 한계를 소프트웨어 아키텍처로 통제**하기 위해, 불필요한 문서를 일괄 주입하는 대신 [NTIS RAG System](../projects/ntis-rag-system.md)에서 설계한 바와 같이 질문 의도를 분석하여 필요한 문서만 선별적으로 추출하는 `SEARCH / LOOKUP / JOIN` 검색 모드 분리 아키텍처를 도입했습니다.
