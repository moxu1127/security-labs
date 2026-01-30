# Natas Level 1 — Basic HTTP Authentication

## 🧠 Vulnerability
This level demonstrates the insecurity of HTTP Basic Authentication.

## 🔍 Analysis
The server protects the page using HTTP Basic Authentication.
Credentials are transmitted in the `Authorization` header as a Base64-encoded string.

## 🛠 Exploitation
The Authorization header follows this format:

Authorization: Basic base64(username:password)

The encoded value can be decoded locally:

```bash
echo bmF0YXMxOjBuekNpZ0FxN3QyaUFMeXZVOXhjSGxZTjRNbGtJd2xx | base64 -d
✅ Result

Successfully authenticated and accessed the protected resource.

📌 Takeaway

Base64 is encoding, not encryption. Client-side restrictions do not provide real security.


