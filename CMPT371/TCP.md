![[Pasted image 20250228185114.png]] 
Options field provides for extra facilities:
Always begin with 1 byte kind indicates what the option is, followed by a 1 byte length that indicates how long the option is. For example, MSS option’s kind is 2 and length is 4. 

TCP has a protocol called path MTU discovery: during connection setup, discovers the MTU of the path, sender to receiver. MTU is maximum transmission unit, all application dont want ip to fragement their data. TCP is going to take care of by making sure asking the MTU and asks for the packet that will fit in MTU. Happens during connection setup. 
MSS, an option to avoid ip fragmentatino is MTU - IP_Header_size - TCP header size. Usually headers are 20 bytes, but options make the header bigger than it. This MSS is calculated each time segment is sent, to make sure that its not sending too much data. Default is 556 bytes, if you add 20 bytes of ip header to it, it becomes 576, which is the minimum size that each router existing in the world must support. It can’t go below that. 

Window scale: tcp uses 32 bit words for the sequence number, and since the transmission speed is way faster these days, it runs out really quickly. Every byte in tcp is numbered. This may cause duplication problem. If two fragement has same sequence number, the receiver can not detect the duplicate properly. You can borrow up to 14 bits from the option field of TCP header, to give a window scale factor. Actual window size = window scale factor * window size in the header . 

Selective acknowledgement:  two ways of acknowledgements so far, stop and wait, which is not very efficient and sliding window. However in sliding window, if something is misconfigured or not acknowledged, start everything again from the point. We can use selective acknowledgement, receiver tells sender which segment was exactly lost,  so sender can send the things those were lost, not the entire packets. 

TimeStamp: is a counter basically, gets incremented no faster than one tic per millisecond.  two purposes.
1) for RTT : sender sends a message and sets a timestamp, once its set, compares the timestamp its sent (starting point) and timestamp in current system (end point). So it is now one millisecond precision, more or less. 
2) protection against wrap ack numbers: duplication problem. Could happen even for the big window size. sending so fast, packets are staying in the network, so there might be still duplicate. Timestamp is gonna help, if sequence number is same, the one with higher timestamp will be taken, duplicates will be removed. 
   
---
TCP header

Source port:  port where source application is residing, whenever the application opens the connection, it is the source. 

How to know if acknowledgement number is valid or not? ACK flag in the header should be set in 1 to say that. If ack is 0, the field does not mean anything. 

TCP header length is included in TCP header. 

CWR : congestion window reduce
ECE: explicit congestion notification echo

Both of them dealt in the tcp congestion lecture. 

PSH: Push flag: I should not be buffering. Whatever you have in buffer just send it right now. Thats how TCP flushes the buffer immediately to the network. 
RST: reset: whenever incorrect connection is established. 

SYN: synchronizing messages when establishing the connection. 
FIN: terminate the connection.

Window size: What is the size of the sliding window? Used by the receiver to send the window size of the sliding window, the exception is that when the protocol is set into window scale factor that the window size is multiplied into that factor. This gives the actual window size. 

Checksum is calculated exactly same way in UDP, using the pseudo header. The protocol number for TCP it is 6, while in UDP it was 17. This is what you put in the pseudo header. 

Urgent pointer: where the data urgent information finishes. What is urgent data? It  depends on the application, and application can mark it as urgent. Terminal app for ex, ctrl c, is urgent, first set PSH, and then put a pointer to then end of ctrl c, so that the receiver knows before terminating the app. The meaning of the urgent is different by the application. Doesnt tell where it starts. The reason why: the receiver as soon as it sees something is urgent. Maybe there is 64kB of data, something urgent ends at 500. So instead of going through the entire data, as soon as the receiver sees the urgent, extract the first 500, and sends it right away. 

Options: followed by the data, 

DATA: maximum size is 76,495 octets. To fit in an ip packet, but in reality never reach the size because layer 2 network technology, mTU is smaller than 64 kb. 

-----
Lecture 8: socket programming

