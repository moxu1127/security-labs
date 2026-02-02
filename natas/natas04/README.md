# Natas Level 4 - Referer Header Trust Bypass
## Vulnerability
This level demonstrates improper improper trust in the HTTP 'Referer' header for access control.
## Analysis
The application restricts access based on the request source.
If the request does not originate from a trusted page,access is denied.
However,HTTP header are fully controlled by the client.
## Exploitation
The request was intercepted using Burp Suite.
A fake 'Referer' header was added:Referer:http://natas5.natas.labs.overthewire.org/
The modified request was then forwarded to the server.
## Result
The server accepted the forged request and revealed the password for natas5.
## Takeaway
- HTTP headers can be easily manipulated 
- 'Referer' must never be used as an authentication or authorizatio mechanism
