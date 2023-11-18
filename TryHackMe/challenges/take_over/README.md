## General Information
- Room: TakeOver
- Difficulty: Easy
- Date: 18/11/2023
- Description: "This challenge revolves around subdomain enumeration." 


## Methodology
- gobuster vhost Scan: Used gobuster with the vhost mode to enumerate subdomains.
- Hosts File Configuration: Added discovered subdomains to the /etc/hosts file for local resolution.
- certificate ssl Scan: Utilized OpenSSL to examine SSL certificates for subdomains.


## Results

### Reconnaissance
- IP: 10.10.205.51 
- Port Scan results:

| PORT    | STATE | SERVICE  | VERSION                         |
|---------|-------|----------|---------------------------------|
| 22/tcp  | open  | ssh      | OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0) |
| 80/tcp  | open  | http     | Apache httpd 2.4.41 ((Ubuntu))  |
| 443/tcp | open  | ssl/http |  Apache httpd 2.4.41 ((Ubuntu)) |

Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

### Tools and Exploitation

#### Nmap Scan
~~~nmap
nmap -sV 10.10.205.51 -oN nmap_scan.txt 
~~~

#### Gobuster
~~~curl
gobuster vhost -u https://futurevera.thm/ -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain false -k -t 64
~~~

#### SSL certificate
~~~openssl
echo -n | openssl s_client -connect support.futurevera.thm:443 2>/dev/null | openssl x509 -text > certificado_support.futureva.thm.txt
~~~