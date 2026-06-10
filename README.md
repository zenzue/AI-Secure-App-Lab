# AI Secure App Lab

**AI Secure App Lab** is a Docker-based, developer-focused security lab for learning common vulnerabilities in LLM-powered applications.

The lab uses a deterministic **mock LLM** so it works offline without OpenAI, Gemini, Ollama, or any external API key. It is designed for education, internal training, and secure AI development workshops.

## What You Will Learn

- System prompt leakage
- Direct prompt injection
- RAG indirect prompt injection
- Sensitive information disclosure
- Excessive agency / unsafe tool calling
- Improper output handling
- Memory poisoning
- Unbounded consumption / denial-of-wallet risk
- Data poisoning in knowledge bases
- Package hallucination and supply-chain risk

## Architecture

```text
Browser UI
   |
   v
FastAPI Lab App
   |
   +--> Mock LLM behavior
   +--> In-memory RAG documents
   +--> In-memory memory store
   +--> Fake customer/order data
   +--> Vulnerable and patched modes
```

## Requirements

- Docker
- Docker Compose v2

## Quick Start

```bash
cp .env.example .env
docker compose up --build
```

Open:

```text
http://localhost:8000
```

## Environment Variables

The application is configured through `.env`.

```env
APP_ENV=lab
APP_NAME=AI Secure App Lab
PORT=8000
LAB_SECURITY_MODE=vulnerable
LAB_SECRET_INTERNAL_REFUND_CODE=REFUND-ADMIN-7781
LAB_SECRET_SYSTEM_MARKER=SYS-LAB-44A9
LAB_CURRENT_USER=alice.employee@jcompass.local
LAB_CURRENT_ROLE=employee
MAX_INPUT_CHARS=2000
MAX_OUTPUT_CHARS=1500
MAX_DAILY_ACTIONS=20
```

## Security Modes

### Vulnerable Mode

```env
LAB_SECURITY_MODE=vulnerable
```

This mode intentionally demonstrates insecure LLM application behavior.

### Patched Mode

```env
LAB_SECURITY_MODE=patched
```

This mode demonstrates safer implementation patterns such as:

- rejecting large input
- treating RAG documents as untrusted
- blocking prompt leakage
- enforcing backend authorization for tools
- sanitizing model output
- rejecting unsafe memory claims

After changing `.env`, restart:

```bash
docker compose down
docker compose up --build
```

## Lab Documents

| File | Purpose |
|---|---|
| `docs/student-guide.md` | Student instructions and module walkthrough |
| `docs/questions.md` | Challenge questions for each module |
| `docs/flags.md` | Flag list and learning meaning |
| `docs/instructor-guide.md` | Instructor setup, teaching notes, and suggested answers |
| `docs/secure-coding-guide.md` | Developer-focused mitigations |

## API Health Check

```bash
curl http://localhost:8000/api/health
```

## Reset Lab State

```bash
curl -X POST http://localhost:8000/api/reset
```

## Educational Safety Notice

This project uses fake data, fake secrets, fake customers, and fake tools. It is intended only for defensive education and secure AI development training.

Do not use these examples against systems you do not own or have permission to test.

`Aung Myat Thu`
