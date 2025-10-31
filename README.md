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
