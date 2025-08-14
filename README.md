# Course Materials RAG System

A Retrieval-Augmented Generation (RAG) system designed to answer questions about course materials using semantic search and AI-powered responses.

## JDD Notes

Had to downgrade to Python 3.12 (in `pyproject.toml`, `.python-version`) to get `torch` to install.  All
due to my Intel-based Mac.

Setup my `.venv`:

```shell
python3.12 -m venv .venv
refresh
```

Needed to use latest `node` (`v22.18.0`):

```shell
nvm install --lts
nvm use --lts
nvm alias default node

nvm ls
```

Also, needed to downgrade numpy (`UserWarning: Failed to initialize NumPy: _ARRAY_API not found`):

```shell
 uv add "numpy==1.26.4"
 uv sync --reinstall
 ```

## Overview

This application is a full-stack web application that enables users to query course materials and receive intelligent, context-aware responses. It uses ChromaDB for vector storage, Anthropic's Claude for AI generation, and provides a web interface for interaction.


## Prerequisites

- Python 3.13 or higher
- uv (Python package manager)
- An Anthropic API key (for Claude AI)
- **For Windows**: Use Git Bash to run the application commands - [Download Git for Windows](https://git-scm.com/downloads/win)

## Installation

1. **Install uv** (if not already installed)
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

2. **Install Python dependencies**
   ```bash
   uv sync
   ```

3. **Set up environment variables**
   
   Create a `.env` file in the root directory:
   ```bash
   ANTHROPIC_API_KEY=your_anthropic_api_key_here
   ```

## Running the Application

### Quick Start

Use the provided shell script:
```bash
chmod +x run.sh
./run.sh
```

### Manual Start

```bash
cd backend
uv run uvicorn app:app --reload --port 8000
```

The application will be available at:
- Web Interface: `http://localhost:8000`
- API Documentation: `http://localhost:8000/docs`

