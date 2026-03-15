HTB Writeup: Meow
Difficulty: Very Easy

OS: Linux

Target IP: 10.129.24.131

1. Introduction
Meow is an introductory machine in the "Starting Point" track. The goal is to understand how to perform basic enumeration and identify insecure administrative services—in this case, Telnet.

2. Enumeration
Nmap Scan
First, we perform a service scan to see what ports are open.

Bash
nmap -sV 10.129.24.131
Results:

Port 23/tcp: Open (Telnet)

Service: Linux telnetd

Observation: Telnet is an unencrypted protocol. Unlike SSH, it often has weak configurations in older or laboratory environments.

3. Exploitation
Connecting via Telnet
We attempt to connect to the target using the telnet client:

Bash
telnet 10.129.24.131
The system prompts for a login. On many misconfigured Linux systems or networking hardware, the administrative account is root.

Username: root

Password: (Tried pressing Enter/Blank)

Success! The system allowed a login as root without requiring a password.

4. Post-Exploitation / Flag
Once logged in, we check the current directory and the user's home folder for the flag.

Bash
whoami
# Output: root

ls
# Output: flag.txt

cat flag.txt
Flag: b40abdfe11b5a0346083f25603780360

5. Summary & Key Takeaways
Service Enumeration: Port 23 is a red flag in modern security; it should almost always be replaced by SSH (Port 22).

Insecure Defaults: The root account had no password set, allowing instant administrative access.

Tooling: Basic usage of nmap and telnet.
