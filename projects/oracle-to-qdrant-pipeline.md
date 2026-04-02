# Oracle to Qdrant Migration Pipeline

> **Confidentiality Note**
> 본 문서는 회사 보안 정책을 준수하는 범위 내에서, 대규모 데이터 마이그레이션 파이프라인의 아키텍처 설계 및 임베딩 벤치마크 결과만을 정리한 포트폴리오용 케이스 스터디입니다.

---

## 1. Background & Challenge
RAG 및 추천 시스템의 최종 품질은 서빙 모델의 성능 이전에 검색 대상이 되는 데이터의 정합성과 구조화 수준에서 결정됩니다. 본 프로젝트는 레거시 관계형 데이터베이스(Oracle)에 적재된 방대한 원천 데이터를 Vector DB(Qdrant)로 동기화하기 위한 **'전처리 → 임베딩 → 적재' 엔드투엔드(End-to-End) 파이프라인**을 설계하고 구현한 사례입니다. 

단순한 1회성 데이터 이관(Migration)이 아닌, **지속적이고 안정적으로 운영 가능한 데이터 동기화 시스템**을 구축하는 것이 핵심 과제였습니다.

### 1.1. 데이터 스키마 불일치 (Relational vs. Vector)
Oracle 기반의 기존 시스템은 트랜잭션(OLTP) 및 업무 처리 중심으로 설계되어 있어, 하이브리드 검색이나 벡터 검색에 즉시 활용하기 부적합했습니다. 이를 해결하기 위해 검색에 최적화된 필드 추출, 페이로드(Payload) 스키마 정규화, 동일 문서의 중복 적재 방지 로직이 필수적이었습니다.

### 1.2. 운영형 파이프라인의 안정성 확보
프로덕션 환경에서는 네트워크 단절이나 연산 지연 등 예기치 못한 장애가 발생할 수 있습니다. 시스템 장애 시 파이프라인을 처음부터 다시 실행하는 비효율을 막기 위해 **증분 적재(Incremental Ingestion)**, **체크포인트(Checkpoint) 상태 관리**, **실패 구간 복구(Retry)** 메커니즘을 시스템 레벨에서 보장해야 했습니다.

## 2. Architectural Decisions

**① 파이프라인 단계의 철저한 분리 (Decoupling)**
파이프라인을 `1) Source Fetch (Oracle)`, `2) Preprocess & Normalize`, `3) Embedding`, `4) Upsert (Qdrant)`, `5) Checkpoint & Logging`의 5단계로 엄격히 분리했습니다. 이를 통해 특정 단계(예: 임베딩 서버 지연)에서 장애가 발생하더라도 문제 원인을 즉각 격리하고 실패 지점부터 부분 재실행이 가능하도록 구성했습니다.

**② 증분 적재 및 체크포인트 기반 아키텍처**
전체 데이터를 매번 갱신하는 Full-Refresh 방식 대신, 마지막 처리 지점을 기록하고 변경된 데이터만 후속 처리하는 체크포인트 기반의 증분 적재 구조를 도입하여 RDBMS 부하를 최소화하고 처리 효율을 극대화했습니다.

**③ 데이터 기반의 임베딩 모델 선정**
운영 환경에서의 임베딩 모델은 단순한 검색 품질(Accuracy)뿐만 아니라 대용량 데이터 처리 속도(Throughput)와 인프라 유지보수성을 동시에 충족해야 합니다. 이를 검증하기 위해 동일 하드웨어 환경에서 복수의 임베딩 모델을 벤치마크했습니다.

## 3. Architecture Flow

```mermaid
flowchart LR
    O["Oracle Source<br/>(원천 데이터)"] --> P["Preprocess & Normalize<br/>(정제 / 필드 매핑 / 스키마 정규화)"]
    P --> E["Embedding Layer<br/>(임베딩 생성 배치)"]
    E --> Q["Qdrant Vector DB<br/>(데이터 Upsert / 페이로드 저장)"]
    Q --> R["RAG / Retrieval Systems<br/>(검색 / 질의응답 연동)"]

    P --> C["Checkpoint & Retry<br/>(증분 적재 / 실패 복구 / 상태 로깅)"]
    E --> C
    Q --> C
