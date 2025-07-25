To simply to connect together all the existing networks together. Between networks.

There are specific networks called backbone networks, connects the networks together, and they all operate with IP protocols, when we talk about router, we talk about network layer switching, which is also the layer 3 switch. Router only cares about the network layer. 
The internet grew up bottom up, first you have LAN, and then connecting each other. 

> 5-3 Routing in networks
> Interioir router protocol vs exterior router protocol
> If you contorl the entire network by yourself, you have the entire information that which hosts connects to which subnet, the location of hosts, so you can run very detailed protocol, such as OSPF. Open shortest path forward. So interior router protocol is passing routing information with an AS, which is autonomous system, set of networks and routers operated by a single organization. If you are connected to other AS, then routing protocol is different, called as exterior router protocol. Depending on the level of the details you have on the network, there are different categories of routing. 

> 5-4 network layer
> Responsible for sending data from source to destination. This includes the nodes in between, so it is not a end to end. Responsible for routing. 

> 5-5 Internet architecture
> In a tcp ip internet, ip routes provide interconection among physical networks. A device that interconnects two or more networks and passes packets from one to the other is called an internet router. 

> 5-6 IP internet protocol
> The most widely used internetworking protocol, IPV 4, IPV 6. To connect all exsiting networks together.

> 5-7 Internetworking Requirements
> There are many problems to solve
> 1. Different addressing schemes: mac layer, ethernet, wifi 5 g etc. The numbers are all different
> 2. Different maximum packet sizes
> 3. Different network access mechanisms: fiber optics, air, etc
> 4. Different timeouts: there is retransmission, back-off, etc
> 5. Different transmission modes: connection oriented vs connectionless : connection oriented means 1. Packets are guaranteed to arrive 2. Expected to arrive in the same order. If packets are being dropped or not arriving in wrong number, then it is connection less.
> 6. Error control: ethernet doesnt even have it, it has error detection, in the packet using CRC and drops the packet. WIFI also has it. It is not considered as miscommunication btw.
> 7. Flow control: how fast or slow should sender talk. 


> 5-8 IP header

> 5-9 Different addressing schemes solution
> We will make new addressing scheme that sits on any other ones. Introduce 32 bits ip addres as a global address. The format is 32 bit and this was divided into 5 classes. E was experimental, D was for multicast so the options were actually A, B, C. the requirement was that the whole planet no ip address should be same. Later, big exception to this . NAT. Allows usage of private IP addresses. 

> 5-10 Address range
> Class A, starts with 0, can have 2^7 networks, and 24 bits for the host. Range of host address is 1.0.0.0 to 127.255.255.255. Class A onlu has 128 networks, each with 2^24 hosts. Class B starts 10, 2^14 networks each with 2^16 hosts. Class C starts with 110, 2^21 networks each with 2^8 hosts. Network is used to identify the network group or the subnet, while host is the devices that are connected to the network. 
> 
> This was used until 1990 s, it was not the best distribution, think of class A. to many hosts. So it was wasted. 
> 
> The ip routers have different IP addresses for the each subnet. The reason why is that the router should be able to communicate through the physical layer (MAC layer). In each subnet, one to one mapping between mac address and the ip address. 

> 5-12 switch to CIDR.
> Forget about the classes. For each ip address, how many bits you are using for the network part and how many bits you are using for the host parts. You still need to buy (own) the ip addresses, but how you divide is up to you. 
> ![[Pasted image 20250220215314.png]]
> First 20 bits are used for network parts. Remaining number of 0 bits are going to be used for the hosts. This is the example of /20 address. First 20 is used for networking. The figure below which is a class B IP addressm we are using 4 bits form the host portion for the subnet. We have 16 subnets with 4096 hosts, as opposed to having a single network with 65536 hosts. The hosts know which network address they are assigned into. So if the host wants to send the data to another one, it can tell if the destination host is part of my subnet or not. 
> 
> How? Take the ip address of destination, do and operation with subnet mask. The result is subnet address. To determine if the destination is in the same subnet. Subnets are ranked ahead of time. Usually the first host number on that subnet. So you can check if the destination is in the same subnet or not. If you are in the same subnet, you dont have to send the packet out of the subnet. Directly connect with the host in the MAC layer. No need to go to the network layer. 

> 5-22 ARP, RARP
> Address resolution protocol 
> ![[Pasted image 20250220221336.png]]
> Looking for node whose ip address is…
> This request is shared on the entire subnet since ethernet has the shared medium. Intended host then will reply back with the MAC address. This goes into the cache of system A. this is how local mapping is done. Ethernet can not talk ip. 
> This is why router need to have different ip addresses. It has to communicate to the every single physical network it is connected to. 
> 
> RARP
> ![[Pasted image 20250220221944.png]]
> The new host joins the subnet and asks to AP for its IP address. 


> 5-15 Different maximum size
> ![[Pasted image 20250220223259.png]]
> The wifi has 2000, ethernet has 1500. Suppose D → B. maximum size of B exceeded. What if a packet reaches a network and the packet exceeeds the network’s MTU. Use fragementation to split large packets into smaller packets. So R 1 has no choice, sends two packets instead. Challenge is reassembling. When should you do the reassembly? At the next network or at the destination? Dont put it together until receiver, since you  might have to fragment it again.
> 
When the router gets an ip address, the router will run through subnet mask with and operation, extract the subnet address. This is th esubnet the destination. It will simply sends the packet into the packet. If R 1 sends to R 2, R 2 will do the subnet mask again.  


