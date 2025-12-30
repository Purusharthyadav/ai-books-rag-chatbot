# AI Book Chatbot with RAG Pipeline

An intelligent chatbot that answers questions about Artificial Intelligence using content from O'Reilly AI books, powered by Groq LLM and advanced RAG (Retrieval-Augmented Generation) with hybrid retrieval, cross-encoder reranking, and semantic chunking.

## 📚 Features

- **Hybrid Retrieval System**: Combines semantic search (embeddings) and lexical search (BM25) using Reciprocal Rank Fusion (RRF)
- **Cross-Encoder Reranking**: Improves result quality by reranking top candidates with a cross-encoder model
- **Semantic Chunking**: Intelligent text chunking with overlap to preserve context and meaning
- **Accurate Citations**: Every answer includes inline numbered citations [1], [2] with book title, chapter, and page
- **Image Support**: Automatically extracts and displays relevant diagrams and figures from books
- **Multi-Book Reasoning**: Synthesizes information across 6 O'Reilly AI/ML books
- **Advanced Filtering**: Filter results by book, chapter, or page range
- **Modern UI**: React + TypeScript frontend with TanStack Query/Table
- **Fast API Backend**: Built with FastAPI for high performance

## 📖 Books Included

1. AI Engineering
2. Applied Machine Learning and AI for Engineers
3. Hands-On Large Language Models
4. Hands-On Machine Learning with Scikit-Learn and PyTorch
5. LLM Engineers Handbook
6. NLP with Transformer Models

## 🏗️ Project Structure

```
AI Book RAG/
├── Books_pdf/                          # Source PDF files
├── notebooks/                          # Jupyter notebooks for each process
│   ├── 01_pdf_ingestion.ipynb         # Extract text, images, metadata
│   ├── 02_text_chunking.ipynb         # Semantic chunking with overlap
│   ├── 02_text_chunking_semantic.ipynb # Advanced semantic chunking
│   ├── 03_embedding_vectordb.ipynb    # Generate embeddings, setup Chroma
│   └── 04_rag_pipeline_test.ipynb     # Test hybrid RAG pipeline
├── backend/                            # FastAPI backend
│   ├── app/
│   │   ├── main.py                    # FastAPI app entry point
│   │   ├── models.py                  # Pydantic models
│   │   ├── rag_engine.py              # RAG pipeline logic
│   │   └── routes/
│   │       └── chat.py                # Chat endpoints
│   └── requirements.txt
├── frontend/                           # React + TypeScript frontend
│   ├── src/
│   │   ├── components/                # React components
│   │   ├── hooks/                     # TanStack Query hooks
│   │   └── App.tsx
│   └── package.json
├── data/                               # Processed data
│   ├── extracted/                     # Extracted text and images
│   └── chunks/                        # Chunked text with metadata
├── chroma_db/                         # Vector database storage
├── requirements.txt                   # Python dependencies
├── .env.example                       # Environment variables template
└── README.md
```

## 🚀 Setup Instructions

### Prerequisites

