# Network Forensic Analysis Report

## Time Thieves
You must inspect your traffic capture to answer the following questions:

1. What is the domain name of the users' custom site?
    - FRANK-N-TED.COM

![](https://github.com/smalani06/cs-final-project/blob/main/images/network/tt1.png)

2. What is the IP address of the Domain Controller (DC) of the AD network?
    - 10.6.12.12

![](https://github.com/smalani06/cs-final-project/blob/main/images/network/tt2.png)

3. What is the name of the malware downloaded to the 10.6.12.203 machine?
    - june11.dll
    - Export the file by going to File > Export Objects > HTTP.

![](https://github.com/smalani06/cs-final-project/blob/main/images/network/tt3.png)

4. Upload the file to [VirusTotal.com](https://www.virustotal.com/gui/).

![](https://github.com/smalani06/cs-final-project/blob/main/images/network/tt4.png)

5. What kind of malware is this classified as?
    - Trojan

---

## Vulnerable Windows Machine

1. Find the following information about the infected Windows machine:
    - Host name: ROTTERDAM-PC
    - IP Address: 172.16.4.205
    - MAC Address: 00:59:07:b0:63:a4

![](https://github.com/smalani06/cs-final-project/blob/main/images/network/w1.png)

2. What is the username of the Windows user whose computer is infected?
    - matthijs.devries

![](https://github.com/smalani06/cs-final-project/blob/main/images/network/w2.png)

3. What are the IP addresses used in the actual infection traffic?
    - 185.243.115.84

![](https://github.com/smalani06/cs-final-project/blob/main/images/network/w3.png)

4. As a bonus, retrieve the desktop background of the Windows host.

---

## Illegal Downloads

1. Find the following information about the machine with IP address `10.0.0.201`:
    - MAC address: 00:16:17:18:66:c8
    - Windows username: elmer.blanco
    - OS version: Windows 10

![](https://github.com/smalani06/cs-final-project/blob/main/images/network/i1.png)

![](https://github.com/smalani06/cs-final-project/blob/main/images/network/i1_2.png)

2. Which torrent file did the user download?
    - Betty_Boop_Rhythm_on_the_Reservation.avi.torrent

![](https://github.com/smalani06/cs-final-project/blob/main/images/network/i2.png)

References:
  - https://unit42.paloaltonetworks.com/using-wireshark-identifying-hosts-and-users/
