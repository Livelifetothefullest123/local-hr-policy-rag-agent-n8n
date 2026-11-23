# local-hr-policy-rag-agent-n8n
# Local HR & Global Mobility Policy RAG Agent (n8n + Ollama)

![Architecture and Chat Output](architecture.png)

## Overview
A privacy-first Retrieval-Augmented Generation (RAG) assistant built in **n8n** and powered entirely by local **Ollama** models. HR teams and relocating employees can query complex company policy handbooks in natural language with grounded citations and zero external API calls.

## Problem & Solution
* **Problem:** Employee mobility handbooks are dense PDFs, and uploading internal HR policies to public cloud LLMs creates data privacy and compliance risks.
* **Solution:** Built a two-pipeline local RAG workflow that indexes PDF policy documents into a vector store using local embeddings and exposes a tool-calling AI Agent capable of answering multi-turn policy questions strictly from retrieved context.

## Tech Stack
* **Orchestration:** n8n (v2.x AI Agent & Vector Store nodes)
* **LLM & Embeddings:** Ollama (`qwen3.5:4b` for reasoning/tool-calling, `nomic-embed-text` for dense vector embeddings)
* **Memory & Retrieval:** In-Memory Vector Store + Window Buffer Chat Memory

## Architecture
1. **Indexing Pipeline:** Loads PDF files from disk → splits text via Default Data Loader → generates embeddings via `nomic-embed-text` → stores vectors under `mobility_policies`.
2. **Inference Pipeline:** Chat Trigger → AI Agent (`qwen3.5:4b`) → invokes `mobility_policy_search` vector retrieval tool → synthesizes cited answers with conversational memory.

## How to Run Locally
1. Install [Ollama](https://ollama.com) and pull the required models:
   ```bash
   ollama pull qwen3.5:4b
   ollama pull nomic-embed-text
