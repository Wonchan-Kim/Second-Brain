The field has grown and evolved greatly in the last few years.

## Red team vs Blue team
Attackers vs defenders
Adversary role in breaking into the organizations to identify any potential weaknesses real attackers while the blue team plays the defenders part.
On the other hand, the blue team makes up the majority of infosec jobs. It is responsible for strengthening the organizations defenses by analyzing the risks, coming up with the policies, responding to threats and incidents and effectively using security tools and other similar tasks. 

## Choosing a Distro
There are many linux distributions for penetration testing. There are quite a few debian-based pre existing distros preload with many tools that we need to perform our assessments. Many of these tools are rarely required, and no distro contains every tool that we need to perform our assessments. Many of these tools are rarely required and no distro contains every tool that we need to perform our assessments. 

## Setting up the pentest distro
Hypervisor is software that allows us to create and run virtual machines. It will enable us to use our host computer to run multiple VMs by virtually sharing memory and processing resources. Vms on a hypervisor run isolated from the primary operating system, which offers a layer of isolation and protection between our production network and vulnerable networks. 

> [!note] ISO vs OVA
> ISO vs OVA 
> ISO file is essentially just a CD rom that can be mounted within out hypervisor of choice to build the VM by installing the OS system ourselves. An ISO gives us more room for customization and therefore a more granular approach when setting up our attack VM. 
> 
> OVA file is a pre built virtual appliance that contains an OVF XML file that specifies the VM hardware settings and a VMDK, which is the virtual disk that theoperating system is intalled on. An ova is prevuilt and therefore can be rapidly deployed to get up and running quicker. 

## VPN 
Virtual private network allows us to connect to a private network and access hosts and resources as if we were directly connected to the target private network. It is a secured communications channel over shared public networks to connect to a private network. VPNs provide a degree of privacy and security by encrypting communications over the channel to prevent eavesdropping and access to data traversing the channel. 
![image](https://academy.hackthebox.com/storage/modules/77/GettingStarted.png)
At a high level, VPN works by routing out connection device’s internet connection through the target VPN’s private server instead of our service provider (ISP). When connected to a VPN, data orignates from the VPN server rather than our computer and will appear to originate form a public IP address other than our own. 

There are two main types of remote access VPNs: client based VPN and SSL VPN. SSL VPN uses the web browser as the VPN client. The connection is established between the browser and an SSL VPN gatewat can be configured to onlu access to web based appliations such as email and intranet sites, or even the internal network but without the need for the end user to install or use any specialized software. Client based vpn requires the use of client software to establish the VPN connection . Once connected the user’s host will work mostly as if it were connected directly to the company network and will be able to access any resources allowed by the server configuration. Some corporate VPNs will provide employees with full access to the internal corporate network, while others will palce users on a specific segment reserved for remote workers. 

## Why use a VPN
We can use VPN service such as Private Internet Access and connect to a VPN server in another part of our country or another region of the world to obscure our browsing traffic or disguise our public IP address. This can provide us with some level of security and privacy. Still, since we are connecting to a company’s server, there is always the chance that data is being logged or the VPN service is not following security best pracitces or the security features that they advertise. Using a VPN service comes with the risk that the provider is not doing what they are saying and are logging all data. Usage of a VPN service does not guarantee anonymity or privacy but is useful for bypassing certain network/firewall restirctions or when connected to a possible hostile netowrk. A VPN service should never be used with the thought that it will protect us from the consqeuences of performing nefarious activities. 


## Connecting to HTB VPN
HTB and other services offering purposefully vulnerable VMs/netowrks require players to connect to the target network via a VPN to access the private lab network. Hosts within HTB networks cannot connect directly out to the internet. When connected to HTB VPN, we hsould always consider the network to be hostile. We should only connect from a virtual machine, disallow password authentication if SSH is enabled on our attacking VMs, lockdown any web servers, and not leave sensitive information our attack (VM). When connecting to a VPN, we connect using the following command. 

```
sudo openvpn user.ovpn
```
The last line if executing this command is Initialization sequence completed, telling us that we successfully connect to the VPN. Where sudo tells our host to run the command as the elevated root user, openvpn is the VPN client, and the user. Ovpn file is the vpn key that we dwonload from this platform. If we type ifconfig in another terminal window, we will see a tun adapter if we successfully connected to the VPN. 
```
kwc0827@htb[/htb]$ ifconfig

<SNIP>

tun0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1500
        inet 10.10.x.2  netmask 255.255.254.0  destination 10.10.x.2
        inet6 dead:beef:1::2000  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::d82f:301a:a94a:8723  prefixlen 64  scopeid 0x20<link>
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen
```
Netstat -rn will show us the networks accessible via the VPN. 
```
kwc0827@htb[/htb]$ netstat -rn

Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         192.168.1.2     0.0.0.0         UG        0 0          0 eth0
10.10.14.0      0.0.0.0         255.255.254.0   U         0 0          0 tun0
10.129.0.0      10.10.14.1      255.255.0.0     UG        0 0          0 tun0
192.168.1.0     0.0.0.0         255.255.255.0   U         0 0          0 eth0
```
Here we see that the 10.129.0.0/16 network used for HTB academy machines is accessible via the tun0 adapter via the 10.10.14.0/23 network. 

> [!tip] Calculating subnet!
> Subnet mask (GenMask) is network and hosts. 
> In CIRD notation, it is represented with /N, N is the bit identifying the bits clarifying the network. 32-N bits are for hosts.
> For example, /16 is using 16 bits for the network. 
> 1111 1111 1111 1111
> 255         255         0 0 
> /23 is using 23 bits for network
> 1111 1111 1111 1111 1111 1110 
> 255          255         254
> 
> Network address is derived by IP address and operation with subnet mask. 
> First ip is the network address itself, last ip is network address + host bits converted to 1. 
> 
> Ex) 10.129.0.0/16
> Subnet mask is 255.255.0.0
> Network address is 10.129.0.0
> Broadcast address is 10.129.255.255

