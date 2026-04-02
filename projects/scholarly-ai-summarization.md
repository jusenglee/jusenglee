# AccessON OA AI Summarization Pipeline

> **Confidentiality Note** > 본 문서는 회사 보안 정책을 준수하는 범위 내에서, 대규모 논문 데이터 대상의 AI 파이프라인 아키텍처 설계 및 구현 범위를 정리한 포트폴리오용 케이스 스터디입니다.

---

## 1. Background & Challenge
약 40만 건 규모의 대형 논문 오픈 액세스(OA) 플랫폼에 'AI 초록 요약 기능'을 프로덕션 수준으로 도입하는 프로젝트입니다. 

논문 데이터의 방대한 규모를 고려할 때, 요약 기능은 단순한 부가 기능(Nice-to-have)을 넘어 사용자의 정보 탐색 비용(Search Cost)을 실질적으로 절감하는 핵심 기능이어야 했습니다. 따라서 단순한 모델 PoC(Proof of Concept)를 넘어, **대규모 트래픽을 감당할 수 있는 GPU 추론 환경, 요청 제어, 결과 캐싱(Caching)을 포함한 통합 서빙 파이프라인**을 구축하는 것이 핵심 과제였습니다.

## 2. Architectural Decisions

**① Serving Layer의 구조적 분리**
기존 거대 플랫폼의 모놀리식 코드베이스에 AI 추론 로직을 강결합하지 않고, FastAPI 기반의 독립적인 추론 API 마이크로서비스로 분리했습니다. 이를 통해 플랫폼 본연의 안정성을 보호하고 AI 서빙 환경의 독립적인 스케일링 및 운영 편의성을 확보했습니다.

**② 결과 캐싱(Result Store) 및 요청 제어(Rate Limiting) 도입**
모든 요약 요청에 대해 실시간으로 모델 추론을 수행할 경우, 막대한 GPU 컴퓨팅 리소스 낭비와 응답 지연이 발생합니다. 이를 방지하기 위해 추론 결과를 저장하고 재사용하는 'Result Store' 레이어를 설계했습니다. 또한, 악의적이거나 비정상적으로 집중되는 트래픽으로부터 GPU 자원을 보호하기 위한 요청 제어(Rate Control) 메커니즘을 적용했습니다.

**③ End-to-End 파이프라인 통합**
로컬 환경의 실험을 넘어, GPU 인프라 프로비저닝 → LLM 서빙 API 개발 → 플랫폼 인터페이스 연동 → 운영 로그 수집 및 모니터링에 이르는 전체 데이터 파이프라인을 End-to-End로 설계하고 구현했습니다.

## 3. Architecture Flow

```mermaid
flowchart LR
    P["Scholarly OA Platform<br/>(논문 플랫폼)"] --> API["FastAPI API Layer<br/>(요청 수신 및 제어)"]
    API --> SERVE["GPU Inference<br/>(LLM 요약 생성)"]
    SERVE --> SAVE["Result Store<br/>(추론 결과 캐싱 및 영속화)"]
    SAVE --> P
    API --> LOG["Operational Logs<br/>(처리량 모니터링 / 로깅)"]
