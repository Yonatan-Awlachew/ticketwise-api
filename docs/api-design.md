# API design

Base path `/api/v1`. JSON everywhere. Auth: `Authorization: Bearer <JWT>` from `POST /auth/login` (OAuth2 password form: `username` = email).

## Conventions
- Lists return `{"items": [...], "total": n, "limit": 20, "offset": 0}`; `limit` ≤ 100.
- Errors return `{"error": {"code", "message", "request_id", "details?"}}`.
- `PATCH` changes only the fields sent (`exclude_unset`); unknown fields → 422.
- Every response carries `X-Request-ID`.

## Endpoints

| Method | Path | Auth | Body | Success | Errors |
|---|---|---|---|---|---|
| GET | /health | – | – | 200 | – |
| POST | /auth/register | – | UserCreate | 201 UserRead | 409, 422 |
| POST | /auth/login | – | form | 200 Token | 401, 429 |
| GET | /users/me | user | – | 200 | 401 |
| GET | /tickets?status&priority&assignee_id&q&sort&limit&offset | user | – | 200 Page | 401, 422 |
| POST | /tickets | user | TicketCreate | 201 + Location | 401, 422 |
| GET | /tickets/stats | staff | – | 200 | 401, 403 |
| GET | /tickets/{id} | owner/staff | – | 200 TicketDetail | 401, 404 |
| PATCH | /tickets/{id} | owner/staff | TicketUpdate | 200 | 401, 403, 404, 409, 422 |
| PATCH | /tickets/{id}/assign | staff | {assignee_id} | 200 | 401, 403, 404, 409, 422 |
| DELETE | /tickets/{id} | admin | – | 204 | 401, 403, 404 |
| GET | /tickets/{id}/comments | owner/staff | – | 200 Page | 401, 404 |
| POST | /tickets/{id}/comments | owner/staff | {body} | 201 | 401, 404, 409, 422 |
| POST | /tickets/{id}/comments/{cid}/publish | staff | – | 200 | 401, 403, 404, 409 |
| POST | /documents | staff | DocumentCreate | 201 | 401, 403, 422 |
| GET | /documents | staff | – | 200 Page | 401, 403 |
| GET | /documents/search?q&k | staff | – | 200 [SearchHit] | 401, 403, 422 |
| DELETE | /documents/{id} | staff | – | 204 | 401, 403, 404 |
| POST | /tickets/{id}/ai/summary | staff | – | 200 | 401, 403, 404, 429, 503 |
| POST | /tickets/{id}/ai/triage | staff | – | 200 | 401, 403, 404, 429, 503 |
| POST | /tickets/{id}/ai/suggest-reply?save_draft | staff | – | 200 | 401, 403, 404, 429, 503 |
| POST | /assistant/chat | user | {messages, allow_actions} | 200 | 401, 422, 429, 503 |
| PATCH | /admin/users/{id}/role | admin | {role} | 200 | 401, 403, 404, 409 |
| GET | /admin/ai-usage?days | admin | – | 200 | 401, 403 |

## Decisions
1. **PATCH, not PUT**, for tickets: clients almost always change one field (status); PUT would force them to resend everything.
2. **404 instead of 403** for other customers' tickets: avoids confirming which ids exist.
3. **AI actions as sub-resources** (`POST /tickets/{id}/ai/triage`): they are actions on a ticket, not CRUD, and live next to the resource they act on.
4. **Versioned under `/api/v1`** from day one, so breaking changes can go to `/api/v2`.
