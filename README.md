# From RCE to Database Compromise: Post-Exploitation Pivoting

## Objective

In a previous lab, we confirmed a Remote code Execution (RCE) vulnerability. Now, we will leverage that confirmed (RCE) vulnerability in a web application (DVWA) to harvest backend credentials and pivot to the database system for data exfiltration. Scenario: The attacker has successfully identified a Command Injection flaw (via ls -la) but faces network restrictions blocking interactive reverse shells. The goal is to perform a "Living off the Land" attack using existing system binaries to steal data.


### Skills Learned

- Command Injection
- Living off the Land 
- Data Exfiltration

### Tools Used

Damn Vulnerable Web Application 
Docker 
Windows 10 (Victim Machine hosting DVWA Docker)
Kali Linux 

## Steps

### Configuration Enumeration

Web applications typically hardcode database credentials in configuration files to maintain connectivity. We use the RCE to find these files. Instead of searching the entire drive, we target the web directory and search for the standard configuration file name (config.inc.php).

*The Payload: 127.0.0.1; find /var/www -name "config.inc.php"*

### <img width="650" height="266" alt="image" src="https://github.com/user-attachments/assets/a230d3c8-85bc-44a6-8859-f7f051952a53" />
















