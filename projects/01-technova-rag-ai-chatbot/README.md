# 🤖 TechNova RAG AI Chatbot

An AI-powered company knowledge base assistant built with **n8n, OpenAI, Supabase, and Google Drive**. The chatbot uses Retrieval-Augmented Generation (RAG) to answer questions based on company PDF documents rather than relying only on general model knowledge.

> **Portfolio demo:** TechNova Solutions is a fictional company. All HR policies and company information used in this project are sample data.

## 🎯 Project Overview

This project demonstrates an end-to-end RAG system that transforms PDF documents into a searchable knowledge base and allows users to ask questions through a conversational AI chatbot.

### Problem

Employees often spend time manually searching through company documents to find answers about HR policies, leave entitlements, benefits, and internal procedures.

### Solution

An AI chatbot that retrieves relevant information from company documents and generates clear, context-aware answers, with source references when available.

## 🛠 Tech Stack

- **n8n** — Workflow automation and AI agent orchestration
- **OpenAI GPT-5 mini** — Conversational language model
- **OpenAI Embeddings** — 1536-dimensional text embeddings
- **Supabase / pgvector** — Vector storage and semantic retrieval
- **Google Drive** — PDF document source
- **Simple Memory** — Recent conversation context

## 🏗 System Architecture

The solution consists of two n8n workflows.

### Workflow 01: Document Ingestion

```text
Manual Trigger
    ↓
Search PDF Documents in Google Drive
    ↓
Download PDF
    ↓
Load and Extract PDF Text
    ↓
Split Text into Chunks
    ↓
Generate OpenAI Embeddings
    ↓
Store Chunks and Embeddings in Supabase
```

This workflow prepares company documents for semantic search. The demonstrated ingestion processed one sample HR policy PDF into three document chunks.

**Configuration highlights:**

- PDF extraction using n8n's document loader
- Recursive character text splitting
- Chunk overlap: 150 characters
- Embedding dimensions: 1536
- Supabase table: `documents`
- Manual execution
- Google Drive search currently limited to one PDF per run

### Workflow 02: AI Knowledge Chatbot

```text
User Chat Message
    ↓
AI Agent (GPT-5 mini)
    ├── Simple Memory
    └── Supabase Vector Store Tool
           └── OpenAI Embeddings
    ↓
Document-Grounded Answer
```

The AI agent searches the knowledge base when answering company-related questions and uses the retrieved information to generate a response.

**Configuration highlights:**

- Supabase Vector Store in retrieve-as-tool mode
- 1536-dimensional embeddings
- Simple Memory with a context window of 10
- Source-aware response instructions
- Instructions to avoid inventing unsupported company policies

## 🧪 Testing and Validation

The following scenarios were tested successfully in the development environment:

| Test Scenario | Expected Behavior | Result |
|---|---|---|
| Sick leave entitlement | Retrieve the sample policy entitlement | Passed |
| Medical documentation follow-up | Use document information and conversational context | Passed |
| Work-from-home policy not found | State that the available documents do not provide the answer | Passed |
| Document ingestion | Store document chunks and embeddings | Passed |
| Vector similarity search | Return relevant stored document chunks | Passed |

The Supabase database was verified to contain **3 document chunks and 3 embeddings**, with no missing embeddings at the time of testing.

These results demonstrate a working portfolio prototype, not a production-scale performance or security certification.

## 📁 Repository Files

```text
01-technova-rag-ai-chatbot/
├── README.md
├── workflows/
│   ├── 01-document-ingestion.json
│   └── 02-ai-chatbot.json
└── screenshots/
```

## ⚙️ Setup and Reproduction

1. Create an n8n environment.
2. Prepare a Supabase project with pgvector support and a compatible `documents` table and similarity search function.
3. Configure OpenAI, Supabase, and Google Drive credentials.
4. Import the two n8n workflow JSON files.
5. Reconnect credentials and select your own Google Drive folder.
6. Confirm the embedding dimensions match the vector database schema.
7. Execute the document ingestion workflow.
8. Verify stored chunks and embeddings in Supabase.
9. Activate the chatbot workflow and test document-grounded questions.

**Note:** The public workflow exports are intended as reusable templates and require your own service configuration. Do not commit API keys, access tokens, private documents, or environment secrets.

## 🔒 Security and Limitations

- Sample company documents are fictional.
- Credentials must be configured separately in n8n.
- The ingestion workflow currently runs manually.
- The current Google Drive search is limited to one PDF per execution.
- Chatbot accuracy depends on the quality and coverage of the uploaded documents.
- Additional access control, monitoring, error handling, and evaluation would be needed for production deployment.

## 🚀 Future Improvements

- Automated document synchronization
- Multi-document ingestion
- Duplicate detection and document update handling
- Authentication and role-based access control
- Improved citation and source tracking
- Automated RAG evaluation and monitoring
- Production-ready deployment and error handling

## 💼 Skills Demonstrated

AI automation · n8n workflow development · Retrieval-Augmented Generation (RAG) · AI agents · Semantic search · Vector databases · Document processing · Conversational AI · Workflow testing

---

**Project type:** AI Automation / RAG Knowledge Base Chatbot  
**Status:** Working portfolio prototype  
**Company:** TechNova Solutions (fictional demo)
