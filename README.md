# RAG API with FastAPI

A simple **Retrieval-Augmented Generation (RAG)** API implementation built with FastAPI, ChromaDB, and Ollama. This project demonstrates how to build a question-answering system that retrieves relevant context from a knowledge base and generates answers using a language model.

> **Note:** This is an educational project created for learning purposes.

## 🎯 What is RAG?

Retrieval-Augmented Generation (RAG) is a technique that combines:
- **Retrieval**: Finding relevant information from a knowledge base
- **Augmentation**: Using that information as context
- **Generation**: Producing answers based on the retrieved context

This approach helps language models provide more accurate and contextually relevant answers by grounding their responses in specific documents.

## 🏗️ Architecture

```
User Query → FastAPI Endpoint → ChromaDB (Vector Search) → Ollama (LLM) → Response
```

1. **FastAPI**: Web framework providing the REST API
2. **ChromaDB**: Vector database for storing and retrieving document embeddings
3. **Ollama**: Local LLM inference engine (using tinyllama model)

## 📁 Project Structure

```
.
├── app.py          # FastAPI application with query endpoint
├── embed.py        # Script to embed documents into ChromaDB
├── k8s.txt         # Sample knowledge base document
├── db/             # ChromaDB persistent storage directory
└── README.md       # This file
```

## 🚀 Prerequisites

Before you begin, ensure you have the following installed:

- **Python 3.8+**
- **Ollama** - [Installation Guide](https://ollama.ai/)
- **tinyllama model** - Pull it using: `ollama pull tinyllama`

## 📦 Installation

1. **Clone or download this repository**

2. **Install Python dependencies:**
   ```bash
   pip install fastapi uvicorn chromadb ollama
   ```

3. **Pull the Ollama model:**
   ```bash
   ollama pull tinyllama
   ```

## 🔧 Setup

1. **Embed your documents:**
   ```bash
   python embed.py
   ```
   This will:
   - Read the content from `k8s.txt`
   - Create embeddings and store them in ChromaDB
   - Create a collection named "docs" in the `./db` directory

2. **Start the FastAPI server:**
   ```bash
   uvicorn app:app --reload
   ```
   The API will be available at `http://localhost:8000`

## 📖 Usage

### Query the API

Send a POST request to the `/query` endpoint:

```bash
curl -X POST "http://localhost:8000/query?q=What is Kubernetes?"
```

Or using Python:

```python
import requests

response = requests.post(
    "http://localhost:8000/query",
    params={"q": "What is Kubernetes?"}
)
print(response.json())
```

### Response Format

```json
{
  "answer": "Kubernetes is a container orchestration platform..."
}
```

## 🔍 How It Works

1. **Embedding Phase** (`embed.py`):
   - Reads documents from `k8s.txt`
   - ChromaDB automatically creates embeddings
   - Stores them in a persistent vector database

2. **Query Phase** (`app.py`):
   - Receives a user query
   - Searches ChromaDB for the most relevant document chunk (top 1 result)
   - Passes the context and query to Ollama's tinyllama model
   - Returns the generated answer

## 🛠️ Customization

### Adding Your Own Documents

1. Replace or add content to `k8s.txt` (or modify `embed.py` to read different files)
2. Run `python embed.py` again to update the embeddings

### Using Different Models

In `app.py`, change the model name:
```python
answer = ollama.generate(
    model="llama2",  # Change to your preferred model
    prompt=f"Context:\n{context}\n\nQuestion: {q}\n\nAnswer clearly and concisely:"
)
```

Make sure to pull the model first: `ollama pull llama2`

### Adjusting Retrieval Results

Modify the `n_results` parameter in `app.py`:
```python
results = collection.query(query_texts=[q], n_results=3)  # Get top 3 results
```

## 📚 API Documentation

Once the server is running, visit:
- **Swagger UI**: `http://localhost:8000/docs`
- **ReDoc**: `http://localhost:8000/redoc`

## 🎓 Learning Objectives

This project demonstrates:
- Building REST APIs with FastAPI
- Vector database operations with ChromaDB
- RAG implementation patterns
- Integration with local LLMs via Ollama
- Document embedding and retrieval

## ⚠️ Limitations

- Uses a small model (tinyllama) for educational purposes
- Simple retrieval strategy (top-1 result)
- No advanced chunking or preprocessing
- Single document knowledge base

## 🔮 Future Enhancements

- Support for multiple documents
- Advanced chunking strategies
- Better prompt engineering
- Support for multiple query results
- Authentication and rate limiting
- Web UI for easier interaction

## 📝 License

This is an educational project. Feel free to use and modify as needed for learning purposes.

## 🙏 Acknowledgments

Built with:
- [FastAPI](https://fastapi.tiangolo.com/)
- [ChromaDB](https://www.trychroma.com/)
- [Ollama](https://ollama.ai/)

---

**Happy Learning! 🚀**