---
## Shell
Shell is a program that takes input from the user via the keyboard and passes these commands to the operating system to perform a specific function. In the early days of computing, the shell was the only interface available for interacting with systems. Since then, many more operating system types and versions have emerged along with the GUI to complement command line interfaces such as the linux terminal, windows command line and windows power shell.

Most linux system use a program called bash, to interact with the OS, which is enhanced version of sh, the unix system’s original shell program. Aside from bash, there are also other shells.

Once the target host has been exploited, and we have obtained shell level access and can run commands interactively as if we are sitiing logged in to the host. Shell may be obtained by exploiting a web app or network service vulnerability or obtaining credentials and logging into the target host remotely. 

---
## Port
Virtual points where network connections begin and end. 
TCP is connection oriented,  meaning that a connection between a client and a server must be established before data can be sent. The server must be in a listening state awaiting connection requests from clients.
UDP utilizes a connectionless communication model. There is no handshake and therefore introduces a certain amount of unreliability since there is no guarantee of data delivery. UDP is useful when error correction is either not needed or is handled by the application itself. Suitable for applications that run time sensitive tasks since dropping packets is faster than waiting for delayed packet due to retansmission, as is the case with TCP and can significantly affect a real time system. 

---
## SSH
Secure shell is a network protocol that runs on port 22 by default and provides users such as system administrators a secure way to access a computer remotely. Ssh can be configured with password authentication or passwordless using pyblic key authentication using an SSH public key pair. SSH can be used to remotely access systems on the same network, over the internet, facilitate connections to reousrces in other network using port forwarding/proxying and upload/download files to and from remote systems.

```
ssh BOB@10.10.10.10
password:
```
---
## Netcat
Netcat, ncat or nc is an excellent network utility for interacting with TCP/UDP ports. It can be used for many things during pentest. Its primary usage is for connecting to shell. It also can be used to connect to any listening port and interact with the service running on that port. For example, ssh is programmed to handle connections over port 22 to send all data and keys. We can connect to TCP port 22 with netcat:
```
netcat 10.10.10.10 22

SSH-2.0-OpenSSH_8.4p1 Debian-3
```
Port22 sent us its banner, stating that SSH is running on it. This technique called banner grabbing, and can help identify what service is running on a particular port. Netcat comes pre installed in most linux distributions. We can also download a copy for windows machine. There is another windows alternative to netcat coded in powershel called powercat. Netcat can also be used to transfer files between machines.

Another similar network utility is socat, which has a few features that netcat does not support, like forwarding ports and connecting to serial devices. Socat can also be used to upgrade a shell to a fully interactive TTY. Socat is a very handy utility that should be a part of every penetration tester’s toolkit. A standalone binary of socat can be transferred to a system after obtaining remote code execution to get a more stable reverse shell connection. 