What is socket? Basically communication end point. What is ip address and port number. Sockets have an address consisting of IP address for the machine plus a port number. Receiver has the exact the same thing. Port can be either UDP or TCP, which has to be mentioned. Nothing physically on the device that is socket. Sometimes you see this referred to as Fortopol, ip address, port number and another pair of that. 

Socket programming: most programming languages provide an api to deal with networking and communication. A programmer uses this API to write the necessary code for the networking part of the application. 

Java socket: socket object, which is a constructor, three ways to construct, Socket () creates an unconnected socket, socket (address, port) which creates a socket and connects it to specified port number at the specified ip. Or you can have socker (string host, int port) when you dont know the ip address but the host name

Socket object exclusively works on TCP. In udp, is datagram socket. Since there is no such a thing like UDP connection, there is no connection. Create a datagram socket, converting the message to the bytes, just send it to the destination. In tcp, when you say new socket, it gives address and port number, establishing the connection. There is a TCP connection called my socket (look at the code). In UDP, just send it out to designated address, designated port number. Then you wait for the server response. Wait until the client receives the packet. 
The server as well construct the datagram socket, once the data comes in, because there is no concept of the connection, ip address of the client is derived from the packet it received. In tcp, once you refer to the object, it already knows so no need of this. In java, socket class is for sender in TCP, serversocker is for server in TCP, datagram class is for both server and client. 

In python, if you do in socket. Sock_stream, its tcp, and if it is socket. Sock_dgram, its UDP. In tcp, using recv for receiving, and send all to send, however in UDP, it uses sendto and recvfrom. 

> [!note]  Notes
> Socket is TCP, reliable and connection oriented. Datagram socket is UDP, best effort and connectionless. Some calls are blocking. Every time the server is listening to the client, the code is blocked, meaning the call does not go beyond that. There are always the buffer, while a packet is being is processed, others might arrive. Then they are served one by one. Buffer is the reason we have the queueing theory. Buffers are not only in the internet but in the router as well. 

---
Slide 8-12 application layer protocol

The example was easy, send time, receive time, but there are many more complicated real word example, and we need to distinguish between different messages. Possible solution is to use a toke, before each message, to signigy its meaning. Imagine a shared white board. There are several users collaborating. Access control allows only one user to perform an action at a time. Shared object needs the access control. 

What is real time? There is issue of concurrency and causality. If events occur at almost the same time, how to decide the first one? The definition of real time is that there is a very small delay so there is bare minimum impact on the quality of the service. Some applications are ok with the small delay, while others arent. The official definition of real time is that any system whose response is within the predetermined threshold, which is not fixed. 

One way to do a token connection: first determine between the client and the server the first ‘word’. Always make the first word a token. Token indicates what will follow next. First token says open, then the url of the file follows. If it is DRAW, color and the coordinates will follow. If it CHAT, there are two fields following, the user and the message they have actually. The message is then sent to the server, and the server is sending back to those connected to the server. 

Main problem of the token is, depending on the number of interactions of the application, it might not work. Imagine the word processor, supporting more than 1000 functions. 

Other approaches: Remote Procedure call, instead of messaging scheme, let’s call the corresponding function of each client. If whatever function is doing on the client, just capture that and call the exact same function. Java has the same method called in RMI. As the cross platform was not supported, there was a limit in usage. Hence, trials have been made to overcome this limit from CORBA to SOAP and then the REST API. The problem of the rest api is that you still have to determine every function you want to implement, but the subtle difference is that you can call the function directly, mostly on the web. 

Other issues: how can we have one server support multiple clients at the same time? This problem is due to the underlying algorithm that the server just locks when the client is talking. This is solved by implementing the multithreading. 
How to handle sender crashes gracefully? 

---
Lecture 9: Domain Name System (DNS) Application layer
![[Pasted image 20250306120029.png]]
In the IP lecture, if devices are physically connected on the same network, they already know how to communicate as they are directly connected. If you have ip address of the node, and if it is in the same subnet, then it is possible to find out to figure out the physical address with ARP protocol. IP address of Z, quick subnet mask, and if it matches my own subnet, it means Z is on my physical network. ARP msg asking for the physical address corresponding to the ip address, and Z will send back the physical address. And A can speak directly with Z in this example. 

