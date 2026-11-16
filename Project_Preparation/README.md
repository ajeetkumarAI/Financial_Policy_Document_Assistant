# 📑 GenAI Powered Financial Policy Assistant — End-to-End Solution


## 💼 1. Business Problem

In banks, insurance firms, and financial institutions, professionals face **thousands of pages of compliance manuals, regulatory filings, and internal policy documents**.

- Searching manually wastes time and increases compliance risk.
- Misinterpretation can lead to **multi-million dollar fines**.
- Employees and auditors need **instant access to precise, policy-based answers**.

This project solves that problem by creating an AI assistant that ingests **financial policy PDFs**, converts them into embeddings, and answers questions strictly based on those documents — ensuring compliance and reducing risk.



## 🌍 2. Why This Problem Is Important

- **Financial regulations = 1000s of pages** (Basel III, IFRS, SEC filings, RBI circulars, etc.)
- Manual searching is **slow and error-prone**
- **Time-critical decisions** in audits, reporting, and client onboarding
- **Accuracy is non-negotiable** — hallucinations = fines, lawsuits, reputational damage
- Staff are often non-technical → need **natural language Q&A**

👉 "This AI tool democratizes access to financial compliance knowledge, reduces manual effort, and minimizes regulatory risk."



## 🧩 3. Challenges Solved

**Challenge 1 — Long, unstructured financial documents**
- PDFs contain dense legal text, tables of thresholds, irregular formatting, and no metadata.
- ✅ Solution: **OpenAI embedding models** convert text chunks into vector representations stored in Vector Store.

**Challenge 2 — Accuracy in compliance domain**
- LLMs must not hallucinate.
- ✅ Solution: Retrieval-Augmented Generation (RAG) ensures answers are grounded in the uploaded policies only.

**Challenge 3 — Strict document-only answers**
- No external knowledge allowed.
- ✅ Solution: Query pipeline enforces grounding in embeddings, with citations from source PDFs.

**Challenge 4 — Fast inference at scale**
- Compliance queries often happen under audit pressure.
- ✅ Solution: OpenAI LLMs provide low-latency, high-quality responses.

## 🏗️ 4. Technical Architecture

### 🔹 Flow Overview
1. **Frontend (Streamlit)**
   - Chatbot UI for document upload + Q&A.
   - Displays answers with citations.

2. **Backend RAG Pipeline**
   - Handles preprocessing, embeddings, retrieval, and LLM orchestration.

3. **Embedding & Retrieval Layer**
   - **OpenAI Embedding Models** (`text-embedding-ada-002` or newer).
   - Vector DB: FAISS/Chroma (self-hosted) or Azure Cognitive Search.

4. **LLM Layer**
   - **OpenAI GPT models** for grounded answers.
   - RAG pipeline ensures strict document grounding.

5. **Deployment (Azure Web App)**
   - Streamlit frontend deployed separately.
   - Azure services:
     - **Blob Storage** → raw PDFs
     - **Key Vault** → secure API keys
     - **Monitor** → logging & performance


### 🔹 Architecture Diagram (Conceptual)

```
[User Query] → [Streamlit Frontend]
       ↓
   [OpenAI LLM] ← [Relevant Chunks] ← [Chroma Vector DB]
       ↑                               ↑
   [Embedding API] ← [Chunked PDFs] ← [Document Loader]
```



## 📂 5. Document Loaders

- **PyPDFLoader** → standard PDFs
- **UnstructuredPDFLoader** → messy PDFs with tables
- **DocxLoader** → Word-based compliance docs
- **CSVLoader** → tabular compliance thresholds
- **HTMLLoader** → regulatory websites

Other PDF libraries considered: pypdf, pdfplumber, pdfminer, **PyMuPDF / pymupdf4llm** (used in this project).



## ✂️ 6. Chunking Strategies

- **Fixed-size chunking** (512–1024 tokens)
- **Table-aware chunking** → preserve rows/columns
- **Semantic chunking** → split by headings/clauses
- **Overlap strategy** (50–100 tokens overlap) → prevent context loss

This project uses `RecursiveCharacterTextSplitter` (chunk size 1000, overlap 200, separators: paragraph → new line → sentence → word).

## 🔢 7. Embedding Model

- **Sentence Transformers** (open source, e.g. all-MiniLM-L6-v2)
- **OpenAI Embedding** (ada-002, text-embedding-3-small / large) — used in this project
- **Gemini Embedding**
- **Cohere**



## 🚀 8. POC Phase

- Build with **20% data or partial data (public data)**
- Propose the solution technically to the business / stakeholders, covering:
  - Timeline
  - Budget
  - Team structure
- Sign the agreement for the full project

## 📄 9. Document Understanding

Document types in scope: PDF, DOCX, TXT, Confluence, web scraping.
