# Sovereign AI Workbench

Sovereign AI Workbench is a privacy-first, locally hosted AI application for chatting with a local language model, analyzing PDF documents, extracting text from scanned pages, searching a private document knowledge base, and running bounded, verifiable agent tasks.

The application is designed to keep normal inference and document processing on the user's own computer through [Ollama](https://ollama.com/). It provides both a browser-based interface served locally over a trusted network and a lightweight Tkinter desktop client.

## Features

- **Local AI chat:** stream answers from an Ollama-hosted language model.
- **Document analysis:** extract text from PDFs and use Tesseract OCR for scanned pages.
- **Retrieval-augmented generation (RAG):** index local documents and ask questions grounded in the indexed content, with source and page references.
- **Agent tasks:** use a restricted workflow for calculations, authorized document search, and generated TXT/PDF reports.
- **Browser and desktop clients:** use the FastAPI web interface or standalone Tkinter application.
- **Local user management:** authenticated sessions and user-level separation for documents and knowledge collections.
- **Security-conscious execution:** bounded inputs and agent capabilities; uploaded document content is treated as untrusted data.

## Technology

- Python
- FastAPI
- Ollama for local language-model inference
- ChromaDB and local embeddings for retrieval
- Tesseract OCR and PDF text extraction
- HTML, CSS, and JavaScript for the browser interface
- Tkinter for the desktop client

## Requirements

- Windows 10/11 (the launch scripts and steps below use Windows PowerShell).
- Python 3.10 or newer.
- [Ollama](https://ollama.com/) installed and running.
- Tesseract OCR installed for scanned-PDF OCR.
- Python dependencies from `requirements.txt`.

Download the required models once:

```powershell
ollama pull llama3.2
ollama pull nomic-embed-text
ollama list
```

The first model download requires internet access. Once packages and models are present locally, normal inference and document processing can run without a cloud AI API.

## Setup

Clone the repository and create a virtual environment:

```powershell
git clone https://github.com/harshit-033/Sovereign_AI_Workbench.git
cd Sovereign_AI_Workbench
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

If PowerShell blocks virtual-environment activation, call the environment's Python directly:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

## Run the application

### Browser interface

```powershell
.\.venv\Scripts\python.exe run_server.py
```

Alternatively, run `run_server.bat`. Open the local URL printed by the server, usually:

```text
http://localhost:8000
```

The terminal provides the initial administrator credentials when a new installation requires bootstrap. Configure credentials through the supported environment variables before launch if needed; use a strong, unique password.

### Desktop client

```powershell
.\.venv\Scripts\python.exe app.py
```

Alternatively, run `run_chat_app.bat`.

## Project layout

```text
agent/          Restricted planning, tools, approvals, audit, and outputs
core/           Authentication, sessions, file handling, queues, and AI services
document/       PDF loading, extraction, OCR, and document models
rag/            Chunking, embeddings, metadata, vector storage, and retrieval
routing/        Request classification and capability routing
server/         FastAPI endpoints and server-side application logic
static/         Browser UI (HTML, CSS, and JavaScript)
tests/          Functional, document, retrieval, routing, and security tests
app.py          Tkinter desktop client
run_server.py   Browser-server launcher
requirements.txt Python dependencies
SERVER_GUIDE.md Server operation, configuration, and security reference
```

## Configuration and data

The application supports environment-based configuration for the chat model, embedding model, host and port, user-store location, upload directory, RAG storage, agent mode, API documentation, and TLS settings. See [SERVER_GUIDE.md](SERVER_GUIDE.md) for the detailed options.

Uploaded documents, account data, embeddings, and generated outputs may contain private information. Runtime data should remain local and should not be committed to the repository. This repository excludes the sample RAG uploads and metadata; the knowledge base can be populated through the application.

## Run tests

Generate deterministic PDF fixtures:

```powershell
.\.venv\Scripts\python.exe .\tests\create_synthetic_pdfs.py
```

Run the tests:

```powershell
Get-ChildItem .\tests\test_*.py | ForEach-Object {
    & .\.venv\Scripts\python.exe $_.FullName
    if ($LASTEXITCODE -ne 0) { throw "$($_.Name) failed" }
}
```

Tests that exercise actual model inference require Ollama and the configured models to be available.

## Security notes

- Use the service on localhost or a trusted private LAN unless HTTPS is configured.
- Use HTTPS before transmitting credentials over an untrusted network.
- Protect uploaded documents, local user-store files, and generated outputs.
- The agent workflow is intentionally restricted and should not be treated as an unrestricted shell or general-purpose code execution environment.
- Review dependencies and run a security audit before production deployment.

## Scope

Sovereign AI Workbench is a general-purpose local AI and document-intelligence project. It does not require a hackathon, competition, or event-specific workflow to run.
