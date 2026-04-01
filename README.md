<p align="right">
  <strong>English</strong> | <a href="./README.ko.md">한국어</a>
</p>

# Seungju Lee 👋

**LLM Serving & RAG Systems Engineer**

I design and ship **RAG, retrieval, and LLM serving systems** with FastAPI, Qdrant, Triton/vLLM, and on-prem GPU environments.  
With a strong backend foundation, I have expanded into **AI engineering (serving · retrieval · operations)** and focus on building systems that are not only accurate, but also explainable and operable in production.

[Blog / Notes](https://www.notion.so/JusengLee-1753a475fe0080a3b62eef1af1688e19?source=copy_link)

---

## About

- I build **domain-specific RAG systems** and **LLM serving infrastructure**.
- I care about **retrieval quality, response latency, and operational reliability** together.
- My main interests are **LLM Serving, Hybrid Retrieval, AI Backend, Data Pipelines, and Observability**.

---

## Featured Case Studies

### 1. NTIS AI Chatbot — Search Engine-Grade RAG
A domain RAG system for national R&D information that evolved from a simple chatbot into a **search-engine-style retrieval architecture**.  
Key themes: **SEARCH / LOOKUP / JOIN**, planner–contract–executor design, hybrid retrieval, reranking, SSE responses, and evidence-driven answers.  
→ [Read the case study](./case-studies/ntis-rag-chatbot.md)

### 2. Everyone’s R&D — Intent Analysis & Classification
An LLM-powered system that converts free-form user feedback into **corrected text, structured summaries, and topic classifications**.  
Built for an on-prem serving environment with **Gemma 3, Triton, multi-model orchestration, and API integration**.  
→ [Read the case study](./case-studies/everyones-rnd-intent-classification.md)

### 3. Oracle → Embedding → Vector DB Pipeline
A production-oriented ingestion pipeline that turns Oracle-based source data into vector-search-ready assets.  
Key themes: preprocessing, checkpointing, incremental loads, embedding evaluation, and stable Qdrant ingestion.  
→ [Read the case study](./case-studies/oracle-to-qdrant-pipeline.md)

### 4. Scholarly OA AI Summarization
An AI summarization feature built for a large-scale scholarly platform handling roughly **400,000 papers**.  
Key themes: GPU inference setup, FastAPI integration, result persistence, request control, and product-level rollout.  
→ [Read the case study](./case-studies/scholarly-ai-summarization.md)

---

## Public Repositories

### RAG / Retrieval
- [llm_article_rag_test](https://github.com/jusenglee/llm_article_rag_test)  
  Experiments around RAG structure, retrieval quality, and reranking ideas.  
  → [Project note](./projects/llm_article_rag_test.md)

### Document Processing
- [PDF_Extraction_Web](https://github.com/jusenglee/PDF_Extraction_Web)  
  A web-based experiment around PDF extraction and document-processing workflows.  
  → [Project note](./projects/PDF_Extraction_Web.md)

### Data Pipeline
- [oracle_to_qdrant__pipe](https://github.com/jusenglee/oracle_to_qdrant__pipe)  
  Oracle preprocessing, embedding, and vector DB ingestion pipeline implementation.  
  → [Project note](./projects/oracle_to_qdrant__pipe.md)

---

## Shared Notes

- [Architecture](./docs/architecture.md)
- [Benchmarks & Public Metrics](./docs/benchmarks.md)
- [Contracts & Design Rules](./docs/contracts.md)
- [Runbook & Operations Notes](./docs/runbook.md)

---

## Tech Stack

- **Languages**: Python, Java
- **Backend / API**: FastAPI, Spring Boot, REST API, SSE
- **LLM / Retrieval**: Triton Inference Server, vLLM, Qdrant, LangChain, LangGraph
- **Data / Infra**: Oracle, Redis, Docker, Linux
- **Observability**: Prometheus, Grafana, structured logs, quality gates

---

## What I Care About

- Reliable LLM serving infrastructure
- Domain-specific retrieval quality
- GPU inference performance tuning
- Monitoring, observability, and incident response in production

---

## Repository Structure

```text
portfolio/
├─ README.md
├─ README.ko.md
├─ assets/
├─ case-studies/
├─ docs/
└─ projects/
```

> Some source code and internal artifacts cannot be shared due to company security policies.  
> This repository focuses on **architecture, decisions, evidence, and technical contribution** rather than confidential implementation details.
