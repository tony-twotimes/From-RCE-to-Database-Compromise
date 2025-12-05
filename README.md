# From RCE to Database Compromise: Post-Exploitation Pivoting

## Objective

In a previous lab, we confirmed a Remote code Execution (RCE) vulnerability. Now, we will leverage that confirmed (RCE) vulnerability in a web application (DVWA) to harvest backend credentials and pivot to the database system for data exfiltration. 


### Skills Learned

- Command Injection
- Living off the Land 
- Data Exfiltration

### Tools Used

Damn Vulnerable Web Application 
Docker 
Windows 10 (Victim Machine hosting DVWA and MySQL5.7 Docker)
Kali Linux 

## Steps


### Phase 1: Configuration Enumeration

Web applications typically hardcode database credentials in configuration files to maintain connectivity. We use the RCE to find these files. Instead of searching the entire drive, we target the web directory and search for the standard configuration file name (config.inc.php).

*The Payload: 127.0.0.1; find /var/www -name "config.inc.php"*

### <img width="650" height="233" alt="image" src="https://github.com/user-attachments/assets/df97b522-7c7c-4926-ad9a-4bb64c024671" />


Result is that we find the file here: */var/www/html/config/config.inc.php*

### Phase 2: Credential Exfiltration 

We need to read the file. However, simply using cat on a PHP file might cause the browser to interpret the tags and hide the sensitive data. We use grep to extract only the 
specific credential lines.

*The Payload: 127.0.0.1; grep -r "db_" /var/www/html/config/*

### <img width="1187" height="539" alt="image" src="https://github.com/user-attachments/assets/63a9ec3b-bd4c-4f87-bdf5-ced76816345c" />

The result is that the server returns the plaintext credentials. 

### Phase 3: The Heist 

Weneed to run a command that logs into the database locally (where it trusts the connection) and dumps the password hashes to the screen.

*The Payload: 127.0.0.1; mysql -h 127.0.0.1 -u app -pvulnerables -D dvwa -e "SELECT user, password FROM users;"*

### <img width="654" height="312" alt="image" src="https://github.com/user-attachments/assets/0d131a01-10b0-4ff5-9164-2cd7e5c44354" />

### Optional: Use cURL to Automate Extraction 

- In your browser (where you see the hashes), press F12 to open Developer Tools.
- Go to the Storage tab (Firefox) or Application tab (Chrome).
- Click Cookies on the left.
- Copy the value for PHPSESSID.

### <img width="2302" height="1263" alt="image" src="https://github.com/user-attachments/assets/7ffacc6b-3114-477c-9b41-9be8019c4185" />
  

























