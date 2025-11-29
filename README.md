# 🚀 AI-Powered Dynamic Form Generator

A full-stack application that converts natural language prompts into dynamic, shareable web forms using LLM-powered schema generation and a context-aware memory retrieval system. Built with Next.js 15, Express, MongoDB, Cloudinary, and Gemini/Groq/OpenRouter APIs.

📌 1. Setup Instructions
Clone Repository
git clone https://github.com/<your-username>/ai-dynamic-form-generator
cd ai-dynamic-form-generator

Install Dependencies
npm install

Environment Variables

Create /frontend/.env and /backend/.env:

MONGO_URI=your_mongodb_atlas_url
CLOUDINARY_CLOUD_NAME=xxx
CLOUDINARY_API_KEY=xxx
CLOUDINARY_API_SECRET=xxx
AI_API_KEY=your_llm_key
JWT_SECRET=your_jwt_secret

Run Servers

Frontend:

npm run dev


Backend:

npm start

📌 2. Example Prompts & Generated Form Samples
Example Prompt 1

Prompt:
“I need a signup form with name, email, age, and profile picture.”

Generated Schema:

{
  "title": "Signup Form",
  "fields": [
    { "type": "text", "label": "Name", "required": true },
    { "type": "email", "label": "Email", "required": true },
    { "type": "number", "label": "Age" },
    { "type": "image", "label": "Profile Picture" }
  ]
}

Example Prompt 2

Prompt:
“Create an internship hiring form with resume upload and GitHub link.”

Generated Schema:

{
  "title": "Internship Hiring Form",
  "fields": [
    { "type": "text", "label": "Full Name", "required": true },
    { "type": "email", "label": "Email", "required": true },
    { "type": "file", "label": "Resume", "accepted": ["pdf"] },
    { "type": "url", "label": "GitHub Profile" }
  ]
}

📌 3. Architecture Notes for Memory Retrieval

The system uses a Context-Aware Semantic Memory Layer to determine which past user forms are relevant during new form generation.

Memory Storage

Each generated form stores:

JSON schema

Short metadata summary

Embedding vector

Retrieval Pipeline

Convert user’s new prompt → embedding vector

Perform vector similarity search on stored embeddings

Select top-K most relevant forms (K = 3–10)

Inject only the summaries/schemas of these forms into the LLM prompt

LLM generates a new schema using contextual continuity

Why This Approach

Avoids sending thousands of past records to the LLM

Reduces token usage

Ensures better relevance and pattern continuity

Supports massive form histories (10K–100K+)

Context Prompt Structure
You are an intelligent form schema generator.

Relevant form history:
[ ... top-K retrieved summaries ... ]

Generate a new schema for this request:
"<user prompt>"

📌 4. Scalability Handling
Semantic Retrieval

Embeddings allow O(log n) retrieval instead of O(n) scanning

Only top-K summaries (<5 KB total) are passed to LLM

Database Efficiency

Form metadata and embeddings stored separately for fast queries

MongoDB Atlas Search or vector DB (Pinecone) improves retrieval time

LLM Token Management

Summaries are compressed

No large schemas sent

Context never exceeds 3–10 relevant forms

High Volume Handling

System supports:

10,000+ form schemas

Millions of submissions

Concurrent LLM calls with caching & debouncing

📌 5. Limitations

Basic validations only (required, min/max, email check)

LLM-generated schema quality depends on prompt clarity

Cloudinary dependency for media upload

Free LLM APIs may introduce rate limits

No drag-and-drop manual form builder yet

No role-based or multi-user collaboration features

📌 6. Future Improvements

Add drag-and-drop schema editor

Add advanced validation rules (regex, conditional logic)

Use Pinecone for large-scale vector memory

Introduce versioning for form schemas

Add analytics dashboard for submissions

Provide multi-language form generation

Add real-time collaboration for form creation

Enable server-side streaming LLM responses
