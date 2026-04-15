# What Actually Happens When You Type a URL? ☕

> **4 min read** · Web Fundamentals

You type `https://google.com` and press Enter.

A webpage appears in under a second.

But between those two events, your computer made dozens of decisions, sent packets halfway around the world, negotiated a secure connection, and reassembled a pile of text into something visual. Here's the short version.

---

## Step 1 — Your Browser Checks Its Memory

Before anything hits the network, your browser looks up `google.com` in a few places:

1. **Browser cache** — did you visit recently? Saved answer.
2. **OS cache** — has your computer seen it?
3. **Router cache** — has your router seen it?
4. **ISP cache** — has your internet provider seen it?

If any of them know the answer, the lookup stops there. If not, we go to step 2.

---

## Step 2 — DNS: Translating the Name Into a Number

Humans remember `google.com`. Computers need `142.250.80.46`.

**DNS (Domain Name System)** is the phonebook that converts one to the other.

```
You: "Where is google.com?"
DNS: "That's 142.250.80.46"
```

This involves a chain of servers — your ISP's resolver asks a root server, which points to `.com` servers, which point to Google's own nameservers. It's fast because of caching, but the chain exists.

---

## Step 3 — TCP Handshake: Knocking on the Door

Now your computer knows the IP address. It opens a connection to Google's server using **TCP** — a protocol that guarantees data arrives in order and nothing gets lost.

The handshake looks like this:

```
Your computer:  "SYN" (I want to connect)
Google server:  "SYN-ACK" (Got it, go ahead)
Your computer:  "ACK" (Great, let's go)
```

Three messages. Connection established.

---

## Step 4 — TLS: Making It Private

You typed `https://` — the `s` stands for **secure**. Before sending anything real, your computer and Google's server negotiate encryption.

They agree on:
- Which encryption method to use
- A shared secret key (without ever sending it directly)
- A verification that Google is actually Google (via SSL certificate)

This is why the padlock appears in your address bar.

---

## Step 5 — The HTTP Request

Now your browser sends an actual request:

```http
GET / HTTP/2
Host: google.com
User-Agent: Chrome/123
Accept: text/html
```

Translation: *"Give me the homepage, please."*

---

## Step 6 — The Server Responds

Google's server sends back:

```http
HTTP/2 200 OK
Content-Type: text/html

<!DOCTYPE html>
<html>
  ...the actual page...
</html>
```

Along with a bunch of headers — instructions on caching, cookies, content type, and more.

---

## Step 7 — Rendering

Your browser receives the HTML and starts building the page. As it reads through:

- It finds a `<link>` tag — fetches the CSS
- It finds a `<script>` tag — fetches the JavaScript
- It finds `<img>` tags — fetches the images

Each resource potentially triggers its own mini version of everything above. A single page load can involve **dozens of separate requests**.

The browser builds the **DOM** (Document Object Model) from the HTML, applies styles, runs JavaScript, and paints pixels to your screen.

---

## The Whole Thing, Visualised

```
You hit Enter
      │
      ▼
DNS lookup → IP address
      │
      ▼
TCP handshake → connection open
      │
      ▼
TLS negotiation → encrypted channel
      │
      ▼
HTTP GET request sent
      │
      ▼
Server processes request
      │
      ▼
HTTP response received (HTML)
      │
      ▼
Browser fetches CSS, JS, images
      │
      ▼
Page rendered on screen
```

All of this in under 500ms on a good connection.

---

## The Takeaway

> Every webpage is the result of your browser making a series of promises with remote servers — where to find things, how to talk securely, and what to display. When something breaks, it usually breaks at one of these steps.

Knowing this makes you better at debugging. "Is it DNS? Is it the server? Is it the response?" suddenly means something.

---

## Want to Go Deeper?

- 📄 [HTTP Basics Cheat Sheet](../cheat-sheets/http-basics.md) — methods, status codes, headers
- 📄 [REST APIs Guide](../backend/rest-apis.md) — how servers handle requests
- 🌐 [What Happens When (GitHub)](https://github.com/alex/what-happens-when) — the exhaustive version of this article

---

*← [Back to Coffee Reads](./README.md)*
