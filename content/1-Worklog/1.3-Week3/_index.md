---
title: "Worklog - Week 3"
date: 2026-06-15
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:
* Synchronize with the team's data processing (ETL) workflow: Complete the Retrieval-Augmented Generation (RAG) loop.
* Build a two-stage retrieval architecture combining Vector Search and Cross-Encoder Reranking to optimize context precision.
* Integrate an LLM (Google Gemini) to complete the context-aware question-answering workflow.
* Explore AWS cloud infrastructure (Lambda, Bedrock) towards the end of the week.

### Tasks Completed During the Week:
| Day | Task | Start Date | Completion Date | Reference Material |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Registered for an API Key and wrote a test script to invoke the Google Gemini API. | 06/15/2026 | 06/15/2026 | [Gemini API Docs](https://ai.google.dev/docs) |
| 3 | - Loaded the `BAAI/bge-m3` model via `sentence-transformers` to embed search queries (synchronizing the vector space with the team). | 06/16/2026 | 06/16/2026 | [HuggingFace - BAAI/bge-m3](https://huggingface.co/BAAI/bge-m3) |
| 4 | - Installed `qdrant-client` and connected to the Qdrant cluster. <br> - Finalized Payload (Metadata) structures with the ETL team. | 06/17/2026 | 06/17/2026 | [Qdrant Python Client](https://qdrant.tech/documentation/interfaces/python/) |
| 5 | - Built a two-stage retrieval pipeline: <br> 1. Queried Qdrant to retrieve Top-N results. <br> 2. Applied a Cross-Encoder model (e.g., `BAAI/bge-reranker-base`) to score and rerank down to Top-K. | 06/18/2026 | 06/18/2026 | [Advanced RAG - Reranking](https://www.pinecone.io/learn/advanced-rag-techniques/) |
| 6 | - Applied Prompt Engineering techniques: Injected the reranked Top-K context into Gemini to generate accurate Vietnamese responses. | 06/19/2026 | 06/19/2026 | [Prompt Engineering Guide](https://www.promptingguide.ai/) |
| 7 | - **End-to-End Team Testing:** Awaited data push completion from the ETL pipeline -> Executed queries -> Reranked -> Generated responses via LLM. | 06/20/2026 | 06/20/2026 | N/A |

### Week 3 Achievements:

* Optimized context quality by successfully implementing a two-stage retrieval architecture, utilizing Cross-Encoders to refine and reassess the relevance of raw search results from Qdrant.

* Achieved seamless team collaboration workflows, cleanly separating the Data Ingestion pipeline from the Data Consumption flow.

* Synchronized the vector space by sharing the same `BAAI/bge-m3` model with the ETL workflow.

* Completed the semantic search tool and integrated it smoothly with Google Gemini, successfully passing all local End-to-End test scenarios.