If not on the same subnet, it is going to the gateway. It is the first level router to get out of the subnet. That router is responsible for finding out where the other ip address is residing. If the gateway fails to find the subnet, it sends to the hub2, which works as a switch and then sends to the internet. 

Note that in the given example, we assume we know the ip address of the destination. However, in real life, when we type the address of the website on the browser, we don’t know the ip address. How does it know the ip address then? The answer to this is using DNS. 

DNS: launched in 1983, the number was so few, so was assigned manually. Main purpose of the DNS was to convert human readable name into unique IP address. The decoupling of readable machine names from their IP address is needed, in that the ip address might be changed. 

Domain name format: the root is indicated by a dot. The fully qualified domain is restricted to 255 characters, and must end in ‘.’. 

Each component divided by . Is called label, restricted to 63 characters. Fully qualified domain name is restricted to 255, summing all them up. Names are case insensitive.

The internet is divided into top level domain, TLD each covering many hosts. Each domain is then partitioned in to subdomains, each subdomain is further partitioned and so on. All of these domains constitute a namespace hierarchy, which can be represented by a tree. TLD domain is consist of generic and countries. 

Once they find the ip address of the domain, they cache it, so they don’t have to repeat the whole thing again. 

---
9-8 top level internet domains
1500 generic TLD these days. Technically speaking, there are only 13 root servers in the world

![[Pasted image 20250306202551.png]]

These IP addresses are always fixed. To solve the issue: how to find the root server. These 13 root servers are replicated and use anycast routing, which allows the actual number of root server instances to be much larger, 1719 last year. There are replicated and spread all over the world. The ISP will use the geologically closest one. The ip address is purposely replicated, as long as you contact any machine that has ip address, it works without any problem. 


9-10 hierarchy of name servers

Single TLD for .com etc. Root servers have the address of all TLD servers. All .com is handled by gtld-servers. Net, then TLD servers themselves are replicated similar to root servers. A.gtld-servers. Net, b, c…. Each TLD server must know the address of all its lower level servers for its subdomains. Convention is that you only have only one lower layer. 

Demonstration of whole process: go to the root and say, i am looking for .org. Then root server indicates the name server. Then you go for it and say I am looking for acm. Org. Recursive process until getting the ip address. Once i have this answer, ISP is going to cache the information, to make sure if another person gonna ask. 

Non authoritative answer: most of the times the answer is coming from the cache, very rare to go to the root server. 
![[Pasted image 20250306210541.png]]

In each subdomain zones, we have primary and secondary server. They are identical in every possible way. When making any sort of request, it always first goes to the primary server. The only time going to secondary is when you don’t get an answer from the primary server. The idea here is to have a backup server in case of the failure. Some system even have a tertiary server. If it is really important, you can put third one there. If you go to any device and check DNS, there is always primary and secondary. Whenever it sends a request to DNS server, it always go to the local DNS server (primary) for the zone the machine is residing. After the few timeouts, it will try from the second one. 

---
Inverse domain

If sending IP address to find a name of the domain, for example you want to find out the 132.34.45.121, then it is represented into the fully qualified domain and into reverse order, such as 121.45.34.132. In-addr. Arpa. . Dont forget the final dot. What is the benefit of such a look up service? For example, some vidoes in youtube are not allowed in certain countries. This is used to filer the zone information. Or online shopping for delivery information etc. 

---

DNS software
 Name server: a server program that supplies name to address trnalsation mapping from domain names to IP addresses. 
 Name resolver: client program, offered by the OS, that uses one or more name servers when translating a name.
 Resolver accesses the closest DNS server, asking for a mapping, which usually involves both recursive and iterative look up. Closest DNS server is already programmed into the machine. The moment you connect , DHCP protocol will tell all the information, including IP address, subnet, gateway (machine when you want to talk to the IP address that is not in your own subnet, basically a router). Resolver, 13 IP addresses are hardcoded. Resolver can look up DNS mapping. 

