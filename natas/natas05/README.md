# Natas Level 5 - Authentication Bypass via Cookie Manipulation
## Vulnerability
This level demonstrates improper trustin client-side cookies to determine authentication state.
## Analysis
The application denies access if the user is not logged in.
Authentication statu is determined using a cookie value.
Because cookies are controlled by the client,they can be modified.
## Exploitation
The HTTP request was intercepted using Burp Suite.
A cookie indicating authentication status was added/modified:
 Cookie:loggedin=1
The forged request was then sent to the server.
## Result
The server trusted the manipulated cookie and revealed the password for natas6.
## Takeaway
-Cookies are ser-controlled data
-Authentication decisions must be validated server-side
