Ip address have the rule that every device connected to the internet should be unique. However, running out of IP address now, suppose that this is your network.  And there is the private network you belong. 
![[Pasted image 20250317144157.png]] As long as the gateway to the internet knows how to map incoming messages to which computer on the which private network, it is ok to use ip addresses that are not unique. Somehow the gateway should be able to map the ip address. Basic premise is that the gateway should be able to deliver the packet to the correct computer inside the private network. This is done by the network address translation, NAT. 

![[Pasted image 20250317144844.png]]

The operation is pretty simple. NAT masks the port number and source IP address into the dynamically allocated port number and connects the node with the internet. By doing this, the IP address in the private network is meaningless now. 

![[Pasted image 20250317145035.png]]

---
## ICMP

Internet control message protocol

Sending signals, not the real data, used to send the control message between all IP entities. End-hosts, router. No application data is sent. Works directly on top of IP. Checksum is calculated exactly like IP, except it also includes the payload. 

Type and code, echo reply, echo request, etc.
If the TTL is 0, and the router drops the packet→ optionally sends the message i dropped the packet, this message is done through ICMP format. If routers receive the packets too fast, and starts to drop the packet, optioanally dropping the package, uses ICMP format as well. 

PING: sends a ICMP echo request message, receive a ICMP echo reply message, and the machine pinging will calculate RTT using that information. 

TRACErt: trace the route to the destination address. 

ICMP use case: path MTU discovery. TCP doesn’t favour fragmentation. It finds out the MTU of the path, and does not send the packet size that is bigger than MTU. Calculates the MSS as follows, MTU - IP header size - TCP header size. Very common MSS is 1460 bytes. 1500, ethernet payload - headers. MSS is established during the handshake. The sender checks its own technology. The receiver also sends back the MSS (either its own MSS if smaller than the number of sender, or the MSS from the sender). After the handshake, sender will send the packets accordingly to the agreed MSS. If the packet however, hits the router that has the smaller size than that determined MSS, there is a ICMP message, type 3-4, fragmentation needed but DF bit is set. The packet is dropped without any fragmentation. Now the sender is aware of the fact the packet was too big. 1) the router is PMTUD compliant, when sending the ICMP message, it sends its own MTU as well. So sender can recalculate the MSS. 2) not compliant, the sender should reduce the size but not in accurate size. How much lower? Use searching algorithm ex binary search, or the technologies frame size are predetermined, so go try the smaller one. 

What happens if the path and hence the MTU change in the middle of the transmission? The sender has no way to get the information of the change of the MTU. Until the end of the transmission, predetermined MSS will be used. 

![[Pasted image 20250317154233.png]]