> 5-16 reassemble 
> At the destination, it is reassembled. This results in packets getting smaller as data traverses through the internet. This has efficiency problem, sending smaller packets, the overhead is increased. Overhead in IP is the header normally 20 bytes. It is easy to implement. 

> 5-17 IP fragmentation
> Reassemble at the receiver only. 
> Use   ![[Pasted image 20250220232545.png]]
> Checked ones
> 
> Fragment offset here can go maximum 2^16. By the time it reaches the maximum and havent fisnihed the conversation, it wraps up and starts with 1 again. If one datagram hits the network where the MTU lower than itself. Are identification number same or different? It should be the same. It would not change within the fragments. All fragmets coming from original ip packet will have the same identification number. 
> 
> Protocol: protocol field simply tells IP which is the upper layer protocol which sent this. Either TCP or UDP. Whataspp tcp, is gonna say protocol number of correspondance. 
> 
> What about the order? Internet is the best effort, whcih means connectionless. Might trael trhough different routes, coming in wrong orders. Fragment offset will tell you in which the original packet will start in the fragment. If fragemtn offset is octet 20, if it was orignal fata, skip 20 octets, and thats where the data start from. 
> 
> How do you know it is finished? MF. It is just one bit. 0 → last fragment 1→ still more
> ![[Pasted image 20250220233258.png]]
> Data length should be divisible by 8. So it cant be over 208 since 210 bytes are available except for header. Second fragment, left with 196 octets. Offset is simply 26, as 26 * 8 = 208. Beginning of the fragment is right after the first packet. Very last fragment the data does not need to be divisble by 8 as there is no offset after that. All the other fragments, it has to divisble by octets. 
> 
> Headers are never fragmented. They are simply repeated from the original packet and updated with MF and offset. Other than that, it will only be repeated. 

> 5-19 dealing with failure
> Even if the single packet is missing, you cant put that back together.
> If you cant put it together, assumes it is dropped packet. Reassembly timeout. 

> 5-20 differnent network access
> The router will handle it. It is multi lingual. Router is connected to wifi, connected to ethernet, it can speak both of them. How? They have the network card for it. 
> ![[Pasted image 20250220234537.png]]
> Router X has a network card for ethernet. So it can speak with end system A. suppose it’s whatsapp. And the other part of router X can speak x 25 packet. Router goes up to IP, and checks the address. This address seems like x-25 packet to Y. the router goes all the way up to the network layer. How does the router sends through MAC layer? By ARP. The result of ARP is saved. How to know if the destination is in the same subnet? Looks at the ip address and tells it should go this way. How does it know its on the same subnet? How does it in on the same physical layer? Even to send the ARP, it should know it is in the same subnet. Doing subnet mask. 

> 5-25 different timeout
> Different networks use different timeout mechanisms. Datagrams could loop indefinitely leading to extra traffic and possibly congestion. Solution to this is datagram lifetime. Ip datagram has a time to live field. Each router decrements TTL by one. Once lifetime expires, datagram is discarded. TTL is decreased by one every routing. If it hits 0, the packet is dropped. Optionally, the router can said ICMP message. 

>  5-27 error control
>  CRC checksum only for the ip header. It doesnt care about the data. It only protects the header. There are no errors in TTL, address.. Etc. Whether the data itself, it doesnt really care. If application, data has to be checked, TCP does it. Every router changes at least TTL field, because of that every router has to recalculate the checksum. If there is error, it does not do anything. Optionally, it sends the message ICMP, if packet is discarded. They want the core network to be fast an dumb. 

> 5-28 IP checksum calculation
> ![[Pasted image 20250221013730.png]]
> Checksum field of the ip header. It sets to 0. Once complement addition, of all 16 bit words in the header, then once sum is calculated then takes one’s complement again. 
> If they get the same checksum, its ok or drop it. 
> At each router, or hop checksum is verified first then recomputeed. Why? Because router decreases the TTL field. 

> 5-29 flow control
> Flow control allows routers and stations to limit the rate of incoming data. If buffer is getting overflowed, send the message to the sender to slow down. Doesnt specify how much. Ip sent an ICMP source quench message to a source to request reduced flow. It is deprecated. Today flow control is done with explicit congestion notification, random early detection and token bucket algorithm. 

> 5-30 IP header
> ![[Pasted image 20250221014653.png]]
> IHL, internet header length, isnt the header fixed? No, there are options. So you need to know how big is the header. Differneitated services is used to indicate the quality of service. Congestion control is handled in last two bits. Total length is the length of the header and the data. Identification is like the sequnce number. Just getting increased every send. DF means dont fragment. What happends if MTU smaller than the packet size and DF is 1. Drop the packet. In video app, the DF is usually set 1. Live conferencing. 

> 5-32 Some options
> Security: specifies how secret the datagram is
> Strict source routing: gives the complete path to be followed (router loses the authorization)
> Loose source routing: gives a list of routers not to be missed
> Record route: makes each router append its ip address
> Timestamp: makes each router append its address and timestamp
> Why would you want to kill the trnasmission if TTL reaches 0? To prevent loops. If somhow network was misconfigured, there might be a loop. The buffers will get full. 

> 5-31 
> Internet header length is 4 bits, length of the header in 4 octet units. Minimum is 5, for 20 octects, and maximum is 15, for 60 octets. 
> 
> Maximum total length is 65536 octets. But usually never reaches the maximum size. Why? 
> 1. Wifi and ethernet itself the maximum packet size is 2000 bytes. 
> Minimum length is 576 octets. You cant have smaller than 576 octets. To avoid fragmentation based on early networking technology. 
> 