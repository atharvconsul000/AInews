# 📰 AI News Platform

An automated, highly-personalized AI-driven news aggregation platform that completely eliminates information overload. 

This platform scrapes global news feeds, allows users to track hyper-specific keywords, ranks articles via community voting, and generates a **personalized daily email newsletter** automatically summarized by AI!


## ✨ Core Features

* **🤖 Personalized AI Newsletter:** The core engine! Add custom topics (e.g., "Quantum Computing", "OpenAI") and custom RSS sources. Every morning at 8:00 AM, a background Cron Job uses HuggingFace Sentence Transformers to semantically filter the news, ranks the most relevant clusters, and uses an LLM to write and email you a personalized summary.
* **🌍 Real-Time Global Feed:** Browse the top articles from globally trusted sources in real-time. Features a Reddit-style Upvote/Downvote ecosystem and nested community discussion threads.
* **📈 Community Trending:** The platform actively monitors the custom sources added by users and dynamically generates "Trending Topic" brackets to help you discover new feeds.
* **🛡️ Master Admin Dashboard:** Dedicated admin portal to view platform-wide analytics, manage global feed sources, and force-trigger the scraping pipelines.
* **⚡ Optimistic UI:** The React frontend utilizes optimistic state updates for instantaneous vote toggling and comment editing without waiting for database round-trips.

## 🛠️ Tech Stack

### Frontend
* **React + Vite:** Lightning fast frontend tooling.
* **Lucide React:** Beautiful, consistent iconography.
* **Supabase Auth:** Secure JWT-based user authentication.

### Backend
* **FastAPI:** High-performance async Python backend.
* **SQLAlchemy + SQLite:** Relational database ORM for managing users, votes, comments, and articles.
* **HuggingFace:** `sentence-transformers` for semantic similarity mapping and HuggingFace Inference API for the final Newsletter LLM generation.
* **BeautifulSoup4 & Feedparser:** Robust scraping pipeline.

## 🚀 Getting Started

### Prerequisites
1. Python 3.9+
2. Node.js v18+
3. A free [Supabase](https://supabase.com/) account for Authentication.
4. A free [HuggingFace](https://huggingface.co/) Access Token.
5. An SMTP Email account (e.g. Gmail App Password) to send the newsletters.

### 1. Backend Setup
```bash
# Clone the repository
git clone https://github.com/atharvconsul000/AInews.git
cd AInews

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`

# Install dependencies
pip install -r requirements.txt

# Configure Environment Variables
cp .env.template .env
# Edit .env with your SUPABASE_URL, HF_TOKEN, and SMTP details!

# Start the FastAPI Server
uvicorn app.main:app --reload
```

### 2. Frontend Setup
```bash
cd frontend

# Install Node modules
npm install

# Configure Frontend Environment Variables
# Create a .env file in the frontend folder with your Supabase keys:
# VITE_SUPABASE_URL=your_url
# VITE_SUPABASE_ANON_KEY=your_key

# Start the React Dev Server
npm run dev
```

### 3. Seed Mock Data (Optional)
To test the UI without waiting for organic users, you can seed the database with mock community members, trending URLs, and Reddit-style discussion threads:
```bash
curl http://localhost:8000/api/seed
```

## 📜 Architecture Overview

- **`app/api/routes.py`**: The central nervous system. Handles all CRUD operations for user preferences, global voting, and triggers the AI generation pipeline.
- **`app/services/scrapers/`**: Ingests unstructured RSS feeds, standardizes the metadata, and safely merges them into the SQLite database.
- **`app/services/ai_engine.py`**: The intelligence layer. Converts user keywords into vector embeddings, computes Cosine Similarity against the daily news dump, clusters the top matches, and prompts the LLM to write the final newsletter copy.
- **`app/main.py`**: The FastAPI entry point. Also houses the asynchronous background Cron Job that fires the daily emails.

## 🤝 Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## 📝 License
[MIT](https://choosealicense.com/licenses/mit/)
