# Troubleshooting & War Stories

프로덕션 환경에서 대규모 LLM을 서빙하며 마주했던 인프라 병목, 보안 이슈, 하드웨어 장애를 디버깅하고 해결한 실제 경험(War Stories)입니다. 이 경험들은 현재 저의 **"보수적인 아키텍처 설계"**와 **"하드웨어 최적화"** 철학의 기반이 되었습니다.

---

## 1. [Security] 프론트엔드 파라미터 노출로 인한 LLM 자원 어뷰징
**"Contract(계약) 기반의 아키텍처가 탄생하게 된 계기"**

* **Situation:** 초기 시스템 구축 당시, Java 서버에서 Triton Inference Server로 직접 요청을 보내는 구조를 설계했습니다. 유연성을 높이기 위해 프론트엔드(Ajax)에서 프롬프트와 일부 추론 파라미터를 동적으로 전송할 수 있도록 열어두었습니다.
* **Problem:** 서비스 오픈 직후, 악의적인 사용자들이 네트워크 페이로드를 조작하여 본인들이 원하는 임의의 프롬프트를 주입했고, 사내 LLM 자원(GPU)을 개인적인 용도로 마구 사용하는 어뷰징 사태가 발생했습니다.
* **Resolution & Lesson Learned:** 단순히 API 게이트웨이를 막는 것을 넘어, **백엔드 아키텍처를 전면 개편**했습니다. 프론트엔드의 입력을 절대 신뢰하지 않고, 백엔드 단에 엄격한 **'Contract Layer(검증 계층)'**를 두어 시스템이 정의한 의도(Intent)와 페이로드가 일치하지 않으면 조용히 우회하지 않고 즉각적으로 에러를 뱉도록(Fail-close) 설계했습니다.

---

## 2. [Performance] Llama 3 서빙 타임아웃과 Triton/TensorRT 도입
**"Ollama의 한계를 넘어, Dynamic Batching과 양자화로 Queue 병목 타파"**

* **Situation:** A40 GPU 환경에서 Ollama를 활용해 Llama 3 모델을 서빙했으나, 프로덕션 트래픽을 감당하기에는 추론 속도(Throughput)가 현저히 부족했습니다.
* **Problem:** 속도를 높이기 위해 Triton Inference Server로 이관했으나, 요청이 몰리는 피크 시간대에 Triton 큐(Queue)에 요청이 지속적으로 쌓이면서 처리 속도 저하 및 **대규모 Timeout**이 발생했습니다.
* **Resolution:**
  1. **TensorRT-LLM 엔진 도입:** 단순 모델 로딩을 넘어, GPU 가속을 극대화하기 위해 모델을 Tensor-Engine으로 변환하여 서빙했습니다. (Triton 사용의 시작)
  2. **Dynamic Batching & Quantization:** 다이나믹 배칭(Dynamic Batching) 기능을 활성화하여 큐에 대기 중인 요청들을 하나의 배치로 묶어 처리량을 극대화했습니다. 동시에 모델을 **양자화(Quantization)**하여 VRAM 여유 공간을 확보하고, 동시 처리 가능한 최대 토큰 수(Batch Capacity)를 대폭 늘려 Timeout 이슈를 완벽히 해소했습니다.

---

## 3. [Hardware] 다중 GPU(TP) 환경에서의 PCIe ACS P2P 통신 병목
**"하드웨어/네트워크 레벨의 디버깅으로 Tensor Parallelism 성공"**

* **Situation:** 단일 GPU(A40 1장)로는 튜닝 이후에도 VRAM과 연산량 감당이 어려워, A40 2장을 묶어 Tensor Parallelism(TP, 텐서 병렬화)으로 모델을 올리려 시도했습니다.
* **Problem:** 모델 분산 로딩 중 지속적인 통신 에러가 발생하거나, 예상과 달리 단일 GPU보다도 추론 성능이 심각하게 저하되는 기이한 현상이 발생했습니다.
* **Resolution:**
  소프트웨어 설정 문제가 아님을 직감하고 NCCL(NVIDIA Collective Communications Library) 로그와 하드웨어 통신망을 딥다이브(Deep Dive)했습니다.
  원인은 서버 메인보드의 **PCIe 보안 기능인 ACS(Access Control Services)가 활성화**되어 있었기 때문이었습니다. 이로 인해 GPU 간의 P2P(Peer-to-Peer) 직접 통신이 차단되고, 모든 텐서 데이터가 CPU(PCIe Root Complex)를 강제로 경유하면서 치명적인 병목(Bottleneck)이 발생한 것이었습니다.
* **Outcome:** BIOS 및 OS(Linux) 레벨에서 ACS 설정을 튜닝하여 GPU 간 P2P 통신을 활성화했고, 정상적으로 TP를 구성하여 거대 모델 서빙 환경을 구축해 냈습니다.