Iterative: where resolver receives partial answers. 
![[Pasted image 20250311080950.png]]
Suppose the machine has never accessed to the noise. Cs. Uchicago. Edu. What the resolver does, send a msg to the root name server (which is hardcoded), where should I go for .edu? Root name server gives such a name server. The resolver is going to store this information. If my resolver wants to go to the edu again, it will not go to the root server, instead, it already knows by cacheing. It goes now to the .edu server and asks  Who is the nameserver for uchicago. Edu? When it receives the answer, it is going to cache the information again. Now connects to the uchicago .edu name server and asks cs. Uchicago. Edu. Finally going to uchicago. Cs name server, and ask for the rest of the part. All of these information is cached. This is called iterative, the resolver does all the job. 1) router has the copy of all the answers. 2) my local resolver got an authoritative answer, getting answer directly from the name server responsible for that device. 

Recursive happens most of the time: my client does not go through the iterative search, client server instead contacts the local dns server, and asks for the ip address of the required server. Then the local dns server goes through the iterative search, gets the answer and sends to the client server. That answer is not authoritative its coming from the local dns server, not the server it self. 

---
9-16 DNS message format. ![[Pasted image 20250311082441.png]]
Identification: a 16 bit number for client to match the response to the query
You might be asking multiple questions, so identification for each question is needed. 

9-17 DNS communication protocol
Client server architecture, with server usually listening on UDP port 53. Why not TCP? TCP has a overhead. 3 way handshake, just for one packet? UDP is much more faster to send it, no overhead. Just put a timer to check if you got the right answer. Each message has an ID (identification), when the answer comes back it has same id. Server responds using the ID provided. Duplicate responses are dropped, this was a protocol design flow, hackers attacking way. DNS spoofing. More secure DNS trends, DNS over HTTPs, over TLS, DNSSEC. 


























--- 
SMB 
Server message block is a prevalent protocol on Windows machines that provides many vectors for vertical and lateral movement. SMB versions may be vulnerable to RCE exploits such as EternalBlue. It is crucial to enumerate this sizeable potential attack surface carefully. Nmap has many scripts for enumerating SMB, such as smb-os-discovery. Nse, which will interact with the SMB service to extract the reported operating system version. 
```
nmap --script smb-os-discovery.nse -p445 10.10.10.40

Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-27 00:59 GMT
Nmap scan report for doctors.htb (10.10.10.40)
Host is up (0.022s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: CEO-PC
|   NetBIOS computer name: CEO-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2020-12-27T00:59:46+00:00

Nmap done: 1 IP address (1 host up) scanned in 2.71 seconds
```
In this case the host runs a legacy window 7 os, and we could conduct further enumeration to confirm if it is vulnerable to eternalblue. The metasploit framework has several modules for eternalblue that can be used to validate the vulnerability and exploit it, as we will see in a coming section. We can run a scan against our target for this module section to gather informatino from the SMB service. We can ascertain that the host runs a Linux Kernal, Samba version 4.6.2, and the hostname is GS-SVCSCAN.

```
kwc0827@htb[/htb]$ nmap -A -p445 10.129.42.253

Starting Nmap 7.80 ( https://nmap.org ) at 2021-02-25 16:29 EST
Nmap scan report for 10.129.42.253
Host is up (0.11s latency).

PORT    STATE SERVICE     VERSION
445/tcp open  netbios-ssn Samba smbd 4.6.2
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Adtran 424RG FTTH gateway (92%), Linux 2.6.39 - 3.2 (92%), Linux 3.1 - 3.2 (92%), Linux 3.2 - 4.9 (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

Host script results:
|_nbstat: NetBIOS name: GS-SVCSCAN, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-02-25T21:30:06
|_  start_date: N/A

TRACEROUTE (using port 445/tcp)
HOP RTT       ADDRESS
1   111.62 ms 10.10.14.1
2   111.89 ms 10.129.42.253

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.72 seconds
```