# Runbook & Operations Notes

이 문서는 포트폴리오에 포함된 시스템들에서 반복적으로 사용한 **운영 체크리스트**를 정리한 문서입니다.  
실제 내부 런북의 세부 절차 전체가 아니라, 공개 가능한 수준에서 핵심 원칙만 요약합니다.

---

## 1. On-prem Triton Serving Checklist

### Before Deploy
- 모델 파일, `config.pbtxt`, 런타임 파라미터 확인
- model repository 구조 정리
- Docker image / dependency 버전 확인
- warm-up 시나리오 준비

### After Restart
- 컨테이너 정상 기동 확인
- 모델 로드 성공 여부 확인
- inference endpoint / health endpoint / metrics endpoint 확인
- 초기 로그에서 model load / warm-up / error stack trace 확인

### During Operation
- 평균 latency / queue delay / error logs 모니터링
- GPU 사용량, 메모리 사용량, batching 상태 확인
- 품질 이슈 발생 시 prompt / input payload / generation params 분리 점검

### Useful Principles
- 재시작이 쉬운 구조가 운영 비용을 낮춘다
- 로그와 metrics가 없으면 품질 문제도 추적하기 어렵다
- 모델 교체보다 먼저 입력/출력 계약이 흔들리지 않는지 확인해야 한다

---

## 2. RAG Service Checklist

### Request Path
1. 입력 수신
2. 의도 / 전략 해석
3. retrieval 실행
4. evidence 정리
5. answer generation
6. stream / response 반환
7. state 저장

### Debug Order
1. planner / strategy가 맞았는가
2. retrieval 결과가 질문 의도와 맞았는가
3. canonical evidence가 잘 정리되었는가
4. answer generation이 evidence를 벗어나지 않았는가
5. fallback / safe response가 과도하게 늘지 않았는가

### Common Failure Modes
- broad query가 detail query처럼 축소됨
- 사람/기관 질의에서 오염 후보가 과도하게 유입됨
- relation query가 join seed 없이 실행됨
- retrieval 결과는 맞는데 answer stage에서 과장되거나 누락됨

---

## 3. Intent Classification Service Checklist

### Input
- 자유 형식 사용자 의견 / 개선 요청
- 입력 길이 및 문체 편차 확인
- 사전 정제 여부 확인

### Output Validation
- 교정 결과가 원문 의미를 훼손하지 않았는가
- 구조화 요약이 4개 섹션으로 유지되는가
- 분류 결과가 정의된 토픽 체계를 벗어나지 않는가

### Serving Validation
- API 응답 포맷 일관성
- 추론 서버 재시작 후 모델 로드 정상 여부
- 장문 입력에서 응답 포맷이 깨지지 않는가

---

## 4. Data Pipeline Checklist

### Ingestion
- 마지막 처리 지점(checkpoint) 보존
- 증분 적재 기준 컬럼 확인
- 중복 적재 / 누락 적재 여부 확인

### Embedding
- 임베딩 모델 버전 기록
- batch size / throughput 모니터링
- 실패 시 재시도 및 부분 복구 가능성 확보

### Vector DB
- payload schema 일관성 확인
- 필터 키와 검색 축이 의도와 맞는지 점검
- re-index / upsert 비용을 고려해 운영 절차 문서화

---

## 5. Release Notes Rule

포트폴리오에 담긴 프로젝트들은 공통적으로 다음 원칙을 따릅니다.

- 코드 변경 시 운영 기준도 함께 갱신
- 성능 수치가 시험 환경인지 운영 환경인지 구분
- 비공개 구현을 설명할 때는 구조와 의사결정 중심으로 정리
