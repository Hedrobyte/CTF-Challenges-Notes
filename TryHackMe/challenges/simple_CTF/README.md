## General Information
- Room: Simple CTF
- Difficulty: Easy
- Date: 16/03/2023
- Description: Beginner level ctf.


## Methodology
- Reconnaissance: 
    - Port scanning to identify open ports, services, and versions.
    - Directory enumeration to discover potential entry points.
- Exploration:
    - Exploitation of identified vulnerabilities to gain unauthorized access.
    - Privilege escalation to obtain higher levels of access privileges.
- Results Analysis:
    - Documentation of findings, including vulnerability identification and remediation recommendations.
  

## Results
Three open ports were discovered on the target system that were running FTP, HTTP, and SSH services. GoBuster was utilized to scan the target and revealed the existence of a directory named /simple. Further investigation of this directory exposed crucial information about the target system, including its underlying CMS Made Simple 2.2 version.

An anonymous FTP connection was established to the system, and by exploiting a security flaw, a file containing the message "Damn man... you're the worst dev I've seen. You set the same password for the system user, and the password is so weak... I cracked it in seconds. Gosh... what a mess!" was discovered.

Using the SearchSploit tool, a vulnerability search was initiated for the specific CMS Made Simple 2.2 version. A relevant exploit was located and subsequently downloaded to the local system using the command "searchsploit -m php/webapps/46635.py".

The downloaded 46635.py exploit was then utilized to retrieve the credentials by passing the target site address as an argument. The obtained credentials were then used to establish an SSH connection to the target system. Once access to the system was obtained, a privilege escalation vulnerability was exploited, leading to the discovery of the root.txt flag.

### Reconnaissance
- IP: 10.10.6.153
- Port Scan results:

| Port     | State | Service | Version         |
|----------|-------|---------|-----------------|   
| 22/tcp   | open  | ftp     | vsftpd 3.0.3 |
| 80/tcp   | open  | http    |    Apache httpd 2.4.18 ((Ubuntu)) | 
| 2222/tcp | open  | ssh     | OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0) |

- ftp-anon: Anonymous FTP login allowed (FTP code 230)

### Exploitation

#### nmap scan
~~~nmap
nmap -sVC -T4 --open -oA ./nmap/service_scan 10.10.6.153
~~~

#### gobuster
~~~ gobuster
gobuster dir -u 10.10.6.153 - 40 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt | tee scan_gobuster_dir    
~~~

#### FTP
~~~ftp 
ftp 10.10.6.153   
~~~

#### searchsploit
~~~searchsploit
└─$ searchsploit cms made simple 2.2 
~~~

#### SSH
~~~ssh
ssh mitch@10.10.6.153 -p 2222
~~~


## Exploited Vulnerabilities
- FTP Anonymous Login and Weak Password: The FTP service allowed anonymous login, and it was discovered that the system user had set the same weak password for both the system and FTP accounts. This security flaw allowed unauthorized access to the system.
- CMS Made Simple 2.2 SQL Injection: A vulnerability in the CMS Made Simple version 2.2 was exploited using an SQL injection attack. This vulnerability provided an entry point to access sensitive information and execute unauthorized actions on the target system.
- Privilege Escalation via Vim: A privilege escalation vulnerability was identified, allowing the mitch user to run the vim command with root privileges. By exploiting this vulnerability, the root account was accessed, providing full control over the system.


## Recommendations
- Keep the system and all installed software up to date with the latest security patches to address known vulnerabilities.
- Enforce strong and unique passwords for all user accounts, avoiding the use of weak or default credentials. Implement two-factor authentication (2FA) where possible.
- Restrict or disable anonymous FTP access and enforce secure FTP configurations, such as using SFTP (SSH File Transfer Protocol) instead of traditional FTP.
- Ensure secure coding practices are followed when developing web applications to prevent common vulnerabilities like SQL injection and other code injection attacks.
- Implement the principle of least privilege, granting users only the permissions necessary for their specific roles and responsibilities.