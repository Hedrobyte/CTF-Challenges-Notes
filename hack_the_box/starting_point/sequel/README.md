## General Information
- Machine: Sequel
- Difficulty: Very easy
- OS: Linux
- Date: 01/03/2023


## Methodology
- Reconnaissance: Execution of a port scan using Nmap to identify running services on the target machine.
- Exploitation: Establishment of a connection with the MariaDB database using the root user without a password to exploit the data contained within.
- Results Analysis: Documentation of findings, identification of vulnerabilities, and recommendations for remediation.


## Results
During the reconnaissance phase, it was discovered that the machine has only one open port: 3306/tcp running MariaDB. In the exploitation phase, it was possible to connect to the database using the root user without a password. Two tables were identified: users and config, which contained relevant information about machine configuration and user accounts. In the config table, the flag was found.

### Reconnaissance
- IP: 10.129.233.223
- Port Scan results:

| Port     | State | Service | Version         |
|----------|-------|---------|-----------------|   
| 3306/tcp |  open | mysql   | 5.5.5-10.3.27-MariaDB |

### Commands Used

#### nmap 
~~~nmap
nmap -sVC -v -oN ./service_scan_port 10.129.86.37
~~~

#### MySQL
~~~SQL_injection 
sudo mysql -h 10.129.86.37 -u root
~~~


## Recommendations
- Set a strong password for the root user account in the MariaDB database.