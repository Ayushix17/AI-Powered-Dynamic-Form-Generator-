# 🚀 AI-Powered Dynamic Form Generator

Generate dynamic, shareable forms using natural language. Features scalable context-aware memory retrieval, intelligent schema generation, image uploads, and submission management — powered by Next.js, Express, MongoDB, and modern LLMs.

✨ Overview

This project enables users to create fully functional forms using a simple text prompt. An LLM converts the prompt into a JSON schema, which is used to render forms dynamically. All submissions, including uploaded media, are stored securely and viewable in a private dashboard.

The system includes a semantic memory retrieval layer that selects only the most relevant past forms (top-K) to guide new form generation without sending the entire user history — enabling scalability to thousands of forms.

🧩 Core Features
🔐 Authentication

Email/password login and signup

Secure user session management

🤖 AI Form Generation

Convert natural language → JSON schema

Models supported: Gemini, Groq, OpenRouter, or any free LLM API

Persist schema to MongoDB

🧠 Context-Aware Memory Retrieval

Store metadata + embeddings for each form

Retrieve only top-K relevant past forms

Prevents token overflow and reduces latency

Ensures consistent form-pattern continuity for each user

📝 Dynamic Form Rendering

Public shareable link: /form/[id]

Auto-generated UI from schema

Supports images, text fields, file uploads, validation rules

📤 Image & File Uploads

Cloudinary upload pipeline

Store only returned URLs in DB

Works in both:

Form generator UI

Public form submission

📊 User Dashboard

View all created forms

View submissions grouped by form

Access uploaded images/files via URLs

🏗️ Tech Stack
Layer	Technology
Frontend	Next.js 15 + TypeScript
Backend	Express.js
Database	MongoDB Atlas
AI Models	Gemini / Groq / OpenRouter / Free Chat APIs
Embeddings	Gemini Embeddings / JW Embeddings / Any Provider
Media Uploads	Cloudinary
Auth	Custom email/password
🧠 Memory Retrieval Architecture

To scale beyond 10,000+ user forms, the system uses semantic search:

1. Store Form History

For every generated form:

JSON schema

Short metadata summary

Embedding vector

2. Query for Relevant Past Forms

On each new prompt:

Convert user prompt → embedding

Compute vector similarity (Mongo Atlas Search / Pinecone / custom store)

Retrieve top-K relevant forms (3–10 max)

Trim these into a compact context

3. Build Final LLM Prompt
You are an intelligent form schema generator.

Here is relevant user form history:
[ ... top-K summaries ... ]

Now generate a new form schema for this request:
"<user prompt>"

Why This Scales

Only ~1 KB of context per form summary

Top-K ensures stable latency

Prevents hitting token limits

Works even with 100K+ stored forms

📂 Project Structure
/frontend (Next.js 15)
  /app
  /components
  /lib

/backend (Express)
  /routes
  /controllers
  /models

/shared
  /types
  /utils

⚙️ Setup Instructions
1. Clone Repository
git clone https://github.com/<your-username>/ai-dynamic-form-generator
cd ai-dynamic-form-generator

2. Install Dependencies
npm install

3. Configure Environment Variables

Create /frontend/.env and /backend/.env:

MONGO_URI=your_mongodb_atlas_url
CLOUDINARY_CLOUD_NAME=xxx
CLOUDINARY_API_KEY=xxx
CLOUDINARY_API_SECRET=xxx
AI_API_KEY=your_llm_key
JWT_SECRET=your_jwt_secret

4. Run Server
npm run dev

🧪 Example Prompts

“Create a registration form with name, email, age, and profile picture”

“Build a feedback form with ratings and optional screenshots”

“Make a job application form with resume upload and GitHub link”

📌 Example Generated Schema
{
  "title": "Job Application",
  "fields": [
    { "type": "text", "label": "Full Name", "required": true },
    { "type": "email", "label": "Email", "required": true },
    { "type": "file", "label": "Resume", "accepted": ["pdf"] },
    { "type": "url", "label": "GitHub Profile" }
  ]
}

⚡ Scalability Notes

Embeddings reduce DB load and improve retrieval speed

Limits the LLM context to <5KB

Works with 100K+ user forms

Vector search can be swapped with Pinecone for large-scale deployments

🚧 Limitations

Basic validation rules only (optional upgrade)

Requires internet access for Cloudinary + LLM APIs

Large-scale vector search needs a dedicated vector DB for optimal performance

🌱 Future Improvements

Add client-side form validation builder

Drag-and-drop form editor

Role-based access controls

Realtime collaboration for form creation

Switch to RAG-style multi-vector memory for extremely large datasets
