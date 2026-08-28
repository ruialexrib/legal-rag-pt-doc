<div align="center">

# Legal RAG PT — Documentation

### Technical documentation for a fully local RAG system over Portuguese legal documents

[![RAG](https://img.shields.io/badge/RAG-Retrieval--Augmented%20Generation-7B2CBF)](#architecture)
[![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Qdrant](https://img.shields.io/badge/Qdrant-Vector%20Database-DC244C?logo=qdrant&logoColor=white)](https://qdrant.tech/)
[![Ollama](https://img.shields.io/badge/Ollama-Local%20Models-black)](https://ollama.com/)
[![n8n](https://img.shields.io/badge/n8n-Workflow%20Automation-EA4B71?logo=n8n&logoColor=white)](https://n8n.io/)

**Local-first · Portuguese legal RAG · Semantic retrieval · Grounded generation**

</div>

---

## About

This repository contains the central technical documentation for a fully local **Retrieval-Augmented Generation (RAG)** system that answers natural-language questions about the **Regulatory Code of the Municipality of Porto (CRMP)**.

The solution processes and indexes the regulatory document, retrieves relevant legal passages through semantic search, and uses a locally executed language model to generate answers in European Portuguese with article and page references.

The complete technical report is available in [`legal_rag_pt_doc.pdf`](legal_rag_pt_doc.pdf).

---

## Project Repositories

| Repository | Purpose |
| --- | --- |
| [`legal-rag-pt`](https://github.com/ruialexrib/legal-rag-pt) | Document extraction, preprocessing, legal parsing, chunking, embeddings, Qdrant indexing, vector search, and retrieval evaluation. |
| [`legal-rag-pt-n8n`](https://github.com/ruialexrib/legal-rag-pt-n8n) | Conversational application and n8n workflow for retrieval, grounded answer generation, and source presentation. |

---

## Architecture

```text
CRMP PDF
   │
   ▼
Extraction and preprocessing
   │
   ▼
Article parsing and chunking
   │
   ▼
bge-m3 embeddings ──────► Qdrant vector collection
                              │
User question                 │
   │                          │
   ▼                          │
n8n workflow ──► semantic search (Top-5)
   │                          │
   ◄──────────────────────────┘
   │
   ▼
Grounded context ──► AMALIA-9B via Ollama
   │
   ▼
European Portuguese answer
with article and page references
```

The architecture separates two main stages:

- **Ingestion and indexing** — extracts the CRMP, preserves legal structure and page provenance, creates overlapping chunks, generates embeddings, and stores them in Qdrant.
- **Query and generation** — embeds the question, retrieves the most relevant chunks, constructs grounded context, generates the answer, and presents the corresponding sources.

---

## Technology Stack

| Technology | Purpose |
| --- | --- |
| **Python / Jupyter** | Corpus processing and retrieval evaluation |
| **bge-m3** | Multilingual text embeddings |
| **Qdrant** | Vector storage and similarity search |
| **Ollama** | Local model execution |
| **AMALIA-9B** | Answer generation in European Portuguese |
| **n8n** | Workflow orchestration and conversational interface |

---

## Documentation Scope

The technical report covers:

- Embeddings, vector databases, and RAG fundamentals
- System architecture and local environment setup
- Extraction and processing of the 662-page CRMP document
- Identification of 1,440 articles
- Creation of 1,617 text chunks
- Generation of 1,024-dimensional `bge-m3` embeddings
- Qdrant indexing and semantic vector search
- Retrieval evaluation with Hit@K, Recall@K, and Mean Reciprocal Rank
- Conversational application implementation with n8n
- Limitations and future work

---

## Preliminary Results

The retrieval component was initially evaluated using five manually annotated questions. A relevant article was ranked first in every evaluated case:

| Metric | Result |
| --- | ---: |
| Hit@1 | `1.000` |
| Recall@1 | `1.000` |
| MRR | `1.000` |
| Mean search latency | `0.909 s` |

These results validate the implementation for the evaluated examples but should **not** be interpreted as evidence of general retrieval performance. A larger and more diverse evaluation set is required.

---

## Getting Started

Clone the two implementation repositories:

```bash
git clone https://github.com/ruialexrib/legal-rag-pt.git
git clone https://github.com/ruialexrib/legal-rag-pt-n8n.git
```

Then:

1. Use `legal-rag-pt` to process the source document, generate embeddings, and populate the `crmp_bge_m3` Qdrant collection.
2. Use `legal-rag-pt-n8n` to start n8n, import the workflow, and access the conversational interface.

Refer to each repository's README for detailed setup instructions.

---

## Repository Structure

```text
legal-rag-pt-doc/
├── legal_rag_pt_doc.pdf   # Complete technical report
└── README.md               # Project overview
```

---

## Scope and Disclaimer

This project is intended for **experimental, educational, and technical demonstration purposes**.

Generated responses do not constitute legal advice and must be verified against the applicable official sources, including the official CRMP text.

---

## Author

**Rui Ribeiro** — [GitHub](https://github.com/ruialexrib)
