## General Information 
- Machine: Dancing
- Difficulty: Very easy
- OS: Windows
- Date: 01/02/2023


## Methodology
- Reconnaissance: Execution of a port scan using Nmap to identify running services on the target machine.
- Exploitation: Execution of the smb service, identified on port 445/tcp, to exploit the target machine.
- Results Analysis: Documentation of findings, identification of vulnerabilities, and recommendations for remediation.


## Results
During the reconnaissance phase, a port scan was performed using Nmap, revealing the running services on the target machine. By exploiting the smb service on port 445/tcp, access to the WorkShares share was obtained. These findings highlight the presence of vulnerabilities in the target machine, enabling unauthorized access and the discovery of the flag.


## Reconnaissance
- IP: 10.129.198.14
- Port Scan results:

| Port    | State | Service | Version         |
|---------|-------|---------|-----------------| 
| 135/tcp | open  | msrpc         | Microsoft Windows RPC |
| 139/tcp | open  | netbios-ssn   | Microsoft Windows netbios-ssn |
| 445/tcp | open  | microsoft-ds  |  |


### Commands Used

#### Nmap
~~~nmap
nmap -sVC -T4 -oN ./service_scan_port 10.129.198.14
~~~

#### SMB
~~~SMB
smbclient -L 10.129.198.14           
~~~


## Recommendations
- Review and update the access permissions for the WorkShares share.
- Strengthen the authentication for the SMB service by implementing stronger authentication methods.