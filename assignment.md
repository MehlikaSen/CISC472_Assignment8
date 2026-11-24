**Hosted application:** [https://cisc472-assignment8.onrender.com](https://cisc472-assignment8.onrender.com)

# Broken Access Control

**Location:** `server.js`, `/internal` route

**Trigger:** Sending `GET /internal` from an unauthenticated browser or HTTP client.

*Example:* An attacker opens `https://cisc472-assignment8.onrender.com/internal`. The endpoint returns:

```text
Desk note. Not for the public preview list. Release code PREVIEW-4418. Rotate after the spring screening.
```

This happens because `server.js` registers the `/internal` route without any authentication or authorization check. The endpoint also publicly exposes the sensitive `PREVIEW-4418` release code.

**The Fix**:

The `/internal` route was removed from `server.js` because the application has no authentication or authorization system. The route now returns `Cannot GET /internal`, so it no longer exposes the desk note or release code. The exposed release code `PREVIEW-4418` should also be considered compromised and rotated.

**what happens after you submit a URL:**

The browser sends `POST /preview` to the Node.js server. The server reads `req.body.url`, rejects an empty value with a `400` response, fetches the URL from the server, converts the response into text, extracts the title and first portion of text, and returns a new preview card in HTML.

**which process makes the request:**

The Node.js/Express process running `server.js` makes the request using the server-side `fetch(url)` call. The browser only sends the URL to the server; it does not directly fetch the submitted page. A fetch failure produces a `502` response.

**what the home page shows versus what the server will fetch:**

The home page initially shows the two cards stored in the `saved` array: Robotics club and Film society. It does not automatically fetch those URLs. When a user submits a URL, the server fetches the submitted external URL and displays a temporary result card containing that page's title, text snippet, URL, and HTTP status.

**what other routes exist in `server.js`:**

- `GET /health` returns `{ "ok": true }` as JSON.
- `GET /` serves `public/index.html`.
- `POST /preview` fetches the submitted URL and returns a generated preview card.
- Static files in `public/` are served by `express.static()`.
