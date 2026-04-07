# 🤖 AI Assistant

An intelligent AI Assistant built to automate tasks, answer queries, and assist users using modern AI capabilities.

## 🚀 Features

- 💬 Natural language conversation
- ⚡ Fast API using FastAPI
- ☁️ Deployable on Google Cloud Run
- 🔗 Modular agent architecture
- 🧠 AI-powered responses
- 📦 Easy to extend tools

## 🛠️ Tech Stack

- Python
- FastAPI
- Google ADK
- Cloud Run
- Uvicorn
- Pydantic

## 📂 Project Structure

```
.
├── main.py
├── requirements.txt
├── agent/
├── tools/
├── .env
└── README.md
```

## ⚙️ Installation

```bash
git clone https://github.com/Bhanuprakash2580/taskify_guide_agent.git
cd taskify_guide_agent
pip install -r requirements.txt
```

## ▶️ Run Locally

```bash
uvicorn main:app --reload
```

## ☁️ Deploy to Cloud Run

```bash
uvx --from google-adk==1.14.0 \
adk deploy cloud_run \
  --project=$PROJECT_ID \
  --region=us-central1 \
  --service_name=ai-assistant \
  --with_ui \
  . \
  -- \
  --service-account=$SERVICE_ACCOUNT
```

## 🔐 Environment Variables

Create a `.env` file:

```env
PROJECT_ID=your-project-id
GOOGLE_APPLICATION_CREDENTIALS=path-to-key.json
```

## 📌 Usage

1. Start the server
2. Open the provided URL
3. Chat with the AI assistant

## 🏗️ Architecture Diagram

```
┌─────────────────────────────────────────────────────────┐
│                        Client                           │
│                  (Browser / API Client)                 │
└─────────────────────┬───────────────────────────────────┘
                      │ HTTP Request
                      ▼
┌─────────────────────────────────────────────────────────┐
│                    FastAPI Server                       │
│                     (main.py)                           │
│                                                         │
│   ┌─────────────────────────────────────────────────┐   │
│   │               Agent Layer                      │   │
│   │              (agent/)                          │   │
│   │                                                 │   │
│   │   ┌──────────────┐    ┌──────────────────────┐  │   │
│   │   │  Google ADK  │    │    Tools Registry    │  │   │
│   │   │   (LLM Core) │◄──►│      (tools/)        │  │   │
│   │   └──────────────┘    └──────────────────────┘  │   │
│   └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────┐
│              Google Cloud Platform                      │
│                                                         │
│   ┌──────────────────┐    ┌───────────────────────┐     │
│   │   Cloud Run      │    │   Google AI Models    │     │
│   │  (Hosting)       │◄──►│   (Gemini / Vertex)   │     │
│   └──────────────────┘    └───────────────────────┘     │
└─────────────────────────────────────────────────────────┘
```

**Flow:**
1. Client sends a natural language query via HTTP.
2. FastAPI routes the request to the Agent Layer.
3. The Agent uses Google ADK to process the query and invoke relevant tools.
4. Tools execute tasks (search, compute, respond) and return results.
5. The AI model generates a final response sent back to the client.

---

## 🔌 API Endpoints & Docs

Once the server is running, interactive API docs are available at:

- **Swagger UI:** `http://localhost:8000/docs`
- **ReDoc:** `http://localhost:8000/redoc`

### Core Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/` | Health check — returns server status |
| `POST` | `/chat` | Send a message and receive an AI response |
| `GET` | `/history` | Retrieve conversation history |
| `DELETE` | `/history` | Clear conversation history |

### Example Request

```bash
curl -X POST "http://localhost:8000/chat" \
  -H "Content-Type: application/json" \
  -d '{"message": "What tasks can you help me with?"}'
```

### Example Response

```json
{
  "status": "success",
  "response": "I can help you automate tasks, answer questions, search for information, and much more! What would you like to do?",
  "session_id": "abc123xyz"
}
```

---

## 🧪 Testing Instructions

### 1. Install Test Dependencies

```bash
pip install pytest pytest-asyncio httpx
```

### 2. Run All Tests

