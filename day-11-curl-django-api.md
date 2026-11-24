# Day 11: curl -v against my Django API

Local dev server only (`python manage.py runserver`). Endpoint tested: `/api/inventory/materials/` (JWT auth, token from `/api/token/`).

## Results

| Method | URL | Status | Meaning |
|---|---|---|---|
| GET | /api/inventory/materials/ | 200 | list returned (16735 bytes of JSON) |
| OPTIONS | /api/inventory/materials/ | 200 | DRF describes the endpoint (allowed methods and fields) |
| POST | /api/inventory/materials/ | 201 | created a new material (id 38) |
| PUT | /api/inventory/materials/38/ | 200 | replaced the whole object |
| DELETE | /api/inventory/materials/38/ | 204 | deleted, empty body (0 bytes) |
| PUT | /api/inventory/materials/38/ | 404 | the resource no longer exists |

## How to read curl -v

| Prefix | Meaning |
|---|---|
| `*` | connection info |
| `>` | the request curl sent (method, path, headers, body size) |
| `<` | the response from Django (status line, headers) |

Auth header on every call: `Authorization: Bearer <token>`. `-X` is only needed for PUT and DELETE, because `-d` already makes curl send POST.

## Mistake that taught me something

`POST /api/inventory/` returned 404 with an HTML page, even with a valid token. `/api/inventory/` is only a prefix. The real routes are `materials/`, `materials/<int:pk>/`, `categories/`, `requests/`, and so on. The 404 was about the URL, not about authentication (that would be 401/403).

## Response headers seen (Django dev server)

| Header | Value | Role |
|---|---|---|
| X-Frame-Options | DENY | blocks embedding in an iframe (anti-clickjacking) |
| X-Content-Type-Options | nosniff | browser must trust the declared Content-Type |
| Referrer-Policy | same-origin | Referer header sent only to the same site |
| Cross-Origin-Opener-Policy | same-origin | isolates the page from cross-origin windows |
| Vary | origin | cache depends on the Origin header |
| Server | WSGIServer/0.2 CPython/3.14.10 | reveals server and Python version |
| Content-Type | text/html (error page) / application/json (API) | format of the body |

## Security notes

- With `DEBUG = True`, a 404 page lists every URL pattern of the project. Fine on localhost, must be `False` in production.
- The `Server` header shows the exact software and version. In production, hide or generalize it.
- A JWT access token is a credential. Never paste it in chats, issues or commits.

## Check yourself

1. Which status code does a successful POST usually return, and a successful DELETE?
   201 Created for POST, 204 No Content for DELETE.
2. Why did PUT on /materials/38/ return 404 after the DELETE?
   The resource was deleted, so the URL no longer points to anything.
3. What is the difference between `>` and `<` lines in curl -v?
   `>` is the request sent by curl, `<` is the response from the server.
4. Why did `POST /api/inventory/` return 404 and not 401?
   The authentication was fine; the URL itself does not exist. Real endpoints are under `materials/`, `requests/`, etc.
5. Name two security headers seen in the response and what they do.
   X-Frame-Options: DENY (anti-clickjacking) and X-Content-Type-Options: nosniff (no MIME sniffing).