- Python 3.10+
- Node.js 18+
- Groq API Key (free tier available at https://console.groq.com)

### Backend Setup

1. **Clone and navigate to the project**:
   ```bash
   cd "d:\AI Book RAG"
   ```

2. **Create virtual environment**:
   ```bash
   python -m venv venv
   venv\Scripts\activate  # Windows
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables**:
   ```bash
   copy .env.example .env
   # Edit .env and add your GROQ_API_KEY
   ```

5. **Run the notebooks in order**:
   - `01_pdf_ingestion.ipynb` - Extract content from PDFs
   - `02_text_chunking.ipynb` or `02_text_chunking_semantic.ipynb` - Semantic chunking with overlap
   - `03_embedding_vectordb.ipynb` - Generate embeddings and populate vector DB
   - `04_rag_pipeline_test.ipynb` - Test the hybrid RAG pipeline

6. **Start the backend**:
   ```bash
   cd backend
   uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
   ```

### Frontend Setup

1. **Navigate to frontend**:
   ```bash
   cd frontend
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Start development server**:
   ```bash
   npm run dev
   ```

4. **Open browser**:
   ```
   http://localhost:3000
   ```

## 📝 Usage

1. Open the web interface
2. Type your AI-related question in the chat input
3. Receive answers with:
   - Inline citations [Book Title, Chapter, Page]
   - Relevant diagrams/images
   - Source snippets (optional)

### Example Questions

- "What is transfer learning and how does it work?"
- "Explain the transformer architecture"
- "What are the best practices for fine-tuning LLMs?"
- "How do I implement a neural network with PyTorch?"

## 🔧 Configuration

### RAG Parameters (in `.env`)

- `TOP_K_RESULTS`: Number of relevant chunks to retrieve (default: 5)
- `CHUNK_SIZE`: Size of text chunks in characters (default: 1000)
- `CHUNK_OVERLAP`: Overlap between chunks (default: 200)
- `TEMPERATURE`: LLM temperature for response generation (default: 0.1)
- `GROQ_MODEL`: LLM model to use (default: llama-3.3-70b-versatile)

### Retrieval Configuration

**Retrieval Methods:**
- `hybrid` (default): Combines semantic + lexical search with RRF
- `semantic`: Pure vector similarity search
- `lexical`: Pure BM25 keyword matching

**Reranking:**
- Enabled by default using cross-encoder model
- Improves top-k result quality significantly
- Model: `cross-encoder/ms-marco-MiniLM-L-6-v2`

### Embedding Model

Default: `sentence-transformers/all-MiniLM-L6-v2` (384 dimensions, fast)

Alternatives:
- `sentence-transformers/all-mpnet-base-v2` (768 dimensions, more accurate)
- `BAAI/bge-small-en-v1.5` (384 dimensions, optimized for retrieval)

### Reranker Model

Default: `cross-encoder/ms-marco-MiniLM-L-6-v2` (fast, accurate)

Alternatives:
- `cross-encoder/ms-marco-MiniLM-L-12-v2` (better quality)
- `cross-encoder/ms-marco-electra-base` (highest quality)

## 🚢 Deployment

### Backend (Render/Railway)

1. Push code to GitHub
2. Connect repository to Render/Railway
3. Set environment variables
4. Deploy with auto-build

### Frontend (Vercel)

1. Push frontend code to GitHub
2. Import project to Vercel
3. Configure build settings:
   - Build Command: `npm run build`
   - Output Directory: `dist`
4. Deploy

### Vector Database

- **Development**: Local Chroma DB
- **Production**: Consider Qdrant Cloud (free tier) or persist Chroma to volume

## 🛠️ Tech Stack

### Backend
- **Framework**: FastAPI
- **LLM**: Groq (Llama 3.3 70B Versatile)
- **Embeddings**: Sentence Transformers (all-MiniLM-L6-v2)
- **Vector DB**: ChromaDB
- **Hybrid Search**: BM25 (rank-bm25) + Semantic Search
- **Reranking**: Cross-Encoder (ms-marco-MiniLM-L-6-v2)
- **PDF Processing**: PyMuPDF, PDFPlumber
- **Text Processing**: LangChain for semantic chunking

### Frontend
- **Framework**: React + TypeScript
- **Build Tool**: Vite
- **State Management**: TanStack Query
- **Table Display**: TanStack Table
- **Styling**: Tailwind CSS
- **Icons**: Lucide React

## 📊 Advanced Features

### Retrieval & Search
- ✅ **Hybrid Retrieval**: Combines semantic (embeddings) and lexical (BM25) search
- ✅ **Reciprocal Rank Fusion (RRF)**: Merges ranked results from multiple retrieval methods
- ✅ **Cross-Encoder Reranking**: Reranks top candidates for improved accuracy
- ✅ **Metadata Filtering**: Filter by book, chapter, or page range
- ✅ **Similarity Thresholding**: Filter out low-quality results

### Text Processing
- ✅ **Semantic Chunking**: Intelligent text splitting with context preservation
- ✅ **Chunk Overlap**: 200-character overlap to maintain continuity
- ✅ **Chunk Consolidation**: Merges chunks from same page to reduce redundancy

### Answer Generation
- ✅ **Multi-book reasoning**: Synthesizes information across multiple sources
- ✅ **Numbered Citations**: Academic-style inline citations [1], [2], [3]
- ✅ **Citation Deduplication**: Each unique source gets one reference number
- ✅ **Intent Detection**: Handles greetings and technical queries appropriately

### Visual & UI
- ✅ **Image Extraction**: Automatically extracts diagrams and figures from PDFs
- ✅ **Image Relevance Scoring**: Confidence-based image matching to queries
- ✅ **Source Snippet Viewing**: Expandable source text with metadata
- ✅ **Query Caching**: TanStack Query for fast repeat queries
- ✅ **Performance Metrics**: Tracks retrieval time, LLM time, and source count

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is for educational purposes. Please respect the copyright of the O'Reilly books.

## 🙏 Acknowledgments

- O'Reilly Media for the excellent AI books
- Groq for providing fast LLM inference
- ChromaDB for the vector database
- TanStack for Query and Table libraries
