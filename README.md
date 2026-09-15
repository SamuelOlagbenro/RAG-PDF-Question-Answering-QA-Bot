# RAG-PDF-Question-Answering-QA-Bot

A Retrieval-Augmented Generation (RAG) application that enables users to upload PDF documents and ask natural language questions about their content. The bot leverages IBM's Granite models and LangChain to deliver accurate, context-aware answers.

![Python](https://img.shields.io/badge/Python-3.11-blue.svg)
![LangChain](https://img.shields.io/badge/LangChain-framework-green.svg)
![Gradio](https://img.shields.io/badge/Gradio-UI-orange.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)
![Production Ready](https://img.shields.io/badge/Production--Ready-Yes-brightgreen)

---

## 🎯 Project Overview

This project demonstrates a real-world implementation of RAG concepts by building an intelligent document Question Answering system. Users can upload any PDF document and ask natural language questions; the application retrieves relevant context and generates accurate answers using a large language model.

**Key Concepts Implemented:**
- Document loading and preprocessing
- Semantic text chunking
- Vector embeddings and similarity search
- Retrieval-Augmented Generation (RAG)
- Web interface with Gradio

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                  Gradio Web Interface                    │
├─────────────────────────────────────────────────────────┤
│                                                           │
│  ┌──────────────┐          ┌──────────────┐             │
│  │  PDF Upload  │          │  Query Input │             │
│  └──────┬───────┘          └──────┬───────┘             │
│         │                         │                      │
│         └────────────┬────────────┘                     │
│                      │                                   │
├──────────────────────▼──────────────────────────────────┤
│            RetrievalQA Chain (LangChain)                │
├──────────────────────┬──────────────────────────────────┤
│         ┌────────────▼──────────────┐                   │
│         │   Document Processor      │                   │
│         │  • PDF Loader             │                   │
│         │  • Text Splitter (500px)  │                   │
│         └────────────┬──────────────┘                   │
│                      │                                   │
│         ┌────────────▼──────────────┐                   │
│         │  Embedding Generator      │                   │
│         │  (Granite 278M)           │                   │
│         └────────────┬──────────────┘                   │
│                      │                                   │
│         ┌────────────▼──────────────┐                   │
│         │   Chroma Vector Store     │                   │
│         │  (Semantic Similarity)    │                   │
│         └────────────┬──────────────┘                   │
│                      │                                   │
│         ┌────────────▼──────────────┐                   │
│         │   LLM Inference           │                   │
│         │  (Granite 4H Small)       │                   │
│         └────────────┬──────────────┘                   │
│                      │                                   │
├──────────────────────▼──────────────────────────────────┤
│                 Generated Answer                         │
└─────────────────────────────────────────────────────────┘
```

---

## 🛠️ Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **LLM Framework** | LangChain | Orchestrate RAG pipeline |
| **Language Model** | IBM Granite 4H Small | Generate contextual answers |
| **Embeddings** | IBM Granite Embedding 278M | Semantic text representation |
| **Vector Database** | Chroma | Store & retrieve embeddings |
| **Document Loader** | PyPDFLoader | Extract text from PDFs |
| **Text Processing** | RecursiveCharacterTextSplitter | Intelligent chunking |
| **UI Framework** | Gradio | Interactive web interface |
| **Infrastructure** | IBM Watsonx AI | Cloud-based LLM & embedding APIs |

---

## 📋 Features

✅ **PDF Upload** - Support for multiple PDF documents  
✅ **Smart Text Chunking** - RecursiveCharacterTextSplitter with 500-token chunks and 50-token overlap  
✅ **Semantic Search** - Vector embeddings for accurate context retrieval  
✅ **RAG Pipeline** - Context-aware answer generation using retrieved documents  
✅ **Web Interface** - User-friendly Gradio application  
✅ **Real-time Processing** - Instant question-answering on uploaded content  

---

## 🚀 Getting Started

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/pdf-qa-bot.git
   cd pdf-qa-bot
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up IBM Watsonx credentials**
   ```bash
   export IBM_CLOUD_API_KEY="your_api_key"
   ```

### Usage

1. **Run the application**
   ```bash
   python qabot.py
   ```

2. **Access the web interface**
   - Open your browser to `http://localhost:7862`
   - The app will also generate a public shareable link

3. **Upload and Query**
   - Click "Upload PDF Document" and select your PDF
   - Enter a question in the textbox
   - Click "Submit" to get answers
   - Example query: *"What this paper is talking about?"*

---

## 📁 Project Structure

```
pdf-qa-bot/
├── qabot.py                 # Main application (Gradio + RAG pipeline)
├── requirements.txt         # Python dependencies
├── README.md               # This file
└── docs/
    └── architecture.md     # Detailed architecture explanation
```

---

## 🔧 Core Functions

### `get_llm()`
Initializes the IBM Granite 4H Small language model with temperature=0.7 for balanced creativity and coherence.

### `document_loader(file)`
Loads PDF documents using PyPDFLoader from LangChain community library.

### `text_splitter(data)`
Splits documents into 500-token chunks with 50-token overlap using RecursiveCharacterTextSplitter for semantic coherence.

### `watsonx_embedding()`
Generates multilingual text embeddings using IBM Granite Embedding 278M model.

### `vector_database(chunks)`
Creates and stores embeddings in a Chroma vector database for semantic similarity search.

### `retriever(file)`
Orchestrates the document processing pipeline and returns a retriever object.

### `retriever_qa(file, query)`
Implements the complete RAG chain:
1. Retrieves relevant document chunks
2. Passes context to LLM
3. Generates answer with source references

---

## 🧠 RAG Implementation Details

This project demonstrates a complete RAG pipeline:

1. **Document Ingestion** → PDF files are loaded and parsed
2. **Chunking Strategy** → Text is split intelligently to preserve semantic meaning
3. **Embedding Generation** → Each chunk is converted to a dense vector representation
4. **Storage** → Embeddings indexed in Chroma for fast retrieval
5. **Retrieval** → User queries are embedded and matched against stored chunks
6. **Generation** → Retrieved context augments the prompt sent to the LLM
7. **Output** → Answer generated with source document references

**Why RAG?** RAG combines the strengths of retrieval-based and generation-based approaches:
- ✅ Answers grounded in actual document content
- ✅ Reduced hallucinations compared to LLM-only approaches
- ✅ Up-to-date information without retraining
- ✅ Transparency through source attribution

---

## ⚙️ Configuration

### Model Parameters

**LLM (Granite 4H Small):**
- Temperature: 0.7 (balanced between deterministic and creative)
- Max Tokens: 200 (concise answers)

**Embeddings (Granite 278M):**
- Temperature: 0.0 (deterministic)
- Max Tokens: 100

**Text Chunking:**
- Chunk Size: 500 tokens
- Overlap: 50 tokens (maintains context continuity)

### Gradio Interface

- **Input 1:** PDF file upload
- **Input 2:** Natural language question
- **Output:** Generated answer with reasoning

---

## 📊 Performance Considerations

| Aspect | Details |
|--------|---------|
| **Latency** | ~2-5 seconds per query (depends on document size) |
| **Throughput** | Single-threaded; suitable for interactive use |
| **Scalability** | Can handle PDFs up to tens of MB |
| **Accuracy** | Depends on document quality and query specificity |


---


## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 📚 References

- [LangChain Documentation](https://python.langchain.com/)
- [IBM Watsonx AI](https://www.ibm.com/watsonx)
- [Chroma Vector Database](https://www.trychroma.com/)
- [Gradio Documentation](https://www.gradio.app/)
- [RAG Papers and Resources](https://arxiv.org/abs/2005.11401)

---

## ✨ Acknowledgments

- IBM Skills Network for the project specification
- LangChain community for excellent documentation
- IBM Granite models for powerful open-source LLMs

---

## 📧 Contact

**Samuel Olagbenro** 
Data Scientist & AI/ML Engineer

