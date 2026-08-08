# Legal RAG PT - Documentation

Documentation for a fully local Retrieval-Augmented Generation (RAG) system that answers natural-language questions about the **Regulatory Code of the Municipality of Porto (CRMP)**.

The solution processes and indexes the regulatory document, retrieves relevant legal passages through semantic search, and uses a locally executed language model to generate answers in European Portuguese with article and page references.

## Project repositories

The implementation is divided into two complementary repositories:

| Repository | Purpose |
|---|---|
| [`legal-rag-pt`](https://github.com/ruialexrib/legal-rag-pt) | Document extraction, text preprocessing, legal structure parsing, chunking, embedding generation, Qdrant indexing, vector search, and retrieval evaluation. |
| [`legal-rag-pt-n8n`](https://github.com/ruialexrib/legal-rag-pt-n8n) | Conversational application and n8n workflow for question embedding, context retrieval, grounded answer generation, and source presentation. |

## Architecture

```text
CRMP PDF
   |
   v
Extraction and preprocessing
   |
   v
Article parsing and chunking
   |
   v
bge-m3 embeddings --> Qdrant vector collection
                          |
User question             |
   |                      |
   v                      |
n8n workflow --> semantic search (top 5)
   |                      |
   +<---------------------+
   |
   v
Grounded context --> AMALIA-9B via Ollama
   |
   v
Answer in European Portuguese with article and page references
```

The architecture separates operations performed during corpus preparation from those performed for each query:

- **Ingestion and indexing:** extracts the CRMP, preserves its legal structure and page provenance, creates overlapping chunks, generates embeddings, and stores them in Qdrant.
- **Query and generation:** embeds the user's question, retrieves the most relevant chunks, builds a grounded context, generates the answer, and presents the corresponding sources.

## Main technologies

- [bge-m3](https://huggingface.co/BAAI/bge-m3) for multilingual text embeddings
- [Qdrant](https://qdrant.tech/) for vector storage and similarity search
- [Ollama](https://ollama.com/) for local model execution
- [AMALIA-9B](https://huggingface.co/ruialexrib/AMALIA-9B-0626-SFT-GGUF) for answer generation in European Portuguese
- [n8n](https://n8n.io/) for workflow orchestration and the conversational interface
- Python and Jupyter notebooks for corpus processing and retrieval evaluation

## Documentation

The complete technical report is available in [`legal_rag_pt_doc.pdf`](legal_rag_pt_doc.pdf).

It covers:

- theoretical background on embeddings, vector databases, and RAG;
- system architecture and local environment setup;
- extraction and processing of the 662-page CRMP document;
- identification of 1,440 articles and creation of 1,617 chunks;
- generation of 1,024-dimensional embeddings with `bge-m3`;
- Qdrant indexing and semantic vector search;
- retrieval evaluation using Hit@K, Recall@K, and Mean Reciprocal Rank;
- implementation of the conversational application in n8n;
- limitations and directions for future work.

## Preliminary results

The retrieval component was initially evaluated using five manually annotated questions. A relevant article was ranked first in every case, producing `Hit@1`, `Recall@1`, and `MRR` scores of `1.000`, with an average search latency of `0.909 s`.

These results validate the implementation for the evaluated examples but should not be interpreted as evidence of general performance. A larger and more diverse evaluation set is required.

## Getting started

Clone both implementation repositories:

```bash
git clone https://github.com/ruialexrib/legal-rag-pt.git
git clone https://github.com/ruialexrib/legal-rag-pt-n8n.git
```

Then follow their individual setup instructions:

1. Use [`legal-rag-pt`](https://github.com/ruialexrib/legal-rag-pt) to process the source document, generate embeddings, and populate the `crmp_bge_m3` Qdrant collection.
2. Use [`legal-rag-pt-n8n`](https://github.com/ruialexrib/legal-rag-pt-n8n) to start n8n, import the workflow, and access the conversational interface.

## Scope and disclaimer

This project is intended for experimental and educational purposes. Generated responses do not constitute legal advice and must be verified against the applicable official sources, including the official CRMP text.

## Author

Rui Ribeiro - [github.com/ruialexrib](https://github.com/ruialexrib)
