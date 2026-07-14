# HTTP — HyperText Transfer Protocol

## What is it?
- HTTP is the **foundation of data exchange on the web**
- It's a request-response protocol — client sends a request, server sends a response
- Stateless — each request is independent, server doesn't remember previous requests
- Runs over TCP, default port **80**

## HTTP Request Structure
```
GET /api/users HTTP/1.1
Host: example.com
Authorization: Bearer token123
Content-Type: application/json

(optional body for POST/PUT)
```

## HTTP Methods
| Method | Purpose |
|--------|---------|
| `GET` | Retrieve data |
| `POST` | Create new resource |
| `PUT` | Replace a resource |
| `PATCH` | Partially update a resource |
| `DELETE` | Remove a resource |

## HTTP Status Codes
| Code | Meaning |
|------|---------|
| 200 | OK — Success |
| 201 | Created |
| 301 | Moved Permanently (redirect) |
| 400 | Bad Request |
| 401 | Unauthorized (not logged in) |
| 403 | Forbidden (logged in but no access) |
| 404 | Not Found |
| 500 | Internal Server Error |

## HTTP Versions
| Version | Key Feature |
|---------|-------------|
| HTTP/1.1 | Persistent connections, one request at a time per connection |
| HTTP/2 | Multiplexing — multiple requests over one connection |
| HTTP/3 | Uses QUIC (UDP-based), faster |

## Good to Know
- HTTP sends data in **plaintext** — not secure (use HTTPS)
- Headers carry metadata (auth tokens, content type, cookies)
- RESTful APIs are built on HTTP
