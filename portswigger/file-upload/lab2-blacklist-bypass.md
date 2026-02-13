# Lab 2 - Content-Type restriction bypass

## Lab Overview
Server checks Content-Type header.

## Vulnerability Type
Client-side Content-Type validation bypass.

## Exploitation Steps
1, upload shell.php
2, intercept request using Burp
3, change:
```
Content-Type: image/png
```
4,forward request

## Key Point
Content-Type is controlled by client -> can be modified.

## Security Fix
Sever must validate MIME type using backend functions.

