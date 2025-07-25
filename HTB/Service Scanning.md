A service is an application running on a computer that performs useful functions for other users or computers. We call these specialized machines that host services "servers" instead of "workstations," allowing users to interact with and consume these various services. Instead of performing the expected actions as part of the service, we are interested in seeing if we can coerce the service into performing some unintended action that supports our objectives, such as executing a command of our choosing. 

Computers are assigned an IP address, which allows them to be uniquely identified and accessible on a network. The services running on these computers may be assigned a port number to make the service accessible. Port numbers range from 1 to 65535, with well-known ports 1 to 1024 being reserved for privileged services. Port 0 is a reserved port in TCP/IP networking and is not used in TCP or UDP messages. If anything attempts to bind to port 0, it will bind to the next available port above 1024 because 0 is considered a wildcard port.

---

Suppose we want to perform a basic scan against a target residing at 10.129.42.253. To do this, we should type nmap 10.129.42.253 and hit return. \

```jsx
kwc0827@htb[/htb]$ nmap 10.129.42.253

Starting Nmap 7.80 ( https://nmap.org ) at 2021-02-25 16:07 EST
Nmap scan report for 10.129.42.253
Host is up (0.11s latency).
Not shown: 995 closed ports
PORT    STATE SERVICE
21/tcp  open  ftp
22/tcp  open  ssh
80/tcp  open  http
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Nmap done: 1 IP address (1 host up) scanned in 2.19 seconds
```

Under the Port heading, it also tells us that these are TCP ports. By default, Nmap will conduct a TCP scan unless specifically requested to perform a UDP scan. The STATE heading confirms that these prots are open. Sometimes we will see other ports with a different state, such as filterted. This can happen if a firewall is only allowing access to the ports from specific address.

The SERVICE heading tells us the service’s name is typically mapped to the specific port number. However, the default scan will note tell us what is listening on that port. Until we instruct Nmap to interact with the service and attempt to tease out identifying information, it could be another service altogether. 

As we gain familiarity, we will notice the several ports are commonly associated with Windows or Linux. For example, port 3389 is the defualt prot for Remote desktop servioces and is an excellent indication that the target is a Windows machine. In our current scenario, port 22 ssh being available indicates that the target ius running Linux or unix but this service can also be configured on Windows. 

-sC parameter can be used to specify that Nmap scripts should be used to try and obtain more detailed information. The -sV parameter instructs Nmap to perform a version scan. In this scan, Nmap will fingerprint services on the target system and identify the service protocol, application name, and version. The version scan is underpinned by a comprehensive databse over 1000 service signatures. -p- tells Nmap that we want to scan all 65535 TCP ports. 

```jsx
Starting Nmap 7.80 ( https://nmap.org ) at 2021-02-25 16:18 EST
Nmap scan report for 10.129.42.253
Host is up (0.11 s latency).
Not shown: 65530 closed ports
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxr-xr-x    2 ftp      ftp          4096 Feb 25 19:25 pub
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff: 10.10.14.2
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp  open  ssh         OpenSSH 8.2 p 1 Ubuntu 4 ubuntu 0.1 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http        Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: PHP 7.4.3 - phpinfo ()
139/tcp open  netbios-ssn Samba smbd 4.6.2
445/tcp open  netbios-ssn Samba smbd 4.6.2
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: GS-SVCSCAN, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb 2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb 2-time: 
|   date: 2021-02-25 T21:21:51
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 233.68 seconds
```

---

Attacking services

1. Banner Grabbing
Banner grabbin gis a useful technique to fingerprint a service quickly. Often a service will look to identify itself by displaying a banner once a connection is initiated. Nmap will attempt to grab the banners if the syntax nmap -sV —script=banner <target> is specified. We can also attempt this manually using Netcat. 

Nc -nv 10.129.42.253 21
    
    Reveals the version on the server. 
    
    Nmap scan of the defualt port for FTP 21 revelas the vsftpd 3.0.3 installation that we identified. It also reports that anonymous authentication is enabled and that a pub directory is available. 
    

```jsx
kwc0827@htb [/htb]$ ftp -p 10.129.42.253

Connected to 10.129.42.253.
220 (vsFTPd 3.0.3)
Name (10.129.42.253:user): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.

Ftp> ls
227 Entering Passive Mode (10,129,42,253,158,60).
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Feb 25 19:25 pub
226 Directory send OK.

Ftp> cd pub
250 Directory successfully changed.

Ftp> ls
227 Entering Passive Mode (10,129,42,253,182,129).
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp            18 Feb 25 19:25 login. Txt
226 Directory send OK.

Ftp> get login. Txt
Local: login. Txt remote: login. Txt
227 Entering Passive Mode (10,129,42,253,181,53).
150 Opening BINARY mode data connection for login. Txt (18 bytes).
226 Transfer complete.
18 bytes received in 0.00 secs (165.8314 kB/s)

Ftp> exit
221 Goodbye.
```

As shown above, FTP supports common commands such as cs and ls and allow us to download files using the get command. Inspection of the downloaded login. Txt reveals credentials that we could use to further our access to the system. 

