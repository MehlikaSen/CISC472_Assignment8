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

Remove the `/internal` route or protect it with authentication and authorization so only authorized users can access it. If the route must remain in the application, require a logged-in session and explicitly verify that the user has permission to view internal notes. The exposed release code `PREVIEW-4418` should also be considered compromised and rotated.
