# Generative Content Variant Engine

A modular FastAPI-based system for generating personalized content variants (text & image) from base templates using LLMs and emotion-tone conditioning.  
This project demonstrates scalable prompt, image, and mood-based generation pipelines with mocked database and storage integrations.

---

## 🚀 Features
- Emotion & tone–conditioned prompt generation  
- Email & WhatsApp message variant generation  
- Brand moodboard creation from URLs  
- Mocked MongoDB + S3 (pre-signed URLs)  
- Async background processing with FastAPI  

---

## 🧩 Architecture
| Component | Description |
|------------|-------------|
| **FastAPI App** | Main service with async endpoints |
| **Prompt Modules** | Generate Email/WhatsApp text variants |
| **Image Engine** | Mock image generation + presigned URL creation |
| **Moodboard** | Scrape + extract color palette from brand sites |
| **Storage Layer** | Simulated MongoDB + S3 for persistence |

---

## ⚙️ Setup

### Local
```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```

Docker
docker build -t gen-variant-engine .
docker run -d -p 8000:8000 --env-file .env gen-variant-engine

🌐 Example Endpoints
POST /generate-email-variants

Generate emotion- and tone-based email copy variants.

{
  "base_prompt": "Announce a new feature to premium users.",
  "emotion": "excitement",
  "tone": "conversational",
  "variations": 3
}

POST /generate-image-variant

Mock image variant generation with presigned URLs.

{
  "prompt": "A cozy bookstore at sunset",
  "emotion": "nostalgic",
  "tone": "warm"
}

🔒 Environment Variables
PROJECT_ID=demo-project
S3_BUCKET=my-bucket
AWS_REGION=ap-south-1
MONGODB_URI=mongodb://localhost:27017/demo
LOG_LEVEL=INFO

🧠 Tech Stack

Python 3.10+, FastAPI

Pydantic, AsyncIO

Pillow, boto3 (mocked)

Docker, MongoDB (mocked)

⚠️ Disclaimer

This repository is an educational recreation of a private internship project.
No proprietary data, credentials, or internal code are included.
