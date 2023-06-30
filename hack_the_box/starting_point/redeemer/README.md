## General Information
- Machine: Redeemer 
- Difficulty: Very easy
- OS: Linux
- Date: 26/02/2023


## Methodology
- Reconnaissance: Execution of a port scan using Nmap to identify running services on the target machine.
- Exploitation: Utilization of the Redis command-line utility to access the Redis server and retrieve sensitive data, including the flag.
- Results Analysis: Documentation of findings, identification of vulnerabilities, and recommendations for remediation.


## Results
During the exploitation phase, a vulnerability in the Redis service was identified, which allowed unauthorized access and manipulation of sensitive data in the database. By using the redis-cli tool, it was possible to select the desired database and obtain information about the present keys.

### Reconnaissance
- IP: 10.129.5.121 
- Anonymous/Guest Access: Yes
- Port Scan results:

| Port     | State | Service | Version         |
|----------|-------|---------|-----------------| 
| 6379/tcp | open  | redis   | Redis key-value  store 5.0.7|

### Commands Used

#### Nmap
~~~nmap
nmap -sVC -T4 --open -p- -oN ./fullscan 10.129.5.121
~~~

#### Redis
~~~redis-cli 
redis-cli -h 10.129.5.121
~~~


## Recommendations
- Apply authentication mechanisms to prevent unauthorized access.