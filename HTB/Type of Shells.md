
Once we compromised a system and exploit a vulnerabilkity to execute commands on the compromised hosts remotely, we usually need a method of communicating with the system not to have to keep exploiting the same vulnerability to execute each command.  To enumerate the system or take further control over it or within its network, we need a reliable connection that gives us direct access to the system's shell, i.e, Bash or PowerShell, so we can thoroughly investigate the remote system for out next move. One wat to connect to a compromised (침해된) system is through network protocols, like ssh for linux or winrm for windows, which would allow us a remote origin to the compromised system. However, unless we obtain a working set of login credentials, we would not be able to utilize these methods without executing commands on the remote system first, to gain access to these services in the first place. 
The other method of accessing a compromised host for control and remote code execution is through shells. There are three type of shells, reverse shell, bind shell, and web shell. Each of these shells has a different method of communication with us for accepting and executing out commands. 

![[Pasted image 20250116213835.png]]

----
## Reverse shell

Most common type of shell, as it is the quickest and easiest method to obtain control over a compromised host. Once we identify a vulnerability on the remote host that allows remote code execution, we can start a netcat listener on our machine that listens on a pecific port, say port 1234. With this listener in place, we can execute a reverse shell command that connects the remote systems shell, i.e, bash or powershell to out netcat listener, which gives us a reverse connection over the remote system. 

### Netcat Listener
The first step is to start a netcat listener on a port of our choosing:

```
kwc0827@htb[/htb]$ nc -lvnp 1234

listening on [any] 1234 ...
```
-l is for listen mode, to wait for a connection to connect to us.
-v is Verbose mode, so that we know when we receive a connection 
-n disable DNS resolution and only connect from/to IPs, to speed up the connection. 
-p 1234 port number netcat is listening on , and the reverse connection should be sent to

We have netcat listener waiting for a connection, we can execute the reverse shell command that connects to us.

---
### Connect Back IP
However, first, we need to find our system's IP to send a reverse connection back to us. We can find our IP with the following command. 

```
kwc0827@htb[/htb]$ ip a

...SNIP...

3: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 500
    link/none
    inet 10.10.10.10/23 scope global tun0
...SNIP...
```
In our example, the IP we are interested is in under tun 0, which is the same HTB network we connected to through our vpn. We are connecting to the IP in tun 0, because we can only connect to HacktheBox boxes through the VPN connection, as they do not have internet connection, and therefore cant connect to us over the internet using eth 0. In the real pentest, we may be directly connected to the same network, or performing an external penentration test, so you may connect through the eth 0 adpater or similar. 

---
### Reverse Shell Command
The command we execute depends on what OS the compromised host runs on i.e, Linux or Wondows, and what applications and commands we access. 
We can utilize the exploit we have other the remote host to execute one of the above commands, through a python exploit or a metasploit module, to get a reverse connection. 

---
Bind Shell
Another type of shell is a Bind Shell. Unlike a Reverse Shell that connects to us, we will have to connect to it on the targets' listening port.

Once we execute a Bind Shell Command, it will start listening on a port on the remote host and bind that host's shell, i.e., Bash or PowerShell, to that port. We have to connect to that port with netcat, and we will get control through a shell on that system.

