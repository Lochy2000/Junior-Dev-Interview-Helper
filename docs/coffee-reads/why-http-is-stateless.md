# Why HTTP Is Stateless (And Why That's Weird) ☕

> **3 min read** · Web Fundamentals

Imagine you walk into a coffee shop. You order your usual. The barista knows your name. Next Tuesday you walk in again — same barista, same shop — and they have no idea who you are.

That's HTTP.

---

## What Stateless Means

Every HTTP request is **completely independent**. The server processes it, sends a response, and immediately forgets you exist.

```
Request 1:  GET /home        → Server: "here's the homepage"    → *forgets*
Request 2:  GET /dashboard   → Server: "who are you again?"
Request 3:  POST /login      → Server: "ok logged in, bye"      → *forgets*
Request 4:  GET /dashboard   → Server: "who are you again?"
```

There's no built-in concept of a "session" or "user" between requests. Each one arrives as a blank slate.

---

## Why Was It Designed This Way?

HTTP was invented in 1989 for sharing academic documents. The original use case was simple: *request a document, receive a document*.

No logins. No shopping carts. No "you have 3 unread notifications".

Statelessness made HTTP **simple and scalable**. Servers don't need to remember anything between requests — which means any server in a cluster can handle any request, because no server holds any special information about any user.

```
                     ┌─── Server A ───┐
User ──► Load ──────►│                │
         Balancer    └───────────────┘
              └─────►┌─── Server B ───┐
                     │                │
                     └───────────────┘

Because HTTP is stateless, Server A and Server B are interchangeable.
Any request can go to either. No server "owns" a user.
```

---

## So How Does Login Work Then?

When you log in to a website, the server can't *remember* you between requests. So it gives you a **token** — a piece of data you carry and present with every subsequent request, proving who you are.

Two common approaches:

**Sessions (old-school):**
```
1. You log in → Server creates a session, gives you a cookie:
   Set-Cookie: session_id=abc123

2. Every future request includes:
   Cookie: session_id=abc123

3. Server looks up abc123 in its session store → "ah, that's Sarah"
```

**JWT (modern):**
```
1. You log in → Server gives you a token:
   { "userId": 42, "name": "Sarah" } [signed with secret key]

2. You send the token with every request:
   Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...

3. Server verifies the signature → "token is valid, user is Sarah"
   (No database lookup needed!)
```

In both cases, **you're carrying the state**. The server is still stateless — it just validates what you bring.

---

## The Trade-Off

Statelessness means servers are simple and scalable. But it also means:

- **Every request is slightly larger** — you're sending identity info each time
- **You have to manage state yourself** — via cookies, tokens, local storage
- **Logout is tricky** — with JWTs especially, you can't "cancel" a token unless you maintain a blocklist (which is… stateful)

Modern web development is largely the art of building stateful experiences on top of a stateless protocol.

---

## The Takeaway

> HTTP is stateless by design — each request stands alone. This keeps servers simple and scalable. Everything we experience as "logged in" or "session" is a layer we've built on top: cookies, tokens, and local storage that re-establish identity with each request.

---

## Want to Go Deeper?

- 📄 [HTTP Basics Cheat Sheet](../cheat-sheets/http-basics.md) — methods, headers, status codes
- 📄 [REST APIs Guide](../backend/rest-apis.md) — how statelessness applies to API design

---

*← [Back to Coffee Reads](./README.md)*
