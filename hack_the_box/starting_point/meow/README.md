## General Information
- Machine: Meow 
- Difficulty: Very easy
- OS: Linux
- Date: 01/02/2023


## Methodology
- Reconnaissance: Identification of the target IP and performing a port scan to discover running services.
- Exploitation: Utilization the Telnet protocol to gain access to the target machine and exploit possible security vulnerabilities.
- Results Analysis: Documentation of findings, identification of vulnerabilities, and recommendations for remediation.


## Results
During the penetration test, a significant vulnerability was identified in the Telnet service running on port 23/tcp of the IP 10.129.185.14. This vulnerability allowed unauthorized access to the target machine.
After gaining access to the machine, it was possible to navigate to the root directory and obtain the flag.

### Reconnaissance
- IP: 10.129.185.14
- Port Scan results:

| Port   | State | Service | Version         |
|--------|-------|---------|-----------------| 
| 23/tcp | open  | telnet  | Linux telnetd  store 5.0.7 |

### Commands Used

#### Nmap
~~~nmap
nmap -sVC -T4 -oN ./service_scan_port 10.129.185.14
~~~

#### Telnet
~~~Telnet
telnet 10.129.185.14 
~~~


## Recomendações
- Disable the Telnet service or replace it with a more secure protocol, such as SSH.
- Implement proper access controls to ensure that only authorized users can authenticate to the system.

