## General Information
- Room: RootMe
- Difficulty: Easy
- Date: 14/03/2023
- Description: A ctf for beginners, can you root me?


## Methodology
- Reconnaissance: 
    - Realization of a port scanning using "nmap" to identify the running services and their versions.
    - Utilizaiton of Gobuster to enumerate the directories of the target URL and identify potential entry points.
- Exploration:
    - Utilization of known exploits to explore the identified vulnerabilities. 
    - Realization of privilege escalation to obtain privileged access. 
- Results Analysis:
    - Documentation of findings, identification of vulnerabilities, and recommendations for remediation.


## Results
Several reconnaissance activities were carried out, including an nmap scan, which identified open ports and their respective services and versions. Gobuster was used to enumerate the directories of the target URL and a directory called /panel was discovered, which redirected to the root URL. Additionally, a file called user.txt was found in the directory /var/www/html, which contained the flag "THM{************}."

To escalate privileges, a netcat listener was started and a command was executed that allowed access to the root account. The root flag "THM{*********}" was found in the /root directory.

### Reconnaissance
- IP: 10.10.215.13
- Port Scan results:

| Port     | State | Service | Version         |
|----------|-------|---------|-----------------|   
| 22/tcp   | open  | ssh     | OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0) |
| 80/tcp   | open  | http    |    Apache httpd 2.4.29 ((Ubuntu)) | 

### Exploitation

#### Nmap
~~~nmap
nmap -sVC -v -T4 -oA ./service_scan 10.10.215.13
~~~

#### gobuster
~~~ gobuster
gobuster dir -u http://10.10.215.13/ -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt -t 50 -x php,html,txt 2>/dev/null | tee scan_gobuster_dir
~~~

#### netcat 
~~~netcat 
nc -lvnp 3333              
~~~


## Exploited Vulnerabilities
- OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
    - The OpenSSH version running on port 22 was susceptible to a known 
    - This vulnerability provided an initial foothold into the system.
- Apache httpd 2.4.29 ((Ubuntu))
    - The Apache web server version running on port 80 had a known vulnerability that allowed for directory traversal and information disclosure.
    - This vulnerability facilitated the discovery of a directory called "/panel" that redirected to the root URL, revealing potential entry points.
- Insufficient privilege separation
    - After gaining access as the "www-data" user, privilege escalation techniques were employed to escalate privileges to the root user.
    - The exploitation of this vulnerability allowed for elevated privileges and complete control over the system.


## Recommendations
- Regularly update and apply security patches to the OpenSSH and Apache web server software to address known vulnerabilities.
- Implement strict access controls to limit access to critical system components and directories.
- Ensure proper privilege separation between user accounts to prevent unauthorized escalation of privileges.