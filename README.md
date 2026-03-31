<p align="right">
  <a href="./README.ko.md">한국어</a> | <strong>English</strong>
</p>

# Hi, I'm Seungju Lee 👋

**LLM Serving & RAG Systems Engineer**  
I design and build **RAG, search, and LLM serving systems** using FastAPI, Qdrant, and Triton/vLLM.  
With a strong backend foundation, I’ve expanded into **AI engineering (serving · retrieval · operations)** in production environments.

[Blog](https://www.notion.so/JusengLee-1753a475fe0080a3b62eef1af1688e19?source=copy_link)

---

## About

- I design, implement, and operate **domain-specific RAG search systems** and **LLM serving infrastructure**.
- I focus on optimizing **retrieval quality**, **response latency (including TTFT)**, and **operational reliability** from a service perspective.
- My interests include **LLM Serving**, **Hybrid Retrieval**, **AI Backend**, **Data Pipelines**, and **Observability**.

---

## Focus Areas

### LLM Serving & Retrieval
- LLM serving with **Triton Inference Server / vLLM** (including on-prem GPU environments)
- **Qdrant-based hybrid retrieval** (Dense + Sparse / BM25)
- RAG pipeline design and re-ranking (**Reranking / RRF**)
- Large-context handling and inference performance optimization (**TTFT / throughput / memory**)

### AI Backend & Data Pipelines
- AI backend and REST API development with **FastAPI**
- Streaming APIs / **SSE** response handling
- Oracle → Embedding → Vector DB ingestion pipelines (incremental load / checkpointing / parallel embedding)
- Service integration with **Redis-based state management**

### Infrastructure & Observability
- Service deployment and operations with **Docker / Linux**
- Monitoring with **Prometheus / Grafana**
- Bottleneck analysis using GPU metrics and performance diagnostics
- Incident investigation and stabilization based on operational logs

---

## Tech Stack

- **Languages**: Python, Java
- **Backend / API**: FastAPI, Spring Boot, REST API, SSE
- **LLM / Retrieval**: Triton Inference Server, vLLM, Qdrant, LangChain, LangGraph
- **Data / Infra**: Oracle, Redis, Docker, Linux
- **Observability**: Prometheus, Grafana

---

## Public Repositories

> Some core projects cannot be open-sourced due to company security policies.  
> Instead, I share public experimental repositories and case studies summarizing architecture, roles, and technical contributions from production work.

### RAG / Retrieval
- [llm_article_rag_test](https://github.com/jusenglee/llm_article_rag_test)  
  RAG experiments, retrieval quality evaluation, and re-ranking ideas

### Document Processing
- [PDF_Extraction_Web](https://github.com/jusenglee/PDF_Extraction_Web)  
  PDF extraction/processing workflows and web-based document processing experiments

### Data Pipeline
- [oracle_to_qdrant__pipe](https://github.com/jusenglee/oracle_to_qdrant__pipe)  
  Oracle preprocessing, embedding, and vector DB ingestion pipeline implementation

---

## Selected Projects (incl. private)

### Domain RAG Search & Q&A System for National R&D Data *(Private / Company Project)*
- Designed and implemented a domain-specific RAG search and Q&A system for national R&D information
- Built a retrieval architecture with explicit **SEARCH / LOOKUP / JOIN** strategy separation
- Implemented Qdrant-based hybrid retrieval and re-ranking logic
- Delivered a production AI backend integrating **FastAPI, LangGraph, Triton/vLLM, and Redis**
- Improved performance and reliability with a focus on retrieval quality, latency, and operational stability

### “Everyone’s R&D” Intent Analysis & Classification System *(Partially Private)*
- Designed and implemented an **LLM-based intent analysis and classification pipeline** for user posts/queries
- Built multi-model inference orchestration and serving on an on-prem **H100 (2 GPU)** environment (including **Gemma 3**)
- Implemented an end-to-end API flow from request input → analysis → structured results for service integration
- Refactored server structure for operational reliability and iterated on **prompts / pipeline / serving** to improve consistency and stability

### Oracle → Embedding → Vector DB Ingestion Pipeline *(Private / Company Project)*
- Designed and implemented preprocessing, embedding, and vector DB ingestion pipelines from Oracle data sources
- Delivered incremental ingestion with checkpoint management, embedding parallelization, and ingestion stability improvements
- Established an operational foundation for large-scale document/metadata indexing automation

### AI Abstract Summarization Feature for Scholarly Data Platform *(Private / Company Project)*
- Designed and integrated AI abstract summarization into a large-scale scholarly content platform
- Implemented LLM API integration, result persistence, request control, and operational logging
- Gained productization-level experience by integrating AI features into an existing backend service

---

## Interests

- Building reliable LLM serving infrastructure (including on-prem/container operations)
- Improving domain-specific retrieval quality (Hybrid Retrieval + Re-ranking)
- Measuring and optimizing GPU inference performance (TTFT / throughput / memory)
- Monitoring, observability, and incident response in production environments

---

<p align="center">
  Thanks for visiting!
</p>