---
## Service scanning
Service is an application running on a computer that performs some useful function for other users or computers. We call these specialized machines that host these useful services servers instead of workstations, allowing users to interact with and consume these various services. What we are interested in are services that have either been misconfigured or have a vulnerability. Instead of performing the actions expected as part of the service, we are interested to see if we can coerce the service into performing some unintended action that supports our objectives, such as executing a command of our choosing. 

Computers are assigned as an IP address, which allows them to be uniquely identified and accessible on a network. The serviecs running on these computers may be assigned a port number to make the service accessible. As discussed prior, port numbers range from 1 to 65535, with the range of well know ports 1 to 1023 being reserved for priviledged services. Port 0 is reserved port in tcp ip networking and is not used in any messages. If anything attempts to bing to port 0, it will bind to the next available port above 1024 because port 0 is treated as a wild card port. 

### NMAP

Start with the basic scan. Suppose we want to perform a basic scan against a target residing at 10.129.42.253. To do this we should type nmap 10.129.42.253 and hit return. We see that the nmap scan was completed very completely. This is becuase if we dont specify any additional options, Nmap will only scan 1000 most common ports by default. The scan output reveals that ports 21, 22, 80, 139, 445 are available. 

```
nmap 10.129.42.253

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
Under the port heading, it also tells us that these are TCP ports. By default, nmap will conduct a TCP scan unless specifically requested to perform a UDP scan. The STATE heading confirms that these ports are open. Sometimes, we will see other ports listed that have a different state, such as filtered. This can happen if a firewall is only allowing access to the ports from specific addresses. The SERVICE heading tells us the services’ name is typically mapped to the specific port number. However, the default scan will not tell us what is listening on that port. Until we instruct nmap to interact with the service and attempt to tease out identifying information, it could be another service altogether. 

Possible to combine with the -sC parameter to specify that Nmap scripts should be used to try and obtain more detailed information. The -sV parameter instructs Nmap to perform a version scan. In this scan, Nmap will fingerprint services on the target system and identify the service protocol, application name and version. The version scan is underpinned by a comprehensive data of over 1000 service signatures. Finally -p- tells Nmap that we want to scan all TCP ports.

```
nmap -sV -sC -p- 10.129.42.253

Starting Nmap 7.80 ( https://nmap.org ) at 2021-02-25 16:18 EST
Nmap scan report for 10.129.42.253
Host is up (0.11s latency).
Not shown: 65530 closed ports
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxr-xr-x    2 ftp      ftp          4096 Feb 25 19:25 pub
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.14.2
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp  open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http        Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: PHP 7.4.3 - phpinfo()
139/tcp open  netbios-ssn Samba smbd 4.6.2
445/tcp open  netbios-ssn Samba smbd 4.6.2
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: GS-SVCSCAN, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-02-25T21:21:51
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 233.68 seconds


```
This time there is a VERSION heading, which reports the service version and the operating system if this is possible to identify. 
So far, we know that the os is Ubuntu Linux. Application versions can also help reveal the target OS version. Take openssh, for example, we see the reported version is openssh 8.2 ubuntu 4ubuntu0.1m. From insepection of other ubuntu ssh package changlogs, we see the release version takes the format 1:7.3p1-1ubuntu0.1. Updating our version to fit this format, we get 1:8.2p1-4ubuntu0.1. A quick search for this version online reveals that is included in ubuntu linux focal fossa 20.04. 

The script -sC flag causes Nmap to report the server headers http-server-header page and the page title http-title for any web page hosted ont he webserver. The webpage title php 7.4.3 - phpinfo () indicates that this is a PHP infofile, which is often manually created to confirm that PHP has been successfully installed. The title also reveals the PHP version which is worth noting if it is vulnerable. 

## Nmap Scripts
Specifying -sc wil run many useful default scripts against a target, but there are cases when running a specific script is required. For example, in an assessment scope, we may be asked to audit a large Citrix installation. We could use Nmap script to audit for the sever Citriv Netscaler vulnerability, while nmap also has other scripts to audit a critix installation. 

```
locate scripts/citrix
```
The syntax for running an Nmap script is nmap —script <name> -p <port> <host>.
