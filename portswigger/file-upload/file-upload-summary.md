# File Upload Vulnerabilities - Complete Summary

## 1. Why File Upload Is Dangerous
File upload becomes critical when:
- Uploaded files are stored in web root
- Server executes uploaded files
- File type validation is weak
- User controls filename
Result:
   Remote Code Execution(RCE)
## 2. Common File Upload Vulnerability Type
- Unrestricted File Upload
   - Example
     Upload shell.php directly.

   - Cause
     No validation of:
        -- Extension
        -- MIME type
        -- Content

   - Exploit
     Access:
     ```
      /uploads/shell.php
     ```

   - Related Lab
     Remote code execution via web shell upload

- Content-Type Validation Bypass
   - Example
     Server checks:
     ```
     Content-Type: image/png
     ```
     But attacker changes header in Burp.
   - Root Problem
     Content-Type is client-controlled.

- Blacklist Bypass
   - Example
     Server block .php
     Attacker uses:
     -- .php5
     -- .phtml
     -- .php.jpg
   - Root Problem
     Blacklist is incomplete.
   - Fix
     Use whitelist:
     ```
     Allow only: jpg,png,gif
     ```

- Path Traversal in File Upload 
   - Vulnerable Code
   ```php
     $filename = $_FILES['file']['name'];
     move_uploaded_file(
       $_FLIES['file']['tmp_name'],
       "/var/www/html/uploads/" . $filename
     );
   ```
   - Exploit
     Upload with:
     ```
     filename="../shell.php"
     ```
     File saved outside upload directory.
   - Related Lab
     Web shell upload via path traversal

- Magic Byte / Image Validation Bypass
   - Example
     Server checks:
     ```
     exif_imagetype()
     ```
     Attacker prepends JPEG magic bytes:
     ```
     ÿØÿ
     ```
     to PHP shell.
   - Related Lab
     Natas13

## 3. Real-World Exploitation Flow
   1.Upload PHP shell
   2.If blocked -> try:
      -- Change Content-Type
      -- Bypass extension
      -- Add double extension
      -- Use path traversal
      -- Add magic bytes
   3.Locate uploaded file
   4.Execute
   5.Gain shell

## 4. Secure Implementation
A secure file upload system should:
-> Use whitelist
Only allow specific extensions.

-> Validate server-side
Use:
-- finfo_file()
-- MIME detection
-- Content analysis

-> Rename files
Generate random filename:
```php
$filename = uniqid() . ".jpg";
```

-> Store outside web root
Bad:
```
/var/www/html/uploads/
```
Better:
```
/var/www/uploads/
```

-> Disable execution in upload folder
Example:
```
php_flag engine off
```

## 5. My Reflection

Through practicing file upload vulnerabilities in both Natas and PortSwigger labs, I understood:
 -- Client-side checks are meaningless
 -- Blacklists are weak
 -- Path traversal can escalate impact
 -- Real security requires strict server-side validation

This vulnerability class frequently leads to Remote Code Execution and is considered high serverity in real-world penetration testing.

