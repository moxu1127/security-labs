# Natas Level 6 - Information Disclosure via Included File

## Vulnerability
This level exposes sensitive configuration data through an included file 
that is publicly accessible.

## Analysis
The application validates user input against a secret value.
The secret is not hardcoded but loaded using PHP 'include' :
include"includes/secret.inc";
The included file is not protected and can be accessed directly.

## Exploitation
The following file was accessed directly via browser:
/includes/secret.inc
This revealed the secret value required by the application.

## Result
Submitting the extracted secret granted access and revealed the password for natas7.

## Takeaway
-Included files may still be publicly accessible
-Sensitive data must never be exposed in web-accessible files
