First what we need to do, is small recon.
```
Starting Nmap 7.93 ( https://nmap.org ) at 2023-10-17 19:42 CEST
Nmap scan report for 10.10.10.6
Host is up (0.11s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 5.1p1 Debian 6ubuntu2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 3ec81b15211550ec6e63bcc56b807b38 (DSA)
|_  2048 aa1f7921b842f48a38bdb805ef1a074d (RSA)
80/tcp   open  http        Apache httpd 2.2.12 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.2.12 (Ubuntu)
9000/tcp open  cslistener?
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 45.43 seconds
```
I don't find anything intresting port. Now we need to check directorys by gobuster. First what i recommend to do is check the website and try this ```index.html``` or ```index.php``` by this we can know, what kind of leangue running on the server. In this example work index, but when you get /index.php i recommend to add -x php in ur gobuster.

![image](https://github.com/Anogota/Popcorn/assets/143951834/2e406987-3998-45a8-9d50-11a33ebbaa47)

We can find some intresting by only one look pretty nice /torrent. Let's go into this directory. This website look normal, like everyone. I try find something in source code, but there nothing what might be useful to us. next my step is create a account.

![image](https://github.com/Anogota/Popcorn/assets/143951834/ac75211d-5ec4-4171-9453-ca13454af234)

We can upload a image, maybe by this we can do RCE. First step is ```cp /usr/share/webshell/php/php-reverse-shell.php ~/HTB``` and modifay there IP. I tryed, everything there, but can't upload jpg.php also chaning in burp too image/jpg, this don't work. But i upload a regular image on the website and deeper we can also see something intresting.
