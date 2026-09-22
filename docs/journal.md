# Learning journal

## Day 1: Setup, Git & Core Python
- **Built:** Initial repo setup, `.env`, and virtual environment.
- **Learned:** Type hints, decorators, context managers, `venv`, basic Git flow.
- **Confused me:** Class-based context managers (`__enter__`/`__exit__`).
- **Tomorrow:** FastAPI basics & Pydantic models.

### Day 2: HTTP, REST & JSON
- **Built:** API request/response dissection, curl CLI testing, and JSON serializations.
- **Learned:** HTTP request/response flow, status codes, REST conventions, and JSON `dumps` vs `loads`.
- **Confused me:** Distinguishing 401 (AuthN) vs 403 (AuthZ) edge cases in API responses.
- **Tomorrow:** FastAPI fundamentals

---

## Reference Cheat Sheet

### 1. HTTP & REST Basics
- **Request Flow:** Client → DNS → TCP/TLS → HTTP Request → Server Logic → HTTP Response.
- **HTTP Status Codes:**
  - `200 OK` (GET/PATCH) | `201 Created` (POST) | `204 No Content` (DELETE)
  - `400 Bad Request` | `401 Unauthorized` | `403 Forbidden` | `404 Not Found` | `422 Unprocessable Content`
  - `500 Internal Error` | `503 Service Unavailable`

### 2. Interview Q&A Flashcards
- **PUT vs. PATCH?**  
  `PUT` replaces the entire resource. `PATCH` applies partial updates (e.g., `{"status": "resolved"}`).
- **401 vs. 403?**  
  `401 Unauthorized` = Unauthenticated ("Who are you? Log in first").  
  `403 Forbidden` = Authenticated, but lacks permissions ("You are logged in, but no access").
- **What makes an API RESTful?**  
  Uses standard HTTP methods (GET, POST, PUT, PATCH, DELETE), resource-oriented URLs (`/tickets/42`), stateless client-server communication, and standard HTTP status codes.

### 3. Python Code & CLI Snippets
```python
import json

# JSON Parsing Gotcha
data = {"status": "open", "id": 42}
json_str = json.dumps(data)  # Python dict -> JSON String
parsed_dict = json.loads(json_str)  # JSON String -> Python dict