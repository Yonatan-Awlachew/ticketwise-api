# Ticketwise API

> 🚧 Status: in development (day 1/30)

A backend API for small SaaS support teams: customers open support tickets, agents triage
and answer them, and AI helps by classifying tickets, drafting replies grounded in the
company's help-centre articles (with citations), and answering questions through an
assistant that can look things up in the system.

## Problem
Small companies handle support through email and chat, lose track of requests, and spend
agent time classifying tickets and re-typing answers that already exist in their help centre.

## Planned features
- Ticket CRUD with filtering, search and pagination
- Comments, assignment and a status workflow
- JWT authentication with customer / agent / admin roles
- AI triage (category, priority, sentiment) with validated structured output
- AI reply suggestions using RAG over help-centre articles, with citations
- A tool-calling support assistant
- Tests, Docker, CI and a live deployment

## Planned tech stack
Python · FastAPI · Pydantic · PostgreSQL · SQLAlchemy · Alembic · JWT · pytest ·
Docker · GitHub Actions · pgvector · LLM APIs

## Running locally
```bash
# 1. Create virtual environment
python -m venv .venv

# 2. Activate virtual environment
source .venv/bin/activate        # macOS/Linux
.venv\Scripts\activate        # Windows

# 3. Install dependencies & run (from project root)
pip install --upgrade pip
pip install -r requirements.txt
python -m app.main
```