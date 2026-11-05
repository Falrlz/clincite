# Clincite

[![Python](https://img.shields.io/badge/Python-3.11%2B-3776AB.svg?logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115%2B-009688.svg?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![Qdrant](https://img.shields.io/badge/Vector_DB-Qdrant-DC244C.svg?logo=qdrant&logoColor=white)](https://qdrant.tech/)
[![IBM Docling](https://img.shields.io/badge/Document_Parsing-IBM_Docling-052FAD.svg?logo=ibm&logoColor=white)](https://github.com/DS4SD/docling)
[![React](https://img.shields.io/badge/React-19-61DAFB.svg?logo=react&logoColor=black)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.8%2B-3178C6.svg?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4-38B2AC.svg?logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![Vite](https://img.shields.io/badge/Vite-6-646CFF.svg?logo=vite&logoColor=white)](https://vite.dev/)
[![MLflow](https://img.shields.io/badge/MLflow-tracking-0194E2.svg?logo=mlflow&logoColor=white)](https://mlflow.org/)
[![Managed by uv](https://img.shields.io/badge/Managed%20by-uv-DE5FE9.svg?logo=astral&logoColor=white)](https://github.com/astral-sh/uv)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**Ask. Cite. Verify.**

Consulting clinical practice guidelines takes time, and text-only PDF parsers destroy renal dosage tables and diagnostic criteria halfway through.

**Clincite** is a production-grade clinical guideline retrieval-augmented generation (RAG) system engineered for high-stakes medical literature. Indexed against official Indonesian National Clinical Practice Guidelines (*Pedoman Nasional Pelayanan Kedokteran / PNPK Kemenkes RI*), it extracts complex layout tables intact via IBM Docling, executes dual-mode hybrid search (FastEmbed dense + BM25 sparse) on local embedded Qdrant, applies FlashRank cross-encoder reranking, and enforces calibrated 3-stage guardrails with verified page and table citations.

---

## Current Status (Under Active Development)

This repository follows a strict **Progressive Directory Creation** lifecycle across three isolated pillars (`rag/`, `backend/`, and `frontend/`):

- [x] **Phase 0 — Repository Foundation & Root Config:** Git initialization, strict exclusion rules, root metadata, and foundation scaffolding. *(Current Phase)*
- [ ] **Phase 1 — Ingestion & Chunking:** Curated PNPK PDF acquisition, IBM Docling `TableFormer` extraction, and chunk EDA.
- [ ] **Phase 2 — Hybrid Retrieval & Eval Curation:** Qdrant local embedded vector store, BM25 sparse indexing, FlashRank CPU reranking, and zero-leakage evaluation datasets (`val.jsonl` & `test.jsonl`).
- [ ] **Phase 3 — Guardrails, MLflow Calibration & Evaluation:** 3-stage guardrail implementation, empirical threshold tuning, formal 100+ question benchmark, and index promotion (`v0.1.0-rag`).
- [ ] **Phase 4 — Stateless Backend API:** Read-only FastAPI service loading promoted index artifacts (`v0.2.0-backend`).
- [ ] **Phase 5 — Clinical Web UI:** React 19 + TypeScript + Tailwind CSS v4 interactive workspace with citation panels and table cards.
- [ ] **Phase 6 — Production Packaging & Cloud Release:** Multi-stage Docker, GitHub Actions CI, and live public deployment (`v1.0.0`).

---

## Architecture at a Glance

```text
[ PNPK PDF ] ──► IBM Docling (TableFormer) ──► HybridChunker (Intact Tables + Breadcrumbs)
                                                          │
                                                          ▼
[ User Query ] ──► Stage 1: Input Guardrail ──► Qdrant Hybrid Search (Dense + BM25 via RRF)
                                                          │
                                                          ▼
                                                FlashRank CPU Reranker
                                                          │
                                                          ▼
                                            Stage 2: Relevance Gate
                                            (Score >= Calibrated Threshold?)
                                             ├── No  ──► Safe Refusal (Zero LLM cost)
                                             └── Yes ──► Dual-Mode LLM Client (Ollama / Cloud)
                                                          │
                                                          ▼
                                            Stage 3: Citation & Numeric Guardrail
                                                          │
                                                          ▼
                                            Verified Clinical Answer + Page & Table Cards
```

---

## Acknowledgements

Clincite is built upon open-source research and engineering tools:

- **IBM Docling & TableFormer** by [DS4SD / IBM Research](https://github.com/DS4SD/docling)
- **Qdrant Vector Database** by [Qdrant Team](https://qdrant.tech/)
- **FastEmbed** by [Qdrant](https://github.com/qdrant/fastembed)
- **FlashRank** by [Prithviraj Damodaran](https://github.com/Prithivida/FlashRank)
- **MLflow** by [The Linux Foundation / Databricks](https://mlflow.org/)
- **uv** package manager by [Astral](https://github.com/astral-sh/uv)
- **Pedoman Nasional Pelayanan Kedokteran (PNPK)** published openly by [Kementerian Kesehatan Republik Indonesia](https://kemkes.go.id/)

---

## Disclaimers

### Medical Disclaimer
Clincite is an academic reference retrieval aid based on indexed *Pedoman Nasional Pelayanan Kedokteran (PNPK)* documents. It is **not** a certified medical device, diagnostic system, or substitute for clinical judgment, medical consultation, or therapy decisions by certified healthcare professionals. Dosage adjustments and clinical actions must always be verified against the official primary source documents.

### Non-Affiliation Notice
Clincite is an independent software engineering and AI research project utilizing publicly accessible clinical guidelines. It is not affiliated with, sponsored by, or endorsed by the Ministry of Health of the Republic of Indonesia (*Kementerian Kesehatan RI*).

---

## License

This project is licensed under the [MIT License](LICENSE).
