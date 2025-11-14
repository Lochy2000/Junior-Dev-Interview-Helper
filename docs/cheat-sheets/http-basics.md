# HTTP Basics Cheat Sheet

> Essential HTTP concepts for web developers

## 📋 Table of Contents

- [HTTP Methods](#http-methods)
- [Status Codes](#status-codes)
- [Headers](#headers)
- [Request/Response Cycle](#requestresponse-cycle)
- [Common Patterns](#common-patterns)

---

## HTTP Methods

### GET

**Purpose:** Retrieve data

```http
GET /api/users HTTP/1.1
Host: api.example.com
```

**Characteristics:**
- Safe (doesn't change server state)
- Idempotent (multiple requests = same result)
- Can be cached
- Parameters in URL query string
- Visible in browser history

---

### POST

**Purpose:** Create new resource

```http
POST /api/users HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "name": "Alice",
  "email": "alice@example.com"
}
```

**Characteristics:**
- Not safe (changes server state)
- Not idempotent (multiple requests = multiple resources)
- Not cached by default
- Data in request body
- Not visible in browser history

---

### PUT

**Purpose:** Update/replace entire resource

```http
PUT /api/users/123 HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "id": 123,
  "name": "Alice Updated",
  "email": "alice@example.com"
}
```

**Characteristics:**
- Not safe
- Idempotent (multiple requests = same result)
- Replaces entire resource

---

### PATCH

**Purpose:** Partial update of resource

```http
PATCH /api/users/123 HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "name": "Alice Updated"
}
```

**Characteristics:**
- Not safe
- May not be idempotent
- Updates only specified fields

---

### DELETE

**Purpose:** Remove resource

```http
DELETE /api/users/123 HTTP/1.1
Host: api.example.com
```

**Characteristics:**
- Not safe
- Idempotent
- No request body needed

---

### HEAD

**Purpose:** Same as GET but without response body

```http
HEAD /api/users HTTP/1.1
Host: api.example.com
```

**Use Cases:**
- Check if resource exists
- Get metadata without downloading
- Check last modified time

---

### OPTIONS

**Purpose:** Get allowed methods for resource

```http
OPTIONS /api/users HTTP/1.1
Host: api.example.com
```

**Response:**
```http
Allow: GET, POST, OPTIONS
Access-Control-Allow-Methods: GET, POST, OPTIONS
```

**Use Cases:**
- CORS preflight requests
- Discover API capabilities

---

## Status Codes

### 1xx - Informational

| Code | Name | Meaning |
|------|------|---------|
| 100 | Continue | Server received headers, continue with body |
| 101 | Switching Protocols | Server switching protocols (e.g., WebSocket) |

---

### 2xx - Success

| Code | Name | Meaning | When to Use |
|------|------|---------|-------------|
| 200 | OK | Request succeeded | GET, PUT, PATCH success |
| 201 | Created | Resource created | POST success |
| 204 | No Content | Success but no content to return | DELETE success |
| 206 | Partial Content | Partial GET (range request) | Large file downloads |

---

### 3xx - Redirection

| Code | Name | Meaning | When to Use |
|------|------|---------|-------------|
| 301 | Moved Permanently | Resource permanently moved | SEO-friendly redirects |
| 302 | Found | Temporary redirect | Temporary redirects |
| 304 | Not Modified | Resource not changed (cached) | Conditional GET requests |
| 307 | Temporary Redirect | Like 302 but doesn't change method | Preserve POST method |
| 308 | Permanent Redirect | Like 301 but doesn't change method | Preserve POST method |

---

### 4xx - Client Errors

| Code | Name | Meaning | When to Use |
|------|------|---------|-------------|
| 400 | Bad Request | Invalid request syntax/parameters | Validation errors |
| 401 | Unauthorized | Authentication required | Not logged in |
| 403 | Forbidden | Authenticated but no permission | Insufficient permissions |
| 404 | Not Found | Resource doesn't exist | Missing resource |
| 405 | Method Not Allowed | HTTP method not supported | Wrong method used |
| 409 | Conflict | Request conflicts with current state | Duplicate resource |
| 422 | Unprocessable Entity | Valid syntax but semantic errors | Business logic errors |
| 429 | Too Many Requests | Rate limit exceeded | Throttling |

**Common Confusion:**
- **401 vs 403**: 401 = "Who are you?" (not logged in), 403 = "I know who you are, but you can't access this"
- **400 vs 422**: 400 = syntax error, 422 = valid syntax but invalid data

---

### 5xx - Server Errors

| Code | Name | Meaning | When to Use |
|------|------|---------|-------------|
| 500 | Internal Server Error | Generic server error | Unexpected errors |
| 502 | Bad Gateway | Invalid response from upstream | Proxy/gateway errors |
| 503 | Service Unavailable | Server temporarily unavailable | Maintenance, overload |
| 504 | Gateway Timeout | Upstream server timeout | Slow upstream server |

---

## Headers

### Common Request Headers

```http
# Authentication
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

# Content Type
Content-Type: application/json
Content-Type: application/x-www-form-urlencoded
Content-Type: multipart/form-data

# Content Length
Content-Length: 1234

# Accept (what client wants)
Accept: application/json
Accept: text/html
Accept: */*

# User Agent
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)

# Host
Host: api.example.com

# Cookies
Cookie: session_id=abc123; user_id=456

# Cache Control
Cache-Control: no-cache
If-None-Match: "etag-value"
If-Modified-Since: Wed, 21 Oct 2023 07:28:00 GMT

# CORS
Origin: https://example.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: Content-Type

# Custom Headers (use X- prefix or not)
X-API-Key: your-api-key
X-Request-ID: unique-request-id
```

---

### Common Response Headers

```http
# Content Info
Content-Type: application/json; charset=utf-8
Content-Length: 1234
Content-Encoding: gzip

# Caching
Cache-Control: public, max-age=3600
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"
Last-Modified: Wed, 21 Oct 2023 07:28:00 GMT
Expires: Thu, 22 Oct 2023 07:28:00 GMT

# CORS
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Max-Age: 3600

# Security
Strict-Transport-Security: max-age=31536000
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Content-Security-Policy: default-src 'self'

# Cookies
Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Strict

# Location (for redirects)
Location: https://example.com/new-location

# Rate Limiting
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 75
X-RateLimit-Reset: 1609459200
```

---

## Request/Response Cycle

### Full HTTP Request Example

```http
POST /api/users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Content-Length: 52
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json
User-Agent: MyApp/1.0

{
  "name": "Alice",
  "email": "alice@example.com"
}
```

---

### Full HTTP Response Example

```http
HTTP/1.1 201 Created
Date: Wed, 21 Oct 2023 07:28:00 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 98
Location: /api/users/123
Cache-Control: no-cache
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 99

{
  "id": 123,
  "name": "Alice",
  "email": "alice@example.com",
  "created_at": "2023-10-21T07:28:00Z"
}
```

---

## Common Patterns

### RESTful API Endpoints

```
GET    /api/users          # List all users
GET    /api/users/123      # Get user 123
POST   /api/users          # Create new user
PUT    /api/users/123      # Update user 123 (full)
PATCH  /api/users/123      # Update user 123 (partial)
DELETE /api/users/123      # Delete user 123

# Nested resources
GET    /api/users/123/posts         # Get posts by user 123
POST   /api/users/123/posts         # Create post for user 123
GET    /api/users/123/posts/456     # Get post 456 by user 123
```

---

### Authentication Patterns

**Bearer Token (JWT):**
```http
GET /api/protected HTTP/1.1
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Basic Auth:**
```http
GET /api/protected HTTP/1.1
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

**API Key:**
```http
GET /api/protected HTTP/1.1
X-API-Key: your-api-key-here
```

**Cookie:**
```http
GET /api/protected HTTP/1.1
Cookie: session_id=abc123
```

---

### Pagination

**Query Parameters:**
```http
GET /api/users?page=2&limit=20 HTTP/1.1
```

**Response with Links:**
```json
{
  "data": [...],
  "pagination": {
    "page": 2,
    "limit": 20,
    "total": 100,
    "total_pages": 5
  },
  "links": {
    "first": "/api/users?page=1&limit=20",
    "prev": "/api/users?page=1&limit=20",
    "next": "/api/users?page=3&limit=20",
    "last": "/api/users?page=5&limit=20"
  }
}
```

---

### Error Response Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "message": "Email is required"
      },
      {
        "field": "age",
        "message": "Must be at least 18"
      }
    ]
  }
}
```

---

### CORS Preflight

**Browser sends OPTIONS first:**
```http
OPTIONS /api/users HTTP/1.1
Host: api.example.com
Origin: https://app.example.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: Content-Type
```

**Server responds:**
```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://app.example.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Max-Age: 3600
```

**Then actual request proceeds:**
```http
POST /api/users HTTP/1.1
Host: api.example.com
Origin: https://app.example.com
Content-Type: application/json
...
```

---

### Content Negotiation

**Client specifies what it wants:**
```http
GET /api/users/123 HTTP/1.1
Accept: application/json
```

**Or:**
```http
GET /api/users/123 HTTP/1.1
Accept: application/xml
```

**Server responds with matching type:**
```http
HTTP/1.1 200 OK
Content-Type: application/json

{"id": 123, "name": "Alice"}
```

---

## Interview Questions

### 1. What is the difference between PUT and PATCH?
**Answer:**
- PUT: Replace entire resource (send all fields)
- PATCH: Update specific fields only

### 2. When would you use 401 vs 403?
**Answer:**
- 401 Unauthorized: User not authenticated (not logged in)
- 403 Forbidden: User authenticated but lacks permission

### 3. What makes a request idempotent?
**Answer:** Multiple identical requests have the same effect as one request. GET, PUT, DELETE are idempotent. POST is not.

### 4. What is CORS?
**Answer:** Cross-Origin Resource Sharing - security feature that restricts web pages from making requests to different domains. Uses preflight OPTIONS requests.

### 5. Explain the difference between 200 and 201
**Answer:**
- 200 OK: Request succeeded (generic success)
- 201 Created: New resource was successfully created (use with POST)

### 6. What is a safe HTTP method?
**Answer:** Method that doesn't change server state. GET, HEAD, OPTIONS are safe. POST, PUT, PATCH, DELETE are not.

### 7. What are common cache headers?
**Answer:**
- Cache-Control: Cache behavior (max-age, no-cache)
- ETag: Resource version identifier
- Last-Modified: When resource last changed
- If-None-Match / If-Modified-Since: Conditional requests

### 8. What is the purpose of the OPTIONS method?
**Answer:** Discover allowed methods and headers for a resource. Mainly used for CORS preflight requests.

### 9. What's the difference between 302 and 307 redirects?
**Answer:** Both are temporary redirects, but 307 guarantees the HTTP method won't change (POST stays POST), while 302 may change to GET.

### 10. Explain REST
**Answer:** Representational State Transfer - architectural style using:
- Standard HTTP methods
- Stateless requests
- Resource-based URLs
- Standard status codes

---

**Keywords**: HTTP methods, HTTP status codes, REST API, HTTP headers, GET POST PUT DELETE, CORS, HTTP request response, web development, API design
