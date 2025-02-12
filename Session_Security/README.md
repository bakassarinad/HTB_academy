# Session Hijacking

Explanation: For session hijacking of a cookie in Storage from Web Tools use the Private Browsing

# Session Fixation

Stage 1. Attacker obtains a valid session.
Stage 2. Attacker fixates a valid session.
Stage 3. Attacker tricks the victim into establishing a session.

Vulnrable Code 

```
<?php
    if (!isset($_GET["token"])) { # if the token is not defined, then:
        session_start(); # start a session
        header("Location: /?redirect_uri=/complete.html&token=" . session_id()); # session_id value is a token  
    } else { # if token is defined
        setcookie("PHPSESSID", $_GET["token"]); # set PHPSESSID to the token.
    }
?>
```

# Obtaining Session Identifiers without User Interaction

Traffic Sniffing requires the attacker and the victim to be on same LAN. 
Unencrypted HTTP traffic

# Cross-Site Scripting

 Session via XSS:
 1. Session iscarried in all  HTTP requests
 2. Session are accessible by JavaScript code

# Cross-Site Request Forgery

CSRF:
1. All parameters are guessable by an attacker
2. Session management is based on cookies
