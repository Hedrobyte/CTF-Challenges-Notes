## General Information
- Machine: Crocodile
- Difficulty: Very easy
- OS: Linux
- Date: 02/03/2023


## Methodology
- Reconnaissance: The execution of network scanning using nmap allowed the identification of services running on open ports. Additionally, the utilization of gobuster was employed to perform a search for directories and pages on the target website, using a common keyword list.
- Exploitation: Anonymous connection and exploitation of the FTP service running on port 21.
- Results Analysis: Documentation of findings, identification of vulnerabilities, and recommendations for remediation.


## Results
During the penetration test, a FTP service was discovered, which, when exploited, contained files with login information. Using Gobuster, a search was performed and a login page was found at '10.129.242.36/login.php'. By utilizing the login information obtained from the FTP, it was possible to exploit the page and gather information, including the flag.

### Reconnaissance
- IP: 10.129.242.36
- Server: Apache/2.4.41 (Ubuntu)
- ftp-anon: Anonymous FTP login allowed (FTP code 230)
- OS: unix
- userlist
- uselist.passwd
- Port Scan results:

| port   | State | Service | Version         |
|--------|-------|---------|-----------------|   
| 21/tcp | open  | ftp     | vsftpd 3.0.3    |


### Commands Used

#### Nmap 
~~~nmap
nmap -sVC -p 21 -oN ./version_service_scan_port21 10.129.242.36
~~~

#### FTP
~~~FTP 
ftp 10.129.242.36
~~~

### Gobuster
~~~
sudo gobuster dir -u 10.129.242.36 -w /usr/share/dirb/wordlists/common.txt -x php, js | tee gobuster_output
~~~


## Recommendations 
- Deactivate anonymous login and implement stronger authentication mechanisms to prevent unauthorized access.