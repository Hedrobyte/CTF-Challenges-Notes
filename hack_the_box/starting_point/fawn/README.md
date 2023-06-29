## General Information
- Machine: Fawn 
- Difficulty: Very easy
- OS: Linux
- Date: 01/02/2023


## Methodology
- Reconnaissance: Identification of the target IP and performing a port scan to discover running services.
- Exploitation: Utilization of anonymous FTP access to retrieve the flag.txt file from the FTP server.
- Results Analysis: Documentation of findings, identification of vulnerabilities, and recommendations for remediation.

## Results
During the penetration test, a significant vulnerability was identified in the FTP service of the target machine, allowing anonymous access. The vsftpd 3.0.3 service was running on port 23/tcp. This vulnerability enabled unauthorized access to the target machine, leading to the discovery of the flag.


### Reconnaissance
- IP: 10.129.171.212
- ftp-anon: Anonymous FTP login allowed
- Port Scan results:

| Port   | State | Service | Version         |
|--------|-------|---------|-----------------| 
| 23/tcp | open  | ftp     | vsftpd 3.0.3    |


### Commands Used

#### Nmap
~~~nmap
nmap -sVC -T4 -oN ./service_scan_port 10.129.171.212
~~~

#### FTP
~~~FTP
 ftp 10.129.171.212
~~~


## Recomendações
- Disable anonymous access to the FTP server to prevent unauthorized access.
- Regularly update the FTP server software to the latest version to address any known security vulnerabilities.