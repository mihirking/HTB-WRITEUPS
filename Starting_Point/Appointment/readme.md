HTB Writeup: Appointment
Difficulty: Very Easy

OS: Linux

Target IP: 10.129.X.X

# 1. Introduction
Appointment is a Tier 1 "Starting Point" machine that focuses on Web Application Security. It demonstrates a classic SQL Injection (SQLi) vulnerability on a login form, allowing an attacker to bypass authentication without a valid password.

# 2. Enumeration
Nmap Scan
We start by identifying the services running on the target.

Bash
nmap -sV 10.129.X.X
Results:

Port 80/tcp: Open (Apache httpd 2.4.41)

Observation: Since only Port 80 is open, the entire attack surface is the web application. Navigating to the IP address reveals a standardized login page.

# 3. Exploitation
Authentication Bypass via SQL Injection
The login form takes a username and password. If the backend code does not "sanitize" the input, we can manipulate the SQL query sent to the database.

The Logic:
Usually, the backend query looks like this:
SELECT * FROM users WHERE username = '$user' AND password = '$password';

By entering admin'# as the username, the query becomes:
SELECT * FROM users WHERE username = 'admin'#' AND password = '...';

The ' closes the username string.

The # (in MySQL) comments out the rest of the query, meaning the database ignores the password check entirely.

Steps taken:

Navigate to the login page at http://10.129.X.X/.

Username: admin'#

Password: (Anything, e.g., password123)

Click Login.

Success! The application identifies us as the admin user and redirects us to the dashboard.

# 4. Flag Retrieval
Once the authentication is bypassed, the dashboard is displayed, containing the flag.

Flag: e3d0b7f87ce2834b4ad57201xxxxxxx

# 5. Summary & Key Takeaways
SQL Injection: Never trust user input. This vulnerability occurred because the input was concatenated directly into the SQL string.

Remediation: Developers should use Prepared Statements or Parameterized Queries to ensure that special characters like ' and # are treated as literal text rather than executable code.

The Power of Comments: In SQLi, comment symbols (# for MySQL, -- for MSSQL/PostgreSQL) are essential tools for terminating a query early to skip security checks.
