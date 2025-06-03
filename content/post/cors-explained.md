---
date: "2025-06-03T00:50:42-07:00"
draft: false
title: "Cors Explained"
cover:
  image: "/post/img/cors.jpg"
  alt: "cors image"
---

## Cross-Origin Resource Sharing Explained

If you've ever worked with APIs—whether consuming them or building your own—you’re probably familiar with this frustrating error message:

```pgsql
Access to fetch at 'http://api.example.com/data' from origin 'https://<your-website.com>' has been blocked by CORS policy.
No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

I, for one, know how much I’ve suffered because of this error. I finish building my API, everything works perfectly when tested with [Postman](https://postman.com/), but the moment I try it in the browser—**BOOM**—I get an error, or worse, nothing shows up. The data isn’t sent properly. I open the browser's `devtools`, check the console, and it’s usually this dreaded error.

So I hop onto [Google](https://google.com/), find a quick fix, and move on with the rest of my project. I'm sure that’s how it goes for most people.

But one day, while scrolling through [LinkedIn](https://linkedin.com/), I came across a post about CORS and the infamous `Access-Control-Allow-Origin` error.

The author pointed out that most developers just search for a workaround without ever really understanding what the error means or why it happens.

He went on to explain what CORS is, and it turned out to be much simpler than I had thought.

I always assumed it was some complex concept far beyond my understanding—but it wasn’t.

In this article, I’ll explain what CORS is, and what that error message is actually trying to tell you.

![app screenshot](/post/img/guy.jpg)

---

### What is CORS?

CORS (Cross-Origin Resource Sharing) is a security mechanism implemented by web browsers. It controls how web applications running on one _origin_ (a combination of domain, protocol, and port) can request resources from a different origin.

If that sounds confusing, here’s an analogy to help you understand:

---

**Picture this**: You walk into Bank A and ask to withdraw money from your account at Bank B. **Bank B (the server)** has a strict policy:

> "We only allow withdrawals if the request comes from one of our own branches (same origin). If someone tries from another bank (cross-origin), they must present a special permission slip (CORS headers like `Access-Control-Allow-Origin`)."

**Bank A (the browser)** checks with Bank B first: "Does my customer have permission to access this?" If Bank B says no, the transaction is blocked.

---

Here’s another analogy:

**Picture this (again)**: You show up at an exclusive club (**Website B**), but the bouncer (**Browser**) stops you because your name isn’t on the guest list.

The **Club Owner (Server)** provides a guest list:  
`Access-Control-Allow-Origin: https://websiteA.com`

If you came from **Website A**, you’re allowed in. If you came from somewhere else (like `https://websiteC.com`), the bouncer denies entry.

---

CORS is essentially a browser-enforced protocol that governs how requests are sent from the frontend (client) to the backend (server).

Now that you have an intuitive sense of what CORS means, let’s dive into how it actually works.

---

### How It Works

#### **1. Headers Define Permissions**

When a browser makes a request to a server from a different origin, the server can respond with specific headers to allow or deny the request. These include:

- `Access-Control-Allow-Origin` – Specifies which origins are allowed (e.g., `https://example.com`, or `*` to allow all).
- `Access-Control-Allow-Methods` – Specifies which HTTP methods are allowed (e.g., `GET`, `POST`).
- `Access-Control-Allow-Headers` – Specifies which request headers are permitted.

These headers tell the browser what it's allowed to do with the response.

#### **2. Preflight Requests (For Complex Requests)**

For certain types of requests (e.g., those using methods like `PUT` or `DELETE`, or custom headers), the browser sends a preliminary `OPTIONS` request—called a _preflight request_—to check if the actual request is allowed.

Only if the server responds affirmatively does the browser proceed with the actual request.

---

### Key Classifications

- CORS restricts access based on **origin**, which includes scheme, domain, and port.
  For example:  
   `https://site.com` ≠ `http://site.com` (different protocol) → request blocked unless CORS headers are present.
- By setting the `Access-Control-Allow-Origin` header, the server tells the browser which origins are allowed. If the origin isn’t listed, the browser blocks the response from being read by frontend JavaScript (even if the server successfully sent it).

---

### ⚠️ Security Note

CORS doesn't prevent someone from sending unauthorized requests (e.g., using `curl` or Postman). It’s a browser-enforced security measure that protects users from malicious cross-origin requests in browser environments (e.g., from scripts trying to hijack session data).

---

### Conclusion

CORS (Cross-Origin Resource Sharing) is a browser security feature that protects users by enforcing restrictions on cross-origin requests.

By setting headers like `Access-Control-Allow-Origin`, the **server** tells the **browser** who’s allowed to access its resources. The **browser** enforces these rules, blocking frontend JavaScript from accessing responses unless permission is explicitly granted.
