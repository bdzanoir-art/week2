# Day 10: HTTP methods, status codes, headers, cookies

## Methods

| Method | Use | Safe | Idempotent |
|---|---|---|---|
| GET | Read data | yes | yes |
| POST | Create data / send a form | no | no |
| PUT | Replace the whole resource | no | yes |
| PATCH | Change part of a resource | no | not always |
| DELETE | Remove a resource | no | yes |
| HEAD | Like GET, headers only | yes | yes |
| OPTIONS | Ask which methods are allowed | yes | yes |

Safe = does not change anything on the server. Idempotent = sending it many times gives the same result as sending it once.

## Status codes

| Class | Meaning | Codes to know |
|---|---|---|
| 1xx | Information | 101 |
| 2xx | Success | 200 OK, 201 Created, 204 No Content |
| 3xx | Redirect | 301 permanent, 302 temporary, 304 Not Modified |
| 4xx | Client error | 400, 401, 403, 404, 405, 429 |
| 5xx | Server error | 500, 502, 503 |

- 401 = not logged in (server does not know who you are)
- 403 = logged in but not allowed (e.g. a simple user on an admin page)
- 405 = the URL does not accept this method for anyone (see the Allow header)

## Headers

| Type | Headers |
|---|---|
| Request | Host, User-Agent, Accept, Content-Type, Authorization, Cookie |
| Response | Content-Type, Location, Set-Cookie, Cache-Control |
| Security | Strict-Transport-Security, Content-Security-Policy, X-Content-Type-Options |

## Cookies

- The server sends `Set-Cookie`; the browser sends it back in the `Cookie` header.
- `HttpOnly`: JavaScript cannot read the cookie (protects the session from theft via XSS).
- `Secure`: sent over HTTPS only.
- `SameSite`: limits sending the cookie on cross-site requests.

## Lab: seeing a redirect with curl

    curl -sI http://github.com/this-page-does-not-exist-123 | head -1
    HTTP/1.1 301 Moved Permanently

    curl -sIL http://github.com/this-page-does-not-exist-123 | grep -i -E "^HTTP|^location"

The first request stops at the 301 (the `Location` header points to the `https://` URL). With `-L`, curl follows it and reaches the final 404. A browser does this silently.

## Check yourself

1. What is the difference between PUT and PATCH?
   PUT replaces the whole resource. PATCH changes only part of it.
2. What does 401 mean, and how is it different from 403?
   401: not authenticated (not logged in or bad credentials). 403: authenticated but not allowed.
3. Why is HttpOnly important for a session cookie?
   JavaScript cannot read it, so an XSS script cannot steal the session.
4. Which header tells the browser where to go after a 301?
   The Location header.
5. What happens if a server answers 405?
   The URL exists but does not support that HTTP method (for anyone). The Allow header lists the methods it accepts.
