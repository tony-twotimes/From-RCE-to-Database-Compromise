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

### <img width="650" height="266" alt="image" src="https://github.com/user-attachments/assets/a230d3c8-85bc-44a6-8859-f7f051952a53" />

Result is that we find the file here: */var/www/html/config/config.inc.php*

### Phase 2: Credential Exfiltration 

We need to read the file. However, simply using cat on a PHP file might cause the browser to interpret the tags and hide the sensitive data. We use grep to extract only the 
specific credential lines.

*The Payload: 127.0.0.1; grep -r "db_" /var/www/html/config/*

### <img width="1187" height="539" alt="image" src="https://github.com/user-attachments/assets/63a9ec3b-bd4c-4f87-bdf5-ced76816345c" />





















