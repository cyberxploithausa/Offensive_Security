# [Recon]

```
=========================================================================
[OS]: 
[Web-Technology]: 
[Hostname]: 
[IP]:  192.168.173.83
[USERS]: 
[CREDENTIALS]: 
=========================================================================
```
-----------------------------------------------------------------------------

# [Scanning and Enumeration]

--	[Network Enumeration]

```
PORT     STATE SERVICE REASON  VERSION
21/tcp   open  ftp     syn-ack vsftpd 3.0.3
22/tcp   open  ssh     syn-ack OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 89:4f:3a:54:01:f8:dc:b6:6e:e0:78:fc:60:a6:de:35 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDp0J8d7K55SuQO/Uuh8GyKm2xlwCUG3/Jb6+7RlfgbwrCIOzuKXICcMHq4i8z52l/0x0JnN0GUIeNu6Ek/ZGEMK4y+zvAs0R6oPNlScpx0IaLDXTGrjPOcutmx+fy6WDW3/jJGLxwu+55d6pAjzzQR37P1eqH8k9F6fbv6YUFbU+i68x9p5bXCC1m17PDO98Che+q32N6yM26CrQMOl5t1OzO3t1pbvMd3VOQA8Qd+fhz5tpxtRBTSM9ylQj2B+z6XjJnbMPhnO3C1oaYHjjL6KiTfD5YabDqsBf+ZHIdZpM+7fOqKkgHa4bbIWPUXB/OuOJnORvEeRCALOzjcSrxr
|   256 dd:ac:cc:4e:43:81:6b:e3:2d:f3:12:a1:3e:4b:a3:22 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDBsZi0z31ChZ3SWO/gDe+8WyFVPrFX7KgZNp8u/1vlhOSrmdZ32WAZZhTT8bblwgv83FeXPvH7btjDMzTuoYA8=
|   256 cc:e6:25:c0:c6:11:9f:88:f6:c4:26:1e:de:fa:e9:8b (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICo+dAzFw2csa366udGUkSre2W0qWWGoyWXwKiHk3YQc
80/tcp   open  http    syn-ack Apache httpd 2.4.38 ((Debian))
|_http-title: Katana X
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.38 (Debian)
8088/tcp open  http    syn-ack LiteSpeed httpd
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: LiteSpeed
|_http-title: Katana X
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

```
-----------------------------------------------------------------------------

--	[Web Enumeration]

```
--> Files
=====================================================================
ID           Response   Lines    Word       Chars       Payload                       
=====================================================================

000000061:   200        23 L     73 W       655 Ch      "index.html"                  
000000149:   403        9 L      28 W       279 Ch      ".htaccess"                   
000000371:   200        23 L     73 W       655 Ch      "."                           
000000529:   403        9 L      28 W       279 Ch      ".html"                       
000000798:   403        9 L      28 W       279 Ch      ".php"                        
000001556:   403        9 L      28 W       279 Ch      ".htpasswd" 


--> Directories
=====================================================================
ID           Response   Lines    Word       Chars       Payload                       
=====================================================================

000000386:   403        9 L      28 W       279 Ch      "icons"                       
000001525:   200        95 L     272 W      3998 Ch     "ebook"                       
000004227:   403        9 L      28 W       279 Ch      "server-status"
```
![[Pasted image 20240906230421.png]]


-----------------------------------------------------------------------------
# [Lateral Movement]


```bash

```

-----------------------------------------------------------------------------

# [Privilege-Escalation]

--> 

```bash

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

`local.txt : `
`proof.txt : `
