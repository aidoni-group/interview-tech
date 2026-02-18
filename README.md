# Pair Programming Exercise: Smart Document Ingest Service

**Time:** 45–60 minutes

**Goal:** Build a clean, production-ready endpoint that closely mirrors the document ingestion flow from your AI Legal Document Analyser project.

---

## User Story

> As a user I want to submit a document (title + content)
> so that the system can automatically extract metadata and queue it for AI analysis.

---

## Requirements

Build a `POST /documents/ingest` endpoint that does the following:

### 1. Accept JSON payload

```json
{
  "title": "GDPR Compliance Agreement v2.1",
  "content": "This agreement... (long text here)",
  "type": "legal"
}
```

> **Note:** `type` is optional. Allowed values: `legal` | `contract` | `note`

### 2. Validate the input

| Field     | Rules                       |
|-----------|-----------------------------|
| `title`   | required, min 5 characters  |
| `content` | required, min 50 characters |
| `type`    | optional enum               |

### 3. Calculate basic metadata

- `wordCount`
- `estimatedReadingTime` (minutes, rounded)

### 4. Generate 3–6 smart tags/keywords

Mock function for now.

### 5. Persist the processed document

In-memory array is fine.

### 6. Queue a background job

Immediately queue a background job named `analyze-document` using BullMQ (Redis ready).

### 7. Return `201 Created`

```json
{
  "id": "doc_123abc",
  "status": "queued",
  "metadata": { "...": "..." }
}
```

---

## Bonus (if time)

- Replace mock with real LangChain + OpenAI structured output
- Add vector embedding comment for RAG
- Show BullMQ retry / dead-letter setup

---

## Tech Stack (already set up)

- NestJS + TypeScript
- BullMQ + ioredis (Redis port 6379)
- class-validator + class-transformer

---

## How to Run

```bash
docker run -d -p 6379:6379 redis:alpine
npm install
npm run start:dev
```

### Test with curl

```bash
curl -X POST http://localhost:3000/documents/ingest \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Test Contract",
    "content": "This is a very long test content that has at least fifty characters so validation passes...",
    "type": "contract"
  }'
```

---

## Files to Work On

| File                                                    | Notes          |
|---------------------------------------------------------|----------------|
| `src/documents/dto/ingest-document.dto.ts`              | skeleton ready |
| `src/documents/documents.controller.ts`                 |                |
| `src/documents/documents.service.ts`                    |                |
| `src/documents/processors/document.processor.ts`        |                |

---

Let's pair program! You drive or I drive — your choice. Ready when you are! 🚀

*Created for Masoud Banimahd – inspired by your AI Legal Document Analyser*
