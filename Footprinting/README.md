# FTP

Question: Which version of the FTP server is running on the target system? Submit the entire banner as the answer.

Answer: InFreight FTP v1.1

Command: nmap -sV 10.129.88.218 -p 21 -sC -A

Question: Enumerate the FTP server and find the flag.txt file. Submit the contents of it as the answer.

Answer: HTB{b7skjr4c76zhsds7fzhd4k3ujg7nhdjre}

# SMB

Question: What version of the SMB server is running on the target system? Submit the entire banner as the answer.

Command: nmap -sV ip -p137,445

Answer: Samba smbd 4.6.2

Question: What is the name of the accessible share on the target?

Command: smbclient -N -L //ip

Answer: sambashare

Question: Connect to the discovered share and find the flag.txt file. Submit the contents as the answer.

Command: smbclient //ip/sambashare
Command: help
Command: get /content/flag.txt

Answer: HTB{o873nz4xdo873n4zo873zn4fksuhldsf}

Question: Find out which domain the server belongs to.

Command: ./enum4linux-ng.py ip

Answer: DEVOPS

Question:  Find additional information about the specific share we found previously and submit the customized version of that specific share as the answer.

Command: smbclient -N -L //ip

Answer: InFreight SMB v3.1

Question:  What is the full system path of that specific share? (format: "/directory/names")

Command:  rpcclient $> netsharegetinfo sambashare

Answer: /home/sambauser

# NFS

Question: Enumerate the NFS service and submit the contents of the flag.txt in the "nfs" share as the answer.

Command: nmap -sV -sC --script nfs* 10.129.202.5 -p111,2049


Answer: HTB{hjglmvtkjhlkfuhgi734zthrie7rjmdze}

Question:  Enumerate the NFS service and submit the contents of the flag.txt in the "nfsshare" share as the answer.

Answer: HTB{8o7435zhtuih7fztdrzuhdhkfjcn7ghi4357ndcthzuc7rtfghu34}

Commands: 
$ mkdir nfs
$ showmount -e 10.129.202.5
$ sudo mount -t nfsshare 10.129.202.5:/ ./nfs -o nolock
$ sudo mount -t nfs 10.129.202.5:/ ./nfs -o nolock
$ cd nfs
$ find . -type f -name 'flag*'


# DNS 

Question:  Interact with the target DNS using its IP address and enumerate the FQDN of it for the "inlanefreight.htb" domain.

Command: dig any inlanefreight.htb @ip

Answer: ns.inlanefreight.htb

Question: Identify if its possible to perform a zone transfer and submit the TXT record as the answer. (Format: HTB{...})

Command: dig axfr internal.inlanefreight.htb @ip

Answer:HTB{DN5_z0N3_7r4N5F3r_iskdufhcnlu34}

Question: What is the IPv4 address of the hostname DC1?

Answer: 10.129.34.16

Question: What is the FQDN of the host where the last octet ends with "x.x.x.203"?

Useful URL: https://www.reddit.com/r/hackthebox/comments/12fw8qc/what_is_the_fqdn_of_the_host_where_the_last_octet/

Command: dnsenum --dnsserver 10.129.75.153 --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt dev.inlanefreight.htb

Answer: win2k.dev.inlanefreight.htb

# SMTP

The Simple Mail Transfer Protocol is a protocol for sending emails in an IP network. Ports: 25, 587.

Question: Enumerate the SMTP service and submit the banner, including its version as the answer.

Answer: InFreight ESMTP v2.11

Usage: telnet <IP> 25

Question: Enumerate the SMTP service even further and find the username that exists on the system. Submit it as the answer.

Answer: robin

Usage:

msfconsole

use smtp_enum

set rhosts <IP>

set USER_FILE /home/htb-ac-611037/Desktop/names.txt (names.txt is the Resources dictionary of usernames)

run

# IMAP/POP3

Question: Figure out the exact organization name from the IMAP/POP3 service and submit it as the answer.

Answer: InlaneFreight Ltd

Question: What is the FQDN that the IMAP and POP3 servers are assigned to?

Answer: dev.inlanefreight.htb

Notes: sudo nmap -sV 10.129.11.28 -p110,143,993,995 -v -sC

Question: Enumerate the IMAP service and submit the flag as the answer. (Format: HTB{...})


Answer: HTB{roncfbw7iszerd7shni7jr2343zhrj}

Notes: curl -k 'imaps://10.129.11.28' --user robin:robin -v

Question: What is the customized version of the POP3 server?

Answer: InFreight POP3 v9.188

Notes: openssl s_client -connect 10.129.11.28:pop3s

Question: What is the admin email address?

Answer: devadmin@inlanefreight.htb

Notes: openssl s_client -connect 10.129.11.28:imaps

1 LOGIN robin robin
1 lIST “” *
1 SELECT DEV.DEPARTMENT.INT
1 fetch 1 all


Question: Try to access the emails on the IMAP server and submit the flag as the answer. (Format: HTB{...})

Answer: HTB{983uzn8jmfgpd8jmof8c34n7zio}

Notes: 1 FETCH 1 BODY[TEXT]