# Financial Policy Document ChatBot Assistant

A Streamlit-based Financial Policy chatbot that uses a RAG (Retrieval-Augmented Generation) pipeline to answer questions about uploaded financial policy and compliance documents. Answers are grounded strictly in the uploaded PDFs, with page citations.

## Features

- Upload financial policy / compliance PDFs
- PDF loading with PyMuPDF
- Recursive chunking (1000 characters, 200 overlap; paragraph → line → sentence → word)
- OpenAI embeddings stored in ChromaDB for fast retrieval
- Similarity search (top 10 chunks) + OpenAI chat model for grounded answers
- Says the document does not cover it instead of guessing
- All settings in `config/config.yaml`

## Prerequisites

- Python 3.11+
- An [OpenAI API key](https://platform.openai.com/api-keys)

## Setup

1. **Create and activate a virtual environment:**

   ```bash
   python -m venv venv

   # macOS / Linux
   source venv/bin/activate

   # Windows (PowerShell)
   .\venv\Scripts\Activate.ps1

   # Windows (Command Prompt)
   venv\Scripts\activate.bat
   ```

2. **Install dependencies:**

   ```bash
   pip install -r requirements.txt

   # or, with uv
   uv pip install -r requirements.txt
   ```

3. **Configure your API key:**

   Copy `.env.example` to `.env` in the project root and set your key:

   ```
   OPENAI_API_KEY=sk-your-key-here
   ```

## Usage

```bash
streamlit run app.py

python -m streamlit run app.py
```

```bash
streamlit run app.py --server.port=8000 --server.address=0.0.0.0
```

Open the URL shown in your terminal (default: http://localhost:8501), upload a financial policy PDF (a sample is in `data/sample_pdfs/`), click **Ingest/Process Document**, then ask questions.

Sample questions:

- What is the main purpose of the Financial Policy and Procedures manual?
- What types of bank accounts does Integrity Action maintain?
- What are the quotation requirements based on purchase value?
- Who are the current authorised bank signatories?

To stop the app, press `Ctrl+C` in the terminal.

> Ingest each document once. Processing the same PDF again adds duplicate chunks; delete the `db/` folder to start fresh.

## Configuration

`config/config.yaml`:

| Setting | Default |
|---|---|
| LLM | `gpt-5.5`, temperature 0 |
| Embeddings | `text-embedding-ada-002` |
| Chunking | size 1000, overlap 200 |
| Retriever | similarity, k = 10 |
| Vector store | `db/` |

If you change the embedding model, delete `db/` and ingest again.

## Run in GitHub Codespaces

The `.devcontainer/` folder lets you open the repo in GitHub Codespaces: dependencies install automatically and the Streamlit app starts on port 8501. Add `OPENAI_API_KEY` as a Codespaces secret.

## Project Structure

```
app.py                  # Streamlit UI entry point
src/
  config_loader.py      # Reads config/config.yaml
  document_loaders.py   # PDF loading via PyMuPDF
  chunking.py           # Recursive text splitting
  embeddings.py         # OpenAI embedding model
  vectorstore.py        # ChromaDB vector store + retriever
  llm_integration.py    # ChatOpenAI LLM wrapper
  rag_pipeline.py       # Indexing and QA orchestration
prompts/
  prompt_template.py    # System/user prompt builder
config/
  config.yaml           # Centralised configuration values
exploration/
  notebook.ipynb        # Step-by-step RAG notebook
data/sample_pdfs/       # Uploaded PDFs stored here
Project_Preparation/
  README.md             # Business problem, architecture, POC plan
```
