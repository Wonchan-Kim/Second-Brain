Internet grew up bottom top ![[Pasted image 20250417182026.png]]
Starts from the LAN, and question arose: How to connect these different networks together.

![[Pasted image 20250417182311.png]]
Two types of protocols: Interior Router Protocol, Exterior  Router Protocol,
The former is when passing routing information within the AS, OSPF. The latter is passing routing information between different ASs, BGP. 

![[Pasted image 20250417183147.png]]
Network layer is responsible for making sure the packet goes from the source to the destination across the network. Not just the LAN, but the different network. Essentially the network layer decides each hop this packet has to go this way or so, this hops are routers. Making the routing decision where to forward the packet to next. This obviously includes the nodes in between, therefore it is not end to end. Contrast to the  Transport layer, caring only end to end (sender and receiver), not caring how actually the packets are forwarded. ![[Pasted image 20250417184322.png]]
![[Pasted image 20250417184435.png]]
The purpose of IP was to connect all networks together.

![[Pasted image 20250417184459.png]]
Each of them have different addressing scheme, maximum packet sizes, network access mechanisms, timeouts, and transmission mode, error control (do CRC check, drop if wrong), and flow control.
Connection-oriented: 1) packets are guaranteed to arrive, (2) to arrive in the correct order
Connectionless: 1) packets may get dropped, 2) packets arrive in wrong order
![[Pasted image 20250417185423.png]]
Solution was implemented by setting IP header. 

![[Pasted image 20250417185507.png]]
Introduced a 32 bit IP address as a global address, E was experimental D was just used for multicast, so options were mostly class A, B and C. All hosts on the Internet must have a unique IP address, except for the techniques such as NAT allows the usage of private IP address that might be duplicated somewhere else. 
![[Pasted image 20250417190150.png]]
First seven bits after 0 (indicator) indicates the numebr of the network and 24 bits for the host. Class B can 14 bits of network, and can have 16 bits of hosts sitting on each network. So class A has 128 networks, each with 2^24 hosts. This is too many number of hosts for the one subnet. This was until early 1990s. 

![[Pasted image 20250417190645.png]]
For routers this is necessity to have multiple IP address. Each port connects to the different subnet, each of the subnets will have differnt IP address, because subnet has their own ip address. The red ip address is the subnet ip address in the diagram. ![[Pasted image 20250417190932.png]]
CIDR. Forget about the classes, tell me how many bits you are using for the network part and  how many using for the host part. This gives more liability on distribvution of the bits. Still have to buy the ip addresses, but how to divide is up to the buyers. 
First 20 bits in the diagram (subnetmask) is used for the network part. Remaining bits are being distributed among the subnet. This can be expressed as /20 address. So here, we have extra four bits for 2^4 subnets. Each with 2^12 hosts. Do bitwise and operation with IP address, and get the subnet address. Check with my subnet ip address and figure out if the destination belongs to the same network as mine. 
![[Pasted image 20250417192743.png]]
In this example, run the subnet mask and check if the subnet id matches. Let’s delve into the last situation, even though the subnet mask doesn’t directly match with the information that R1 has, R1 has the routing table to know it has to send to router 2. 

Does the each subnet in the diagram has the different subnet mask? No, it is already predetermined when distributing the network with hosts. It is the IP address that are changing, every time you have new IP address, go thorough the AND operation to check which subnet it is sitting. 

From outside of this network, it will be obvious to deliver the packet to this router when the IP address is 32~97, the network owner does its own routing inside the network, more efficiently, meeting its’ own requirement. 
![[Pasted image 20250417191924.png]]
![[Pasted image 20250417192120.png]]
If the ip address already exists on the same subnet, you communicate directly using the physical technology, MAC address. But if it is not, it just sends to the gateway, to send to the other network. 

![[Pasted image 20250417193756.png]]
![[Pasted image 20250417193833.png]]
Ethernet segment should be 1500, but lanZ sending 2000 bytes.  What if a packet reaches a network and the packet exceeds the network’s MTU? Cut into the smaller packets. This is called fragmentation in IP. The challenge is reassembling it, to put the fragments together and build the original packet. When should it be reassembled? Until the destination! There is no guarantee there will be only one fragmentation required. ![[Pasted image 20250417194246.png]]
![[Pasted image 20250417194445.png]]
These informations exist in the IP header. 
![[Pasted image 20250417194548.png]]
Identification: sequence number when sends the datagram between the same hosts. Only 16 bits. It wraps around and start from 1 again if the communication is still ongoing after using all of bits. If one datagram where MTU is lower than it. Are identification number same? Yes, so even after the fragmentation, the receiver will know they belongs to the same packet. 
Protocol: protocol of the upper layer. TCP / UDP.

What about the order? Using fragment offset. How to know it is finished, MF is 0. 
![[Pasted image 20250417195128.png]]
Octets, need to find the biggest number divisible by 8. The last fragment does not need to be the multiple of octets. ![[Pasted image 20250417202446.png]]
![[Pasted image 20250417202709.png]]
Addressing scheme: ![[Pasted image 20250417202729.png]]
Router is multilingual, network card attachted allows to speak. 
Suppose lan1 was ethernet, then one side of router can speak Etherent, and other part can say X25. 
Router Y looks at the IP address and knows it has to go to Lan2. How does it know that IP address is on the same physical network? This is data link layer and network layer. Even to send the ARP message, it has to know they are on the same subnet. How does it know? Subnet mask. 

![[Pasted image 20250417203350.png]]
Different networks use different timeout mechanisms, solution is datagram lifetime. IP datagram has a time to live in the header. Every time it passes the router, it decreases TTL by one, and when it becomes 0, datagram is discarded. 
Optionally, the router discarded can send the ICMP message to notify the dropping the message. Otherwise just drop it. 
![[Pasted image 20250417203624.png]]
IP does the checksum only for the ip header. Doesn’t check the data itself. If you have an application that data itself has to be checked, higher layer in most cases TCP cares about it. Every router changes TTL, every router has to change the checksum every time. 
![[Pasted image 20250417203855.png]]
![[Pasted image 20250417204403.png]]
Flow control, IP very minimum level. If the buffer is overflowing, it just says to slow down. IP may send an ICMP source quench message which is deprecated. 
![[Pasted image 20250417204557.png]]
Version: IPv4, IPv6. 
IHL: Header length: options might be included
Differentiated services: quality of service, congestion control are handled in the last two bits.
Total length: header + data
Identification: sequence number
DF: don’t fragment. MTU smaller? Just drop it why? Due to timing: really fast responsive program. Video application, for example live conferencing. No point of retrieving the data that has already moved on. 
MF: more fragment. 
Fragment offset: 
TTL: why kill the packet? To reduce the workload, to prevent the loop. 
Protocol: TCP / UDP / ICMP etc higher level layer

![[Pasted image 20250417205011.png]]
Strict source routing: predetermine the route. 
Loose source routing: let routers decide but with desingated routers in between
Record route: can traverse later
Timestamp: makes each router append its address and timestamp. 

![[Pasted image 20250417205333.png]]
IHL: 4 octet units units, minimum is 5, for 20 octets, and maximum is 15 for 60 octets. 
Differentiated services: for differentiated services and congestion notification. 
Total length: header + data, maximum is 65535, but usually not even near that, physical layer we use wifi or ethernet, 1500 and 2000 each. Biggest packet to begin with. There is also minimum length, at least 576 octets, to avoid fragmentation based on early networking technology. 






