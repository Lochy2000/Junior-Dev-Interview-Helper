# REST APIs - Complete Guide for Beginners

> Build and consume RESTful web services like a pro

## 📋 Table of Contents

- [What is REST?](#what-is-rest)
- [REST Principles](#rest-principles)
- [HTTP Methods in REST](#http-methods-in-rest)
- [Resource Design](#resource-design)
- [Request & Response](#request--response)
- [Status Codes](#status-codes)
- [Authentication](#authentication)
- [Best Practices](#best-practices)
- [Common Patterns](#common-patterns)
- [Real-World Examples](#real-world-examples)
- [Interview Questions](#interview-questions)

---

## What is REST?

**REST (Representational State Transfer)** is an architectural style for designing networked applications. It uses HTTP requests to perform CRUD operations.

### 🎯 Key Concept

Think of REST like a **restaurant menu system**:
- Menu items (resources) have names
- You can VIEW the menu (GET)
- You can ORDER food (POST)
- You can MODIFY your order (PUT/PATCH)
- You can CANCEL your order (DELETE)

---

## REST Principles

### 1️⃣ Client-Server Architecture

```
┌─────────┐          ┌─────────┐
│ Client  │ ◄──────► │ Server  │
│ (React) │   HTTP   │ (Node)  │
└─────────┘          └─────────┘
```

Separation of concerns - client handles UI, server handles data.

### 2️⃣ Stateless

Each request contains ALL information needed. Server doesn't store session state.

```javascript
// ✅ Stateless - includes auth token
GET /api/users/123
Authorization: Bearer token123

// ❌ Not stateless - relies on session
GET /api/users/123
Cookie: session_id=abc
```

### 3️⃣ Cacheable

Responses must indicate if they can be cached.

```http
Cache-Control: public, max-age=3600
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"
```

### 4️⃣ Uniform Interface

Consistent, predictable API design using standard HTTP methods.

### 5️⃣ Layered System

Client can't tell if connected directly to server or through intermediaries (load balancers, proxies).

---

## HTTP Methods in REST

### The CRUD Mapping

| Operation | HTTP Method | Use Case |
|-----------|-------------|----------|
| **C**reate | POST | Create new resource |
| **R**ead | GET | Retrieve resource(s) |
| **U**pdate | PUT/PATCH | Modify resource |
| **D**elete | DELETE | Remove resource |

### Visual Guide

```
┌──────────────────────────────────────────────────┐
│               RESTful Operations                  │
├──────────────────────────────────────────────────┤
│                                                   │
│  GET /users              ─►  List all users      │
│  GET /users/123          ─►  Get user 123        │
│  POST /users             ─►  Create new user     │
│  PUT /users/123          ─►  Replace user 123    │
│  PATCH /users/123        ─►  Update user 123     │
│  DELETE /users/123       ─►  Delete user 123     │
│                                                   │
└──────────────────────────────────────────────────┘
```

---

## Resource Design

### Resource Naming Conventions

```bash
# ✅ Good - Use nouns, plural form
GET    /api/users
GET    /api/users/123
POST   /api/users
GET    /api/products
GET    /api/orders/456/items

# ❌ Bad - Using verbs
GET    /api/getUsers
POST   /api/createUser
GET    /api/deleteUser/123
```

### Nested Resources

```bash
# User's posts
GET    /api/users/123/posts           # All posts by user 123
POST   /api/users/123/posts           # Create post for user 123
GET    /api/users/123/posts/456       # Specific post by user

# Deeper nesting (avoid going too deep!)
GET    /api/users/123/posts/456/comments
```

### Query Parameters

```bash
# Filtering
GET /api/users?status=active
GET /api/users?age_min=18&age_max=65

# Sorting
GET /api/users?sort=created_at:desc
GET /api/products?sort=price:asc,name:asc

# Pagination
GET /api/users?page=2&limit=20
GET /api/users?offset=20&limit=20

# Search
GET /api/users?q=john
GET /api/products?search=laptop&category=electronics

# Field selection
GET /api/users?fields=id,name,email
```

---

## Request & Response

### Request Structure

```http
POST /api/users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json

{
  "name": "Alice Johnson",
  "email": "alice@example.com",
  "age": 25
}
```

### Response Structure

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /api/users/123
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 99

{
  "success": true,
  "data": {
    "id": 123,
    "name": "Alice Johnson",
    "email": "alice@example.com",
    "age": 25,
    "created_at": "2024-01-15T10:30:00Z",
    "updated_at": "2024-01-15T10:30:00Z"
  },
  "message": "User created successfully"
}
```

### Standard Response Format

```javascript
// Success Response
{
  "success": true,
  "data": { /* resource data */ },
  "message": "Operation successful"
}

// Error Response
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "issue": "Email is required"
      }
    ]
  }
}

// List Response with Pagination
{
  "success": true,
  "data": [ /* array of resources */ ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "total_pages": 8
  },
  "links": {
    "first": "/api/users?page=1",
    "prev": null,
    "next": "/api/users?page=2",
    "last": "/api/users?page=8"
  }
}
```

---

## Status Codes

### Success Codes (2xx)

| Code | Name | Use Case |
|------|------|----------|
| 200 | OK | Successful GET, PUT, PATCH |
| 201 | Created | Successful POST (resource created) |
| 204 | No Content | Successful DELETE (no response body) |

### Client Error Codes (4xx)

| Code | Name | Use Case |
|------|------|----------|
| 400 | Bad Request | Malformed request, validation error |
| 401 | Unauthorized | Authentication required/failed |
| 403 | Forbidden | Authenticated but no permission |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Resource already exists |
| 422 | Unprocessable | Valid syntax but business logic error |
| 429 | Too Many Requests | Rate limit exceeded |

### Server Error Codes (5xx)

| Code | Name | Use Case |
|------|------|----------|
| 500 | Internal Server Error | Unexpected server error |
| 503 | Service Unavailable | Server down/maintenance |

---

## Authentication

### 1. Bearer Token (JWT)

**Most common for modern APIs**

```http
GET /api/protected HTTP/1.1
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Login Flow:**
```
1. POST /api/auth/login
   { "email": "user@example.com", "password": "secret" }

2. Response:
   {
     "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
     "expires_in": 3600
   }

3. Use token in subsequent requests:
   Authorization: Bearer <token>
```

### 2. API Key

**Simple, for public APIs**

```http
GET /api/data HTTP/1.1
X-API-Key: your-api-key-here
```

### 3. Basic Auth

**Simple but less secure (use HTTPS!)**

```http
GET /api/protected HTTP/1.1
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

---

## Best Practices

### ✅ DO's

```bash
# Use HTTPS in production
https://api.example.com/users

# Version your API
/api/v1/users
/api/v2/users

# Use plural nouns for resources
/api/users      # ✅
/api/user       # ❌

# Use kebab-case or snake_case consistently
/api/user-profiles
/api/user_profiles

# Return appropriate status codes
POST /api/users → 201 Created
GET /api/users/123 → 200 OK
DELETE /api/users/123 → 204 No Content

# Include timestamp in responses
"created_at": "2024-01-15T10:30:00Z"
"updated_at": "2024-01-15T10:30:00Z"
```

### ❌ DON'Ts

```bash
# Don't use verbs in URLs
/api/getUser/123        # ❌
/api/users/123          # ✅

# Don't expose internal IDs unnecessarily
/api/users?db_id=12345  # ❌
/api/users/uuid-string  # ✅ Use UUIDs

# Don't return sensitive data
{
  "user": {
    "password": "hashed_pw",  # ❌ Never!
    "ssn": "123-45-6789"      # ❌ Never!
  }
}

# Don't ignore errors
try { } catch { }  # ❌
Always return meaningful error messages!
```

---

## Common Patterns

### 1. HATEOAS (Hypermedia)

Include links to related resources:

```json
{
  "id": 123,
  "name": "Alice",
  "links": {
    "self": "/api/users/123",
    "posts": "/api/users/123/posts",
    "friends": "/api/users/123/friends"
  }
}
```

### 2. Bulk Operations

```bash
# Get multiple resources by IDs
GET /api/users?ids=1,2,3,4,5

# Bulk create
POST /api/users/bulk
[
  {"name": "Alice", "email": "alice@example.com"},
  {"name": "Bob", "email": "bob@example.com"}
]

# Bulk update
PATCH /api/users
[
  {"id": 1, "status": "active"},
  {"id": 2, "status": "inactive"}
]
```

### 3. Soft Delete

Instead of deleting, mark as deleted:

```javascript
// Don't physically delete
DELETE /api/users/123

// Instead update deleted_at
PATCH /api/users/123
{
  "deleted_at": "2024-01-15T10:30:00Z",
  "status": "deleted"
}

// Filter out deleted by default
GET /api/users?include_deleted=false
```

---

## Real-World Examples

### Complete User API

```javascript
// Express.js Example
const express = require('express');
const router = express.Router();

// GET /api/users - List all users
router.get('/users', async (req, res) => {
  try {
    const { page = 1, limit = 20, status } = req.query;

    const users = await User.find(status ? { status } : {})
      .limit(limit)
      .skip((page - 1) * limit);

    const total = await User.countDocuments();

    res.json({
      success: true,
      data: users,
      pagination: {
        page: parseInt(page),
        limit: parseInt(limit),
        total,
        total_pages: Math.ceil(total / limit)
      }
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: {
        message: 'Failed to fetch users',
        details: error.message
      }
    });
  }
});

// GET /api/users/:id - Get single user
router.get('/users/:id', async (req, res) => {
  try {
    const user = await User.findById(req.params.id);

    if (!user) {
      return res.status(404).json({
        success: false,
        error: {
          code: 'USER_NOT_FOUND',
          message: 'User not found'
        }
      });
    }

    res.json({
      success: true,
      data: user
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: { message: error.message }
    });
  }
});

// POST /api/users - Create user
router.post('/users', async (req, res) => {
  try {
    const { name, email, age } = req.body;

    // Validation
    if (!name || !email) {
      return res.status(400).json({
        success: false,
        error: {
          code: 'VALIDATION_ERROR',
          message: 'Name and email are required'
        }
      });
    }

    const user = await User.create({ name, email, age });

    res.status(201)
      .location(`/api/users/${user.id}`)
      .json({
        success: true,
        data: user,
        message: 'User created successfully'
      });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: { message: error.message }
    });
  }
});

// PUT /api/users/:id - Update user (full replacement)
router.put('/users/:id', async (req, res) => {
  try {
    const user = await User.findByIdAndUpdate(
      req.params.id,
      req.body,
      { new: true, runValidators: true }
    );

    if (!user) {
      return res.status(404).json({
        success: false,
        error: { message: 'User not found' }
      });
    }

    res.json({
      success: true,
      data: user
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: { message: error.message }
    });
  }
});

// PATCH /api/users/:id - Partial update
router.patch('/users/:id', async (req, res) => {
  try {
    const user = await User.findByIdAndUpdate(
      req.params.id,
      { $set: req.body },
      { new: true, runValidators: true }
    );

    if (!user) {
      return res.status(404).json({
        success: false,
        error: { message: 'User not found' }
      });
    }

    res.json({
      success: true,
      data: user
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: { message: error.message }
    });
  }
});

// DELETE /api/users/:id - Delete user
router.delete('/users/:id', async (req, res) => {
  try {
    const user = await User.findByIdAndDelete(req.params.id);

    if (!user) {
      return res.status(404).json({
        success: false,
        error: { message: 'User not found' }
      });
    }

    res.status(204).send();
  } catch (error) {
    res.status(500).json({
      success: false,
      error: { message: error.message }
    });
  }
});

module.exports = router;
```

### Consuming REST API (Client Side)

```javascript
// Using Fetch API
class UserAPI {
  constructor(baseURL = '/api') {
    this.baseURL = baseURL;
    this.token = localStorage.getItem('token');
  }

  // Helper method for requests
  async request(endpoint, options = {}) {
    const url = `${this.baseURL}${endpoint}`;
    const config = {
      ...options,
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${this.token}`,
        ...options.headers,
      },
    };

    const response = await fetch(url, config);

    if (!response.ok) {
      const error = await response.json();
      throw new Error(error.error?.message || 'Request failed');
    }

    return response.status === 204 ? null : response.json();
  }

  // Get all users
  async getUsers(params = {}) {
    const queryString = new URLSearchParams(params).toString();
    return this.request(`/users?${queryString}`);
  }

  // Get single user
  async getUser(id) {
    return this.request(`/users/${id}`);
  }

  // Create user
  async createUser(userData) {
    return this.request('/users', {
      method: 'POST',
      body: JSON.stringify(userData),
    });
  }

  // Update user
  async updateUser(id, userData) {
    return this.request(`/users/${id}`, {
      method: 'PATCH',
      body: JSON.stringify(userData),
    });
  }

  // Delete user
  async deleteUser(id) {
    return this.request(`/users/${id}`, {
      method: 'DELETE',
    });
  }
}

// Usage
const api = new UserAPI();

// Get users
const users = await api.getUsers({ page: 1, limit: 20, status: 'active' });

// Create user
const newUser = await api.createUser({
  name: 'Alice',
  email: 'alice@example.com',
  age: 25
});

// Update user
await api.updateUser(123, { status: 'inactive' });

// Delete user
await api.deleteUser(123);
```

---

## Interview Questions

### 1. What is REST?
**Answer**: Architectural style for designing networked applications using HTTP methods for CRUD operations. Key principles: stateless, client-server, cacheable, uniform interface.

### 2. PUT vs PATCH?
**Answer**:
- **PUT**: Full replacement of resource (send all fields)
- **PATCH**: Partial update (send only changed fields)

### 3. What makes an API RESTful?
**Answer**:
- Uses standard HTTP methods
- Resource-based URLs (nouns, not verbs)
- Stateless requests
- Returns standard status codes
- Uses JSON for data exchange

### 4. How do you version an API?
**Answer**:
- URL versioning: `/api/v1/users`, `/api/v2/users`
- Header versioning: `Accept: application/vnd.api.v2+json`
- Query parameter: `/api/users?version=2`

### 5. What is idempotency?
**Answer**: Multiple identical requests have same effect as one request. GET, PUT, DELETE are idempotent. POST is not.

### 6. How do you handle pagination?
**Answer**:
```bash
GET /api/users?page=2&limit=20
```
Response includes pagination metadata (total, pages, links).

### 7. What is HATEOAS?
**Answer**: Hypermedia As The Engine Of Application State. API responses include links to related resources.

### 8. How do you handle errors in REST APIs?
**Answer**:
- Return appropriate status codes (400, 404, 500)
- Include error details in response body
- Use consistent error format
- Log errors server-side

### 9. What is rate limiting?
**Answer**: Restricting number of requests a client can make in a time period. Prevents abuse. Returns 429 status code when exceeded.

### 10. REST vs GraphQL?
**Answer**:
- **REST**: Multiple endpoints, over-fetching/under-fetching, simpler, cacheable
- **GraphQL**: Single endpoint, exact data needed, more complex, powerful queries

---

## Practice Exercise

**Build a Simple REST API**

Create a REST API for a blog with:
- Users
- Posts
- Comments

**Requirements:**
1. CRUD operations for all resources
2. Nested routes (user's posts, post's comments)
3. Filtering and pagination
4. Authentication
5. Error handling
6. Input validation

**Bonus:**
- Add rate limiting
- Implement soft delete
- Add search functionality
- Create API documentation

---

## Resources

- [REST API Tutorial](https://restfulapi.net/)
- [HTTP Status Dogs](https://httpstatusdogs.com/) - Fun way to learn status codes
- [Postman](https://www.postman.com/) - API testing tool
- [Swagger](https://swagger.io/) - API documentation
- [JSON Placeholder](https://jsonplaceholder.typicode.com/) - Fake REST API for testing

---

**Keywords**: REST API, RESTful web services, HTTP methods, API design, CRUD operations, API authentication, status codes, API best practices, web services, backend development, API interview questions

