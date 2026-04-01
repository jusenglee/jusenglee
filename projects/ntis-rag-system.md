# NTIS RAG System: Contract-based Architecture

검색 오염을 방지하고 신뢰할 수 있는 답변만을 생성하기 위해, 엄격한 'Contract(계약)' 기반으로 동작하는 사내 RAG 시스템을 설계 및 구축했습니다.

## 🎯 The Challenge: 프롬프트 길이 병목과 할루시네이션
[인프라 벤치마크](../infrastructure/llm-serving-optimization.md)에서 확인했듯, 검색된 모든 문서를 LLM에 주입하면 TTFT 지연이 발생하고 할루시네이션 리스크가 커집니다. 이를 해결하기 위해 **"설명 가능성(Explainability)"**과 **"Fail-close(안전한 실패)"**를 핵심 원칙으로 아키텍처를 설계했습니다.

## 🏗️ System Architecture: Planner-Contract-Executor Pattern
LLM을 단순한 텍스트 생성기가 아닌, 전체 워크플로우를 통제하는 **Router**로 활용합니다.

1. **Intent Planner:** 사용자의 질문을 분석하여 `SEARCH`, `LOOKUP`, `JOIN` 세 가지 모드 중 하나를 결정합니다.
2. **Contract Layer (검증 계층):** Planner가 생성한 파라미터가 유효한지 검사합니다.
   * *예:* "특정 연구자의 과제를 찾아줘"라는 질문에 이름(Name) 엔티티가 누락되었다면, 시스템은 조용히 잘못된 검색을 수행하는 대신(Silent Rewrite) 명시적으로 에러를 반환합니다(Fail-close).
3. **Execution & Generation:** 철저히 검증된 데이터(Payload)만을 Context로 구성하여 답변을 생성합니다.

## 🔍 Retrieval Strategy: 모드 분리를 통한 리소스 최적화
모든 질문에 동일한 Vector Search를 수행하지 않습니다.
* **SEARCH (Recall 지향):** "해안 관련 과제 목록" 등 폭넓은 주제 검색 시 Vector Similarity 활용.
* **LOOKUP (Precision 지향):** "신동구씨가 참여한 과제" 등 명확한 조건이 있을 때 Qdrant의 Payload 필터링(Must, Should)을 활용하여 정확도 100% 보장.
* **JOIN (Relational 지향):** 복합적인 관계망이 필요할 때 사용.

*(여기에 기존에 있던 `ntis-retrieval-modes.png` 이미지를 삽입하세요)*

## 💡 Outcomes
* **정형 출력(Structured Output) 엣지 연동:** LLM의 출력을 단순 자유 텍스트가 아닌 JSON 포맷의 메타데이터로 파싱하여 프론트엔드/서비스 엣지(Edge)에서 즉시 활용 가능하도록 구현.
* **응답 지연 최적화:** 무의미한 문서의 컨텍스트 주입을 차단하여, 토큰 낭비를 막고 서빙 응답 지연(TTFT)을 효과적으로 통제함.