```bash
pytest tests/ -v
```

### 3. Run with Coverage Report

```bash
pytest tests/ --cov=. --cov-report=term-missing
```

### 4. Test a Specific Module

```bash
# Test agent logic only
pytest tests/test_agent.py -v

# Test API endpoints only
pytest tests/test_api.py -v
```

### 5. Manual API Test

```bash
# Start the server
uvicorn main:app --reload

# In another terminal, send a test request
curl -X POST "http://localhost:8000/chat" \
  -H "Content-Type: application/json" \
  -d '{"message": "Hello, who are you?"}'
```

### Sample Test Structure

```
tests/
├── test_api.py        # FastAPI endpoint tests
├── test_agent.py      # Agent logic tests
├── test_tools.py      # Tool functionality tests
└── conftest.py        # Shared fixtures
```

---

## 🛠️ Troubleshooting / FAQ

### ❓ The server won't start — `ModuleNotFoundError`

Make sure all dependencies are installed:

```bash
pip install -r requirements.txt
```

If using a virtual environment, ensure it's activated:

```bash
source venv/bin/activate   # Linux/macOS
venv\Scripts\activate      # Windows
```

---

### ❓ `GOOGLE_APPLICATION_CREDENTIALS` not found

Ensure your `.env` file exists and contains the correct path:

```env
GOOGLE_APPLICATION_CREDENTIALS=path/to/your-service-account-key.json
```

Also verify the key file exists at the specified path and has the correct IAM permissions.

---

### ❓ Cloud Run deployment fails

- Confirm `$PROJECT_ID` and `$SERVICE_ACCOUNT` environment variables are set.
- Make sure the service account has the `roles/run.admin` and `roles/aiplatform.user` roles.
- Check your GCP project has Cloud Run and Vertex AI APIs enabled:

```bash
gcloud services enable run.googleapis.com aiplatform.googleapis.com
```

---

### ❓ Slow or no AI responses

- Check your Google ADK quota and billing in the GCP Console.
- Ensure the correct region is set (`us-central1` recommended).
- Try restarting the server and clearing session history.

---

### ❓ How do I add a new tool?

Create a new Python file inside the `tools/` directory and register it in the agent's tool registry. Follow the modular pattern of existing tools for consistency.

---

## 🗺️ Roadmap & Future Features

### ✅ Current (v1.0)
- [x] Natural language conversation
- [x] FastAPI backend
- [x] Google ADK integration
- [x] Cloud Run deployment
- [x] Modular tool architecture

### 🔄 In Progress (v1.1)
- [ ] Persistent conversation memory across sessions
- [ ] Multi-turn context awareness improvements
- [ ] Streaming responses via WebSocket

### 🔮 Planned (v2.0)
- [ ] 🔐 User authentication & session management
- [ ] 📊 Admin dashboard with usage analytics
- [ ] 🧩 Plugin marketplace for community tools
- [ ] 🌍 Multi-language support
- [ ] 📁 File upload & document analysis
- [ ] 🔔 Webhook & notification integrations
- [ ] 📱 Mobile-friendly UI

> Have a feature request? [Open an issue](https://github.com/Bhanuprakash2580/taskify_guide_agent/issues) on GitHub!

---

## 🙏 Acknowledgements & Credits

This project was made possible thanks to the following technologies and communities:

- **[Google ADK (Agent Development Kit)](https://cloud.google.com/vertex-ai)** — for powering the AI agent core with Gemini models.
- **[FastAPI](https://fastapi.tiangolo.com/)** — for the lightning-fast, modern Python web framework.
- **[Uvicorn](https://www.uvicorn.org/)** — for the high-performance ASGI server.
- **[Pydantic](https://docs.pydantic.dev/)** — for robust data validation and settings management.
- **[Google Cloud Run](https://cloud.google.com/run)** — for seamless serverless deployment at scale.
- **Open Source Community** — for the countless libraries and tools that make projects like this possible.

Special thanks to all contributors and early testers who helped shape this project. 💙

---

## 🤝 Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## 📄 License

[MIT License](LICENSE)
