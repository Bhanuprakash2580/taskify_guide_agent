A multi-agent AI system to manage tasks, notes, and workflow coordination in a Cloud environment using Google Cloud Datastore, FastAPI, and MCP (Multi-Agent Coordination Platform).

The system allows users to interact with a workspace assistant that can create tasks, mark them complete, save notes, and summarize outputs using multiple coordinated AI agents.

Features
Multi-agent orchestration using SequentialAgent and sub-agents.
Task management:
Add, list, and complete tasks.
Note management:
Add and list notes.
Automatic formatting and summarization of outputs.
API-based system using FastAPI for easy integration.
Logs integration with Google Cloud Logging.
Persistent storage using Google Cloud Datastore.
Technologies
Python 3.10+
FastAPI – Web API framework
uvicorn – ASGI server
MCP / Google ADK – Multi-agent orchestration
Google Cloud Datastore – Database for tasks and notes
Google Cloud Logging – Centralized logging
dotenv – Environment variable management
Setup Instructions
1. Clone the repository
git clone <your-repo-url>
cd taskify_guide_agent
2. Set up environment

Create a .env file with the following variables:

MODEL=gemini-1.5-pro
PROJECT_ID=
SA_NAME=
3. Install dependencies
pip install -r requirements.txt

Requirements include FastAPI, uvicorn, google-cloud-datastore, google-cloud-logging, dotenv, and MCP libraries.

4. Set up Google Cloud

Enable required services:

gcloud services enable \
  run.googleapis.com \
  artifactregistry.googleapis.com \
  cloudbuild.googleapis.com \
  aiplatform.googleapis.com \
  compute.googleapis.com

Create and grant IAM roles to your service account:

gcloud iam service-accounts create $SA_NAME \
    --display-name="Service Account for Workspace"

export SERVICE_ACCOUNT=$SA_NAME@${PROJECT_ID}.iam.gserviceaccount.com

gcloud projects add-iam-policy-binding $PROJECT_ID \
    --member="serviceAccount:$SERVICE_ACCOUNT" \
    --role="roles/aiplatform.user"
5. Run the server
python main.py

By default, it runs on http://0.0.0.0:8080.

API Endpoints
POST /api/v1/workspace/chat

Request Body:

{
  "prompt": "Plan my workday and prepare for the team meeting"
}

Response Example:

{
  "status": "success",
  "reply": "Here’s a suggested plan for your day: ...\nTasks and notes have been added."
}

The assistant generates tasks, notes, and summaries dynamically.