1. SMB (Server Message Block)
    
    A prevalent protocol on Windows machines that provides many vectors for vertical and lateral movement. Sensitive data, including credentials can be in network file shares, and some SMB versions may be vulnerable to RCE exploits such as EternalBlue. It is crucial to enumerate this sizeable potential attack surface carefully. Nmap has many scrips for enumerating SMB, such as smb-os-discovery. Nse, which will interact with the SMB service to extract the reported operating system version. 
    

    kwc0827@htb [/htb]$ nmap --script smb-os-discovery. Nse -p 445 10.10.10.40
    
    Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-27 00:59 GMT
    Nmap scan report for doctors. Htb (10.10.10.40)
    Host is up (0.022 s latency).
    
    PORT    STATE SERVICE
    445/tcp open  microsoft-ds
    
    Host script results:
    | smb-os-discovery: 
    |   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
    |   OS CPE: cpe:/o:microsoft:windows_7:: sp 1:professional
    |   Computer name: CEO-PC
    |   NetBIOS computer name: CEO-PC\x 00
    |   Workgroup: WORKGROUP\x 00
    |_  System time: 2020-12-27 T00:59:46+00:00
    
    Nmap done: 1 IP address (1 host up) scanned in 2.71 seconds
    ```
    

We can know the followings: the host runs window 7 os, and we could conduct further enumeration to confirm if it is vulnerable to EternalBlue. The Metasploit framework has several modules for EternalBlue that can be used to validate the vulnerability and exploit it, as we will see in a coming section. We cna run a scan against out target for this module section to gather information form the SMB service. WE can ascertain that the host runs a Linux Kernal, Samba 4.6.2, and the hsot name is GS-SVCSCAN

```jsx
kwc0827@htb [/htb]$ nmap -A -p 445 10.129.42.253

Starting Nmap 7.80 ( https://nmap.org ) at 2021-02-25 16:29 EST
Nmap scan report for 10.129.42.253
Host is up (0.11 s latency).

PORT    STATE SERVICE     VERSION
445/tcp open  netbios-ssn Samba smbd 4.6.2
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210 A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N 56 U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Adtran 424 RG FTTH gateway (92%), Linux 2.6.39 - 3.2 (92%), Linux 3.1 - 3.2 (92%), Linux 3.2 - 4.9 (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

Host script results:
|_nbstat: NetBIOS name: GS-SVCSCAN, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb 2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb 2-time: 
|   date: 2021-02-25 T21:30:06
|_  start_date: N/A

TRACEROUTE (using port 445/tcp)
HOP RTT       ADDRESS
1   111.62 ms 10.10.14.1
2   111.89 ms 10.129.42.253

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.72 seconds
```

Shares
SMB allows users and administrators to share folders and make them accessible remotely by other users. Often these shares have files in them that contain sensitive information such as passwords. A tool that can enumerate and interact with SMB shares is smbclient. The -L flag specifies that we want to retrieve a list of available share on the remote host while -N suppresses the password prompt.

```
kwc0827@htb [/htb]$ smbclient -N -L \\\\10.129.42.253

	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	users           Disk      
	IPC$            IPC       IPC Service (gs-svcscan server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available
```

This reveals the non-default share users. Let us attempt to connect as the guest user. 

```
kwc0827@htb [/htb]$ smbclient \\\\10.129.42.253\\users

Enter WORKGROUP\users's password: 
Try "help" to get a list of possible commands.

smb: \> ls
NT_STATUS_ACCESS_DENIED listing \*

smb: \> exit
```
Ls command resulted in an access denied message, indicating that guest access is not permitted. Trying again with using credentials for the user bob. (bob: welcome 1)

```
kwc0827@htb [/htb]$ smbclient -U bob \\\\10.129.42.253\\users

Enter WORKGROUP\bob's password: 
Try "help" to get a list of possible commands.

smb: \> ls
  .                                   D        0  Thu Feb 25 16:42:23 2021
  ..                                  D        0  Thu Feb 25 15:05:31 2021
  bob                                 D        0  Thu Feb 25 16:42:23 2021

		4062912 blocks of size 1024. 1332480 blocks available
		
smb: \> cd bob

smb: \bob\> ls
  .                                   D        0  Thu Feb 25 16:42:23 2021
  ..                                  D        0  Thu Feb 25 16:42:23 2021
  passwords.txt                       N      156  Thu Feb 25 16:42:23 2021

		4062912 blocks of size 1024. 1332480 blocks available
		
smb: \bob\> get passwords.txt 
getting file \bob\passwords.txt of size 156 as passwords.txt (0.3 KiloBytes/sec) (average 0.3 KiloBytes/sec)
```
Could get access to the users share using credentials and gained access tot the interesting file passwords text file, which can be downloaded with the get command.

## SNMP (simple network management protocol)

SNMP community strings provide information and statistics about a router or device, helping us gain access to it. The manufacturer default community strings of public and private are often unchanged. In snmp version 1 and 2 c, access is controlled using a plaintext community string, and if we know the name, we can gain access to it. Encryption and authentication were only added in snmp version 3. Much information can be gained from snmp. Examination of process parameters might reveal credentials passed on the command line, which might be possible to reuse for other externally accessible services given the prevalance of password reuse in enterprise environments. Routing information, services bound to additional interfaces, and the version of installed software can also be revealed. 

Community string basically has two types, public which only gives read permission, while private gives both read and write permission. 
```
Snmpwalk -v 2 c -c public 10.129.42.253 1.36.1.2.1.1.5.0

Iso. 3.6.1.2.1.1.5.0 = STRING: "gs-svscan"
```
```
Snmpwalk -v 2 c -c private 10.129.42.253
Timeout: no responsef rom 10.129.42.253
```
A tool such as onesixtyone can be used to brute force the community string names using a dictionary file of common community strings such as the dict text file included in the github repo for the tool. 

```
  Service Scanning
kwc0827@htb [/htb]$ onesixtyone -c dict. Txt 10.129.42.254

Scanning 1 hosts, 51 communities
10.129.42.254 [public] Linux gs-svcscan 5.4.0-66-generic #74-Ubuntu SMP Wed Jan 27 22:54:38 UTC 2021 x 86_64
```

Ran 51 community string target towards host. 51 communities implies the number of strings in dictionary. Txt file. 