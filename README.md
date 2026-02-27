# 📰 News Intelligence Chatbot — RAG Pipeline

A production-style **Retrieval-Augmented Generation (RAG)** chatbot that fetches real-time news articles via NewsAPI, stores them in a vector database, and answers questions using a local LLM (Ollama). Built with LangChain, ChromaDB, FastAPI, and Streamlit.

---

## 🏗️ Architecture

```
NewsAPI → ingest.py → ChromaDB (vector store)
                              ↓
User Question → rag.py → similarity search → Ollama LLM → Answer
                              ↓
                         app.py (FastAPI) ← streamlit_app.py (UI)
```

---

## 📁 Project Structure

```
news_rag_chatbot/
├── ingest.py           # Fetch news, chunk, embed, store in ChromaDB
├── rag.py              # Retrieval + LLM answer generation
├── app.py              # FastAPI REST API
├── streamlit_app.py    # Streamlit chat UI
├── .env                # API keys (never commit this)
├── .env.example        # Template for .env
├── .gitignore
├── requirements.txt
└── README.md
```

---

## ⚙️ Setup & Installation

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/news-rag-chatbot.git
cd news-rag-chatbot
```

### 2. Create and activate virtual environment
```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Mac/Linux
source venv/bin/activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Set up environment variables
```bash
cp .env.example .env
```
Edit `.env` and add your NewsAPI key:
```
NEWS_API_KEY=your_key_here
```
Get a free API key at [https://newsapi.org](https://newsapi.org)

### 5. Install and start Ollama
Download from [https://ollama.com/download](https://ollama.com/download), then:
```bash
ollama pull llama3.2:1b
```
Ollama starts automatically on Windows after installation.

---

## 🚀 Running the Project

### Step 1 — Ingest news articles
```bash
python ingest.py
```
This fetches the latest Tesla news, chunks and embeds the articles, and saves them to the `db/` folder.

### Step 2 — Start the FastAPI backend
```bash
python app.py
```
API will be available at `http://localhost:8000`  
Interactive docs at `http://localhost:8000/docs`

### Step 3 — Launch the Streamlit UI
Open a new terminal, activate the venv, then:
```bash
streamlit run streamlit_app.py
```
Open `http://localhost:8501` in your browser.

---

## 🔌 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET    | `/`      | Health check |
| POST   | `/ask`   | Ask a question |
| POST   | `/clear` | Reset chat history and cache |

**Example request:**
```bash
curl -X POST http://localhost:8000/ask \
  -H "Content-Type: application/json" \
  -d '{"question": "What is Tesla latest news?"}'
```

**Example response:**
```json
{
  "question": "What is Tesla latest news?",
  "answer": "Tesla recently announced... \n\n📰 Sources: Reuters, Electrek"
}
```

---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|------------|
| News data | NewsAPI |
| Text splitting | LangChain RecursiveCharacterTextSplitter |
| Embeddings | HuggingFace `all-MiniLM-L6-v2` |
| Vector store | ChromaDB |
| LLM | Ollama (`llama3.2:1b`) |
| Backend API | FastAPI + Uvicorn |
| Frontend | Streamlit |

---

## 📦 Requirements

```
requests
python-dotenv
langchain-text-splitters
langchain-huggingface
langchain-community
langchain-chroma
langchain-core
langchain-ollama
chromadb
sentence-transformers
fastapi
uvicorn
streamlit
```

---

## 🔧 Configuration

You can change the news topic by passing a different query when running ingest:

```python
# In ingest.py, change the default query
ingest_documents(query="apple")  # or "AI", "bitcoin", etc.
```

---

## 📌 Notes

- The `db/` folder is created automatically after running `ingest.py`
- Re-run `ingest.py` any time to refresh the knowledge base with the latest articles
- The LLM only answers from retrieved context — it will say "I don't have enough information" if the answer isn't in the news articles
- Responses are cached per session to avoid redundant LLM calls

---

## 👤 Author

Built by **VICKY RANA**  
[LinkedIn](https://www.linkedin.com/in/vicky-rana-125084225/) · [GitHub](https://github.com/vickyrana123)
