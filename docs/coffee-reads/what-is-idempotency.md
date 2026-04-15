# What Is Idempotency and Why Should You Care ☕

> **3 min read** · APIs & Web Concepts

Here's a scenario.

You click "Submit Order" on a shopping website. The request seems to hang. You're not sure if it went through. You click again.

Did you just place two orders?

The answer depends on whether the API is **idempotent**.

---

## The Definition

An operation is **idempotent** if doing it multiple times has the same effect as doing it once.

More formally: `f(f(x)) = f(x)`

In plain English: *running it again doesn't change anything.*

---

## HTTP Methods and Idempotency

The HTTP spec defines which methods should be idempotent:

| Method | Idempotent? | Why |
|--------|-------------|-----|
| `GET` | ✅ Yes | Just reads data — same result every time |
| `PUT` | ✅ Yes | Sets a resource to a specific state — doing it again leaves it the same |
| `DELETE` | ✅ Yes | Resource is gone after first call — second call, still gone |
| `POST` | ❌ No | Creates a new resource — calling twice creates two |
| `PATCH` | ❌ Usually no | Depends on the operation |

---

## The Shopping Example

**`POST /orders`** — creates a new order.

```
Click 1 → POST /orders → Order #1001 created ✓
Click 2 → POST /orders → Order #1002 created  ← double order!
```

`POST` is not idempotent. Two requests, two orders.

**`PUT /orders/1001/status`** — sets an order to "confirmed".

```
Click 1 → PUT /orders/1001/status { "status": "confirmed" } → Updated ✓
Click 2 → PUT /orders/1001/status { "status": "confirmed" } → Still confirmed
```

`PUT` is idempotent. Same result whether you call it once or ten times.

---

## Why It Matters in Real Systems

Networks are unreliable. Requests time out. Users click twice. Mobile apps retry after a bad connection.

When operations are idempotent, **retrying is safe**. When they're not, retrying can cause duplicate actions — double charges, duplicate emails, multiple records.

The common solutions for non-idempotent operations:

**Idempotency keys:**
```http
POST /payments HTTP/1.1
Idempotency-Key: a1b2c3d4-unique-per-request

{
  "amount": 50,
  "currency": "GBP"
}
```

The server stores the key. If it sees the same key again, it returns the original response instead of processing a second payment. This is how Stripe handles payments.

**Checking before acting:**
```javascript
// Instead of just inserting, check first
const existing = await Order.findOne({ userId, cartId });
if (!existing) {
  await Order.create({ userId, cartId, items });
}
```

---

## Beyond HTTP

Idempotency matters anywhere you have unreliable operations:

- **Database writes** — use `INSERT OR IGNORE` or `UPSERT` instead of plain `INSERT`
- **Message queues** — consumers should handle duplicate messages gracefully
- **Background jobs** — a job that runs twice should produce the same result as one run

---

## The Takeaway

> An idempotent operation produces the same result no matter how many times you call it. GET, PUT, and DELETE should be idempotent. POST usually isn't. This matters because networks fail and retries happen — idempotent APIs are safer to retry without side effects.

This comes up in API design interviews and backend roles. Knowing not just the definition but *why it matters* (retry safety, distributed systems) shows real understanding.

---

## Want to Go Deeper?

- 📄 [REST APIs Guide](../backend/rest-apis.md) — HTTP methods, API design principles
- 📄 [HTTP Basics Cheat Sheet](../cheat-sheets/http-basics.md) — method properties and status codes

---

*← [Back to Coffee Reads](./README.md)*
