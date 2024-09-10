# [Recon]
```
=========================================================================
[OS]: Debian
[Web-Technology]: Apache httpd 2.4.38
[Hostname]: dauwn
[IP]: 192.168.181.11
[USERS]:
ganimedes, dawn
[CREDENTIALS]:

=========================================================================

```

# [Scanning and Enumeration]
```
[NMAP RESULTS]:
=========================================================================
PORT     STATE SERVICE     REASON  VERSION
80/tcp   open  http        syn-ack Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Site doesn't have a title (text/html).
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
139/tcp  open  netbios-ssn syn-ack Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn syn-ack Samba smbd 4.9.5-Debian (workgroup: WORKGROUP)
3306/tcp open  mysql       syn-ack MySQL 5.5.5-10.3.15-MariaDB-1
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.15-MariaDB-1
|   Thread ID: 16
|   Capabilities flags: 63486
|   Some Capabilities: IgnoreSpaceBeforeParenthesis, Support41Auth, DontAllowDatabaseTableColumn, SupportsTransactions, IgnoreSigpipes, InteractiveClient, ODBCClient, Speaks41ProtocolNew, ConnectWithDatabase, SupportsLoadDataLocal, LongColumnFlag, FoundRows, Speaks41ProtocolOld, SupportsCompression, SupportsAuthPlugins, SupportsMultipleResults, SupportsMultipleStatments
|   Status: Autocommit
|   Salt: (]zrK`!#*Hle:.v*lb)B
|_  Auth Plugin Name: mysql_native_password
Service Info: Host: DAWN

Host script results:
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.9.5-Debian)
|   Computer name: dawn
|   NetBIOS computer name: DAWN\x00
|   Domain name: dawn
|   FQDN: dawn.dawn
|_  System time: 2024-08-12T09:28:38-04:00
|_clock-skew: mean: 1h21m40s, deviation: 2h18m34s, median: 1m39s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 21385/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 30916/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 63405/udp): CLEAN (Timeout)
|   Check 4 (port 53994/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2024-08-12T13:28:38
|_  start_date: N/A

=========================================================================
[Web Services Enumeration]:
> Files
=====================================================================
ID           Response   Lines    Word       Chars       Payload                       
=====================================================================

000000061:   200        22 L     84 W       791 Ch      "index.html"                  
000000149:   403        11 L     32 W       298 Ch      ".htaccess"                   
000000371:   200        22 L     84 W       791 Ch      "."                          


> Directories
=====================================================================
ID           Response   Lines    Word       Chars       Payload                       
=====================================================================

000000054:   301        9 L      28 W       315 Ch      "logs"                        
000004227:   403        11 L     32 W       302 Ch      "server-status"               
000004255:   200        22 L     84 W       791 Ch      "http://192.168.181.11/"      
000013767:   301        9 L      28 W       315 Ch      "cctv"


=========================================================================

```

Upon visiting the `/logs` directory on the website, I notice some file which seems very sensitive.
![[Pasted image 20240812174152.png]]

All of the above files where not accessible because i don't have permission excluding the `management.log` file.  So i take my shot in viewing the content of the `management.log` file.

![[Pasted image 20240812180029.png]]


Scrolling down the file a little bit, I started to observe a command name `pspy64` is been executed which is redirecting the output to `var/www/html/logs/management.log` which the current file am seeing this information. Pspy64 is command line utility used to monitor processes such `cronjobs` and other process. 

![[Pasted image 20240812175602.png]]

Scrolling down a bit i notice a `cronjob` which is executing a bash script with the following syntax `/bin/sh -c /home/dawn/ITDEPT/product-control` and another different file in the same directory `/bin/sh -c /home/dawn/ITDEPT/web-control`. This make me believe that we have a user called `dawn` and with all sense of humility a permission is give to both file `[chmod 777]`.

![[Pasted image 20240812174052.png]]

Also, another script was executed as `cronjob` and there we have another user called `ganimedes`

![[Pasted image 20240812175845.png]]

```
[MYSQL Enumeration]: 
Nothing is usefull. Tried bruteforcing `root` user but nothing.

```

```
[SMB Enumeration] : smbclient, enum4linux, smbmap
Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	ITDEPT          Disk      PLEASE DO NOT REMOVE THIS SHARE. IN CASE YOU ARE NOT AUTHORIZED TO USE THIS SYSTEM LEAVE IMMEADIATELY.
	IPC$            IPC       IPC Service (Samba 4.9.5-Debian)


S-1-22-1-1000 Unix User\dawn (Local User)
S-1-22-1-1001 Unix User\ganimedes (Local User)

```

Looking at the above result, I know that i can access the `ITDEPT` share on the box. So i tried login in to the share using `smbclient //192.168.181.11/ITDEPT -N` which log me in to the share.

![[Pasted image 20240812181348.png]]
But unluckily there is nothing on the share. So I then tried to `put file.txt` to see if i can upload something in the share which went successful.  I need to know where i am on the box so run `pwd` command which tells me the part am in `\\192.168.181.11\ITDEPT\` Then i remember that `cronjob` that is running `product-control` and `web-control` scripts.

> From Here i create the two files locally and add a bash and a netcat reverse connection.

![[Pasted image 20240812182035.png]]

I put those file in the smb share and also create a listener locally `nc -lnvp 9002`  and there we go. We have a shell.

![[Pasted image 20240812182210.png]]

# [Privilege - Escalation]
```
=========================================================================
[OTHER / PRIVILEGE-ESCALATION]:
sudo -l
find / -perm -u=s -type f 2>/dev/null

```
![[Pasted image 20240812182722.png]]

Here `zsh` stand out clear to be `SUID`
![[Pasted image 20240812183014.png]]

# [Flags]
`local: 51a6ab6a7a2a3439687958c58520c319`
`proof: c91f8e5f4ea5ac046242450b5955f9c6`

# [Key note]
```
=========================================================================
Take Away Concepts (for the flat-file reference system):
* Always put an eye on every single detail. especially `processes`
* Try every single lateral movement you might think of.
* Use multiple tools for good result to a single problem.

```