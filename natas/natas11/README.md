#  Natas Level 11 Writeup - XOR Encrypted Cookie Bypass

## Challenge Overview
This level stores user state inside a cookie named data.

The cookie is:
'''
base64_encode(xor_encrypt(json_data))
'''
The server checks whether:
'''
"showpassword":"yes"
'''
If so,it reveals the password for the next level.

## Relevant Source Code
'''
$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

function xor_encrypt($in) {
    $key = '<censored>';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}

function loadData($def) {
    global $_COOKIE;
    $mydata = $def;
    if(array_key_exists("data", $_COOKIE)) {
    $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
    if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
        if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
        $mydata['showpassword'] = $tempdata['showpassword'];
        $mydata['bgcolor'] = $tempdata['bgcolor'];
        }
    }
    }
    return $mydata;
}
'''
## Vulnerability Analysis
The application trusts client-side encrypted data.
Key issues:
  XOR is reversible
  No integrity check (no HMAC/signature)
  Server assumes:
   "If I can decrypt it, it must be valid"
As a result, any user who knows the key can forge arbitrary data.

## Exploitation Strategy
Step 1: Obtain Original Cookie
Example cookie:
'''
HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg%3D
'''
URL-decode first,then base64-decode.

Step 2: Recover XOR Key
We know the plaintext structure
'''
{"showpassword":"no","bgcolor":"#ffffff"}
'''
Using XOR properties:
ciphertext ^ plaintext = key

The recovered key is:
'''
eDWo
'''
Step 3: Forge a New Cookie
Target payload:
'''
{"showpassword":"yes","bgcolor":"#ffffff"}
'''
Python code used:
'''
import base64
def xor(data,key):
   out = b''
   for i in range(len(data)):
       out += bytees([data[i] ^ key[i % len(key)]])
   return out

new_plain = b'{"showpassword":"yes","bgcolor":"#ffffff"}'
key = b'eDWo'

new_cookie = base64.b64encode(xor(new_plain,key))
print(new_cookie.decode())
'''
Sept 4: Replace Cookie
Set the browser cookie:
'''
data=<new_cookie_value>
'''
Refresh the page -> password for natas12 is revealed!

## Root Cause
-Client-side data is treated as trusted state
-Encryption without authentication != security
-XOR provides no integrity or authenticity

## Security Lesson
Encryption does not equal trust.

Correct solutions would include:
-Server-side session storage
-HMAC-signed cookies
-Authenticated encryption (e.g. AES-GCM)

## Takeaway
This challenge demonstrates how reversible encryption combined with blind trust leads to privilege escalation.

