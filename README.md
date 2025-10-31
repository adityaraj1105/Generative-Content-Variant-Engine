Generative Content Variant Engine — Project Documentation
Table of contents

Project summary

Repository layout

Architecture & components

Environment variables & secrets management

Setup (local & Docker)

Endpoints (detailed)

Image generation & storage behaviour

Background processing & async patterns

Logging, monitoring & observability

Security considerations

Appendix: sample requests, responses & curl examples

1. Project summary

The Generative Content Variant Engine is a FastAPI-based microservice that demonstrates how to build scalable, modular pipelines for:

Generating personalized text variants (Email & WhatsApp) from base templates conditioned on emotion, tone, and strategic angle.

Creating image variants aligned to brand mood using generative models and lightweight compositing.

Producing moodboards from URLs with color palette extraction and metadata enrichment.

Persisting generated outputs in a mock database (MongoDB) and assets in simulated S3 storage using pre-signed URLs.

Running all heavy operations asynchronously to keep API latency low.

This repository is a safe, educational recreation of an internal project and contains no proprietary code, data, or endpoints.

2. Repository layout
gen-variant-engine/
├─ app/
│  ├─ main.py
│  ├─ email_prompt.py
│  ├─ email_variant.py
│  ├─ whatsapp_prompt.py
│  ├─ whatsapp_variant.py
│  ├─ image_gen.py
│  ├─ moodboard.py
│  ├─ base_templates/
│  │  ├─ email_template.py
│  │  └─ whatsapp_template.py
│  └─ utils/
│     ├─ mock_mongo.py
│     ├─ storage_utils.py
│     └─ config.py
│
├─ data/
│  └─ sample_texts.json
├─ tests/
│  └─ test_api.py
├─ .env.example
├─ requirements.txt
├─ Dockerfile
├─ docker-compose.yml
└─ README.md


Notes:

main.py hosts all FastAPI endpoints and background task wiring.

image_gen.py and moodboard.py use mocked AI and storage layers.

email_* and whatsapp_* modules demonstrate text variant logic.

No real keys, client IDs, or service accounts are included.

3. Architecture & components
Core components

FastAPI app — Serves endpoints for variant and moodboard generation.

Prompt & variant modules — Generate text variants conditioned on emotion, tone, and context.

Image generator — Mocked image synthesis and S3 upload with presigned preview URLs.

Moodboard generator — Extracts images, colors, and typography from URLs.

Storage layer — MongoDB (mocked) for metadata and simulated S3 for assets.

Background tasks — Offload long operations using BackgroundTasks and asyncio.to_thread().

Data flow (simplified)

User sends a generation request.

System triggers the appropriate module (text or image).

Variants are produced and uploaded (or mocked) to storage.

Metadata saved in the database.

Response returns variant info and URLs.

4. Environment variables & secrets management

Create a .env file from .env.example and configure:

PROJECT_ID=demo-project
LOCATION=us-central1
S3_BUCKET=my-demo-bucket
S3_FOLDER=variants
AWS_REGION=ap-south-1
AWS_ACCESS_KEY_ID=demo
AWS_SECRET_ACCESS_KEY=demo
MONGODB_URI=mongodb://localhost:27017/demo_db
LOG_LEVEL=INFO


Tip: Never commit .env — it’s already in .gitignore.

5. Setup (local & Docker)
Local setup
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload


Open docs at http://localhost:8000/docs
.

Docker setup
docker build -t gen-variant-engine .
docker run -d -p 8000:8000 --env-file .env gen-variant-engine


or use:

docker compose up --build

6. Endpoints (detailed)
GET /health

Purpose: Health check
Response: {"status": "ok"}

POST /moodboard

Generate a moodboard from a brand URL.

Example body:

{
  "client_id": "demo-123",
  "url": "https://example.com",
  "options": { "max_images": 8, "extract_colors": true }
}


Response:

{"job_id": "mood-xyz", "status": "queued"}

POST /generate-image-variant

Create image variants using a base prompt and emotion-tone pairing.

Example body:

{
  "prompt": "A cozy bookstore at sunset",
  "emotion": "nostalgic",
  "tone": "warm",
  "variations": 3
}


Response:

{
  "results": [
    {
      "filename": "img_1.png",
      "presigned_url": "https://example-bucket.s3.amazonaws.com/demo/img_1.png"
    }
  ]
}

POST /generate-email-variants

Generates multiple email body/subject/button combinations.

POST /generate-whatsapp-variants

Generates WhatsApp message + CTA/button variations.

Both accept:

{
  "base_prompt": "Announce a new feature to premium users.",
  "emotion": "excitement",
  "tone": "conversational",
  "strategic_angle": "product_launch",
  "variations": 3
}

7. Image generation & storage behaviour

Uploads use mock storage_utils.py functions.

Returned URLs simulate presigned S3 URLs.

Each upload is unique (UUID-based) to avoid collisions.

Real deployments can easily swap storage_utils with a boto3-based backend.

8. Background processing & async patterns

FastAPI BackgroundTasks — handle quick async jobs.

asyncio.to_thread() — run blocking Pillow or scraping tasks.

Status polling — clients can poll /status/{job_id} if extended.

9. Logging, monitoring & observability

Configurable LOG_LEVEL via env.

Structured logs in JSON for pipeline tracing.

Optional integration points for OpenTelemetry.

10. Security considerations

Never expose raw AWS or DB credentials.

Keep .env and secret files local.

Validate all user inputs before processing.

Pre-signed URLs should expire within a reasonable TTL (e.g. 1 hour).

Scraped HTML sanitized to prevent XSS if used in UI layers.

11. Appendix — examples & curl snippets
Generate an image variant
curl -X POST "http://localhost:8000/generate-image-variant" \
-H "Content-Type: application/json" \
-d '{
  "prompt": "A dreamy café at dusk",
  "emotion": "romantic",
  "tone": "soft",
  "variations": 2
}'

Generate an email prompt variant
curl -X POST "http://localhost:8000/generate-email-variants" \
-H "Content-Type: application/json" \
-d '{
  "base_prompt":"Re-engage inactive users",
  "emotion":"empathetic",
  "tone":"friendly",
  "variations":2
}'

⚠️ Disclaimer

This repository is a public demonstration derived from a private internship project.
All code and examples here are original recreations for educational use only —
no proprietary data, code, or credentials from the internship are included.
