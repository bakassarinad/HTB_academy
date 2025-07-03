# Basic Tools

Question: Apply what you learned in this section to grab the banner of the above server and submit it as the answer.

Answer: SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.1

netstat <ip port>

# Service Scanning

Question: Perform an Nmap scan of the target. What does Nmap display as the version of the service running on port 8080?

Answer: Apache Tomcat

Question: Perform an Nmap scan of the target and identify the non-default port that the telnet service is running on.

Answer: 2323

Question: List the SMB shares available on the target host. Connect to the available share as the bob user. Once connected, access the folder called 'flag' and submit the contents of the flag.txt file.

Answer: dceece590f3284c3866305eb2473d099

Usage: smbclient 

# Web Enumeration 

Question: Try running some of the web enumeration techniques you learned in this section on the server above, and use the info you get to get the flag.

Answer: HTB{w3b_3num3r4710n_r3v34l5_53cr375}

# Public Exploit

Question: Try to identify the services running on the server above, and then try to search to find public exploits to exploit them. Once you do, try to get the content of the '/flag.txt' file. (note: the web server may take a few seconds to start)

Answer: HTB{my_f1r57_h4ck}

# Privilege Escalation
Kernel Exploits: Linux version 3.9.0-73-generic - DirtyCow exploit. 

Vulnerable Software: Linux - dpkg -l Windows - C:\Program Files

User Privilege: 1. Sudo 2. SUID 3. Windows Token Privileges 
Command: sudo -l
sudo su 
GTFOBins
LOLBAS

Scheduled Tasks: 
1. Add a new cron job 
2. Trick a job to execute a malware

/etc/crontab
/etc/cron.d
/var/spool/cron/crontabs/root

Exposed credentials:

SSH Keys:
/home/user/.ssh/id_rsa or /root/.ssh/id_rsa

Command: ssh root@10.10.10.10 -i id_rsa

Question: SSH into the server above with the provided credentials, and use the '-p xxxxxx' to specify the port shown above. Once you login, try to find a way to move to 'user2', to get the flag in '/home/user2/flag.txt'.

Answer: HTB{l473r4l_m0v3m3n7_70_4n07h3r_u53r}

Question: Once you gain access to 'user2', try to find a way to escalate your privileges to root, to get the flag in '/root/flag.txt'.

Answer: HTB{pr1v1l363_35c4l4710n_2_r007}