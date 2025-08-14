# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Environment Setup
```bash
# Install dependencies (requires Python 3.12+ due to Intel Mac compatibility)
uv sync

# Set up environment variables
cp .env.example .env
# Edit .env to add your ANTHROPIC_API_KEY
```

### Running the Application
```bash
# Quick start using provided script
chmod +x run.sh
./run.sh

# Manual start (alternative)
cd backend && uv run uvicorn app:app --reload --port 8000
```

### Development Environment
- Python 3.12 (downgraded from 3.13 for Intel Mac torch compatibility)
- Uses `uv` for dependency management
- Requires `ANTHROPIC_API_KEY` environment variable
- Web interface available at http://localhost:8000
- API docs at http://localhost:8000/docs

## Architecture Overview

This is a **RAG (Retrieval-Augmented Generation) system** for querying course materials. The architecture follows a clear separation between data processing, vector storage, AI generation, and web presentation.

### Core Components Flow

```
User Query → FastAPI → RAG System → [Vector Search + AI Generation] → Response
```

### Key Architectural Components

**RAGSystem** (`rag_system.py`) - Central orchestrator that coordinates:
- `DocumentProcessor` - Processes course documents into structured chunks
- `VectorStore` - ChromaDB-based semantic search using sentence transformers
- `AIGenerator` - Anthropic Claude API integration with tool support
- `SessionManager` - Conversation history management
- `ToolManager` - Search tools for enhanced query processing

**Data Models** (`models.py`):
- `Course` - Contains title, instructor, lessons list
- `Lesson` - Lesson number, title, optional link
- `CourseChunk` - Text chunks with course/lesson metadata for vector storage

**Vector Storage Pattern**:
- Two ChromaDB collections: `course_catalog` (metadata) and `course_content` (chunks)
- Uses sentence transformers for embeddings (`all-MiniLM-L6-v2`)
- Semantic search returns relevant chunks with source attribution

**Session Management**:
- Stateful conversations with configurable history limits
- Session IDs track conversation context across API calls
- History pruning to maintain performance

### Frontend Integration

**Static File Serving**: FastAPI serves the frontend directly with development-friendly no-cache headers via custom `DevStaticFiles` class.

**API Endpoints**:
- `POST /api/query` - Main query processing with session management
- `GET /api/courses` - Course analytics and statistics

### Configuration

All settings centralized in `config.py`:
- Chunk size/overlap for document processing
- Vector search limits and embedding models
- Database paths and session history limits
- Model selection and API configuration

### Data Flow Pattern

1. **Document Ingestion**: Text files → `DocumentProcessor` → structured `Course`/`CourseChunk` objects
2. **Vector Storage**: Chunks → sentence transformers → ChromaDB collections
3. **Query Processing**: User query → vector search → relevant chunks → Claude API with context
4. **Response Generation**: Claude processes search results + conversation history → formatted response

### Key Design Patterns

- **Dependency Injection**: `config` object passed to all components
- **Factory Pattern**: Collections created via `_create_collection()` method
- **Strategy Pattern**: Different search strategies via `ToolManager`
- **Session State**: Managed separately from request processing
- **Error Boundary**: Exception handling at API endpoint level

### Intel Mac Compatibility Notes

The codebase includes specific version pins for Intel Mac compatibility:
- `torch==2.2.0` and `numpy==1.26.4` (see pyproject.toml)
- Python 3.12 requirement instead of 3.13
- These constraints are noted in README and pyproject.toml comments
- make sure to always use uv to manage dependencies
- don't list claude as a coauthor in commit messages