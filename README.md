# NeuroRag

**NeuroRag** is a **local Retrieval-Augmented Generation (RAG) assistant** designed for **neuroscience research and computational modeling** (Python / NEURON).

It allows you to **index your private scientific literature** (PDF papers, notes, documentation) and **query it using a local LLM** via **Ollama**, while keeping all data **fully local and private**.

The system retrieves relevant text from your personal research corpus and provides **grounded answers with source references**.

---

# Why NeuroRag?

Scientific research involves large amounts of literature, documentation, and code examples.

NeuroRag helps researchers:

* Quickly search and retrieve information from scientific papers
* Ask natural language questions about their research corpus
* Retrieve relevant code examples and modeling documentation
* Build a **personal AI research assistant** that runs locally


---

# Key Features

* **Local-first architecture**
  All data and models run locally. No external APIs required.

* **Private knowledge base**
  Your papers and research files remain on your machine.

* **Scientific document ingestion**
  Converts PDFs to structured text for indexing.

* **Semantic search using vector embeddings**

* **Local LLM integration via Ollama**

* **Source-grounded answers**

---

# RAG Pipeline Architecture

The system follows a standard Retrieval-Augmented Generation pipeline:

```
Scientific PDFs
      │
      ▼
PDF Text Extraction
      │
      ▼
Text Chunking
      │
      ▼
Embedding Model
(HuggingFace Sentence Transformers)
      │
      ▼
Vector Database
(FAISS)
      │
      ▼
Retriever
      │
      ▼
Local LLM (Ollama)
      │
      ▼
Answer with sources
```

---

# What the current version does

The current implementation provides a **minimal but complete local RAG pipeline**.

### 1. PDF Extraction

```
src/extract_pdfs.py
```

* Reads PDFs from `data/raw/`
* Extracts text using `pypdf`
* Saves `.txt` files to `data/processed/`

---

### 2. Metadata Generation

```
src/create_metadata.py
```

Creates a metadata table:

```
data/interim/paper_metadata.csv
```

Fields include:

* filename
* relative path
* year
* first author
* category


This metadata can later be used for **advanced filtering and retrieval**.

---

### 3. Document Loading

```
src/load_documents.py
```

Loads processed text files and metadata into **LangChain `Document` objects**.

---

### 4. Vector Index Creation

```
src/build_index.py
```

Steps:

1. Split documents into chunks
2. Generate embeddings using:

```
sentence-transformers/all-MiniLM-L6-v2
```

3. Store embeddings in a **FAISS vector database**

The index is saved locally:

```
storage/faiss_index/
```

---

### 5. Retrieval Test

```
src/test_retrieval.py
```

Runs a simple similarity search to verify that the vector index works.

---

### 6. Local Chat Interface

```
src/chat_ollama.py
```

Provides a command-line chat interface.

Workflow:

1. User asks a question
2. FAISS retrieves top-k relevant chunks
3. Context is sent to a local Ollama model
4. Model generates an answer grounded in the retrieved text
5. Sources are printed alongside the answer

---

# Repository Structure

```
NeuroRag/
│
├── src/
│   ├── extract_pdfs.py        # PDF → TXT conversion
│   ├── create_metadata.py     # Generate metadata table
│   ├── load_documents.py      # Load documents with metadata
│   ├── build_index.py         # Build FAISS vector index
│   ├── test_retrieval.py      # Retrieval smoke test
│   └── chat_ollama.py         # CLI chat interface
│
├── requirements.txt
├── .gitignore
└── README.md
```

---

# Data and Privacy

This repository **does NOT include**:

* research papers
* PDFs
* extracted text
* vector indexes

The following folders are **ignored by Git**:

```
data/
storage/
.venv/
```

This ensures:

* your research library remains **private**
* the repository stays **lightweight**

---

# Installation

Clone the repository:

```
git clone https://github.com/fallahtp/NeuroRag.git
cd NeuroRag
```

Create a virtual environment:

```
python -m venv .venv
```

Activate it:

Windows:

```
.venv\Scripts\activate
```

Install dependencies:

```
pip install -r requirements.txt
```

---

# Running the Pipeline

### 1. Extract PDFs

Place your papers inside:

```
data/raw/
```

Then run:

```
python src/extract_pdfs.py
```

---

### 2. Create metadata

```
python src/create_metadata.py
```

---

### 3. Build vector index

```
python src/build_index.py
```

---

### 4. Test retrieval

```
python src/test_retrieval.py
```

---

### 5. Start the chat assistant

Make sure **Ollama is installed and running**, then run:

```
python src/chat_ollama.py
```

---

# Future Improvements

Planned improvements for the project:

* Better metadata-aware retrieval
* Hybrid search (vector + keyword)
* Improved prompt engineering
* Web interface
* Integration with NEURON modeling documentation
* Research paper summarization
* Code retrieval for computational neuroscience

---

# Tech Stack

* Python
* LangChain
* FAISS
* HuggingFace embeddings
* Ollama (local LLM)
* PyPDF

---

# Author

Mahdi Fallahtaherpazir
PhD Researcher —  Neuroscience
Medical University of Innsbruck

---

# License

MIT License
