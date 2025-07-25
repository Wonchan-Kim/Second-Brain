![[Pasted image 20250416153651.png]]
Bunch of nodes connected via a medium.
Allows information to be transmitted from one endpoint to another. 
Send 1? 0? The communication is in fact done in multiple layers, requiring different protocols for each layers. 
Protocol is the rules that are ==human-made==. 
![[Pasted image 20250416153850.png]]
Syntax + Semantics + Timing

Internet protocols are made by IEEE, IEEE/IRTF. 
![[Pasted image 20250416154338.png]]
Protocol has to decide how to detect the error, how to deal with it. 
Protocol may decide how to control the transmission speed so that receiver is not overwhelmed.
Why just not send slowly? Not utilizing the bandwidth fully. 
Resource allocation
Fragmentation: a protocol may have to fragment the packet to fit in.
Multiplexing/Demultiplexing: The minimal functionality the protocol has to provide.
Connection Setup, Addressing, naming, compression, encryption, timer management

![[Pasted image 20250416155106.png]]
Defining services into the 7 layers. 

Transport layer: responsible for making sure the packets are being sent correctly to the entity of the receivers, without any loss or errors. While the network is responsible is for routing, transport does not care about the network itself. Cares about only sender and the receiver. 

Session: establishing the connection between the sender and the receiver. 
Presentation: responsible for translation. Ex) linux→mac
In TCP/IP these two layers are incorporated into the application layer. 

Sender, receiver using two routers in the diagram. Which protocl layers are involved. Whatsapp for example. The sender and receiver is going to send it physically, therefore needs the physical layer. Presentation layer is going to take care of the format of the message, the session layer is going to establish the communication. Transport layer is going to ensure the datas are sent without any loss or errors. Networking layer: who is getting this? The address? Forward this packet to the address required
Data link: translates network to physical.

Routers at the same time only requires the three layers. Layers by layers are processed. Each layers is talking to the corresponding layers. Every protocol adds the header.

![[Pasted image 20250416163338.png]]
![[Pasted image 20250416163628.png]]
Each protocol adds the header. By the time the packet reaches the physical layer, the headers are combined as overhead of the packet. For the data link layer, there is also the trailer. 

Example:
![[Pasted image 20250416164736.png]]
![[Pasted image 20250416164752.png]]
Going to the router, ![[Pasted image 20250416164848.png]]
Basic networking concepts:
Network types, PAN LAN WAN Man internet,
PAN: Personal area network: communicate over the range of a person’s workspace like bluetooth.
LAN: Local area network: connects multiple stations together. Local means it is restricted. ==private== Ethernet in this course.
MAN: Metropolitan area network, covering a city.
WAN: wide area network, spanning a large geographical area, often a country, continent, or even multiple continents, CANARIE in canada is a example. 
![[Pasted image 20250416165552.png]]
Internet is focusing on how to incorporate the different types of networks into the internet. SFU having own LAN, CANARIE in WAN, already existing, letting different devices talking to each other. 

## Line Configuration
![[Pasted image 20250416165722.png]]

![[Pasted image 20250416170111.png]]
Shared medium. Everyone can listen to others communication as well. 

Transmission mode

![[Pasted image 20250416170156.png]]

Topology
![[Pasted image 20250416170305.png]]
Mesh: every nodes are connected to each other. 
Star: Use a hub in the middle. Every node is connected to the hub. Reduce the number of connection to the linear level, however, in small scale, there might be a bottleneck. Malfunction, causing delay. ![[Pasted image 20250416170450.png]]
Tree: multiple hubs.
Bus: the most popular networking technology before the wifi, any nodes just tap in. Each cable end just absorbs to prevent the signal reflecting back. Every nodes can audit the connection. Security was not an issue since this connection was in very small scale, like a room.

![[Pasted image 20250416170801.png]]
Ring: token going around, transmission only valid with the token → no collision
Hybrid: attaching different topologies together.

![[Pasted image 20250416170940.png]]

![[Pasted image 20250416171004.png]]
Problem? Send data wait for ack, while waiting the bandwidth is not fully utilized. This can be improved.
![[Pasted image 20250416171135.png]]
Clock synchronization is difficult in networking since it is basically the speed of light. So, here
![[Pasted image 20250416171231.png]]
Primary selects which node to talk to. What about the a→b transmission?

![[Pasted image 20250416171303.png]]
Now primary asks each node to check if they have anything to send, but again which node is waiting for other checking to be finished. 

With all of this problem involved, CSMA/CD was introduced. 
Carrier sense multiple access with collision detection.
This is used in the ethernet, the most widely used type of LAN. 
![[Pasted image 20250416171452.png]]
Instead of wasting time, let them just try transmission. Listen to the shared medium, if the line is not idle, go back to waiting status. If it is idle, send transmission, but keep listening to the line if there is another transmission. If you are the only one you here, it’s ok, however, if not, collision happens, send the jamming signal, to inform the receivers that the collision happened (junk data), then wait the random time, and start again. If there was no collision, the transmission is over. 

The reason of waiting the random time is to prevent the global synchornization. The same thing will happen again with the fixed time of waiting. 

## Flow control

![[Pasted image 20250416171845.png]]
Stop and wait: send packet, and wait for ack, if the receiver is not done, it will not send the ACK, so the sender can slow down. The sender in this scheme can never overwhelm. ACK needs time to be transmitted and propagated. This is too slow.

![[Pasted image 20250416172245.png]] most protocols use sliding window. Between the sender and receiver, the window size is determined, dynamically. As sending more, the window is getting smaller, and the receiver expands the window when it acks. The window of size 0, therefore means the receiver is overwhelmed. In the TCP, the receiver can resize the window size. 

![[Pasted image 20250416172713.png]]
Error control: error detection, error correction.
What causes an error: 1) collision in the shared medium, (2) hardware issue, (3) packet loss,
(4) ==noise== electromagnetic interference, flipping the bits. 
![[Pasted image 20250416173302.png]]
The receiver doesn’t even know if the package was lost. If the sender doesn’t receive the ack before the time out, sends again. This is what used in the stop and wait when there is the lost packet. There is also limit of the sending packet again, then connection is terminated. 

![[Pasted image 20250416173456.png]]

Same as the lost packet, however, the receiver has to drop the first packet. If the sender timer outs, does it know it was ACK lost or packet loss. The sender never knows, but it really doesn’t matter. ![[Pasted image 20250416173557.png]]
In tcp, ack3 means i am expecting 3. The sender has already sent the packet 3 and 4. The sender sends the packet 5, however, the receiver sends the NAK 3. This han’t reached the sender, that is why sender is keep sending. Anything after NAK, will be discarded. The memory was really expensive in the early times.

![[Pasted image 20250416175359.png]]
Restart from the 2. 
The main problem is discarding the good data. 
![[Pasted image 20250416175446.png]]

Alternative: Selective Reject
![[Pasted image 20250416175623.png]]
Now for damaged frame, the receiver saves the correct data. Much more efficient, however, the price is the memory. 


![[Pasted image 20250416175800.png]]

Repeater: hub
Bridge: network2 switch
Router: network3 switch,
Gateway: application layer

![[Pasted image 20250416175830.png]]
Whatever the communication happens, the repeater picks it up and transmits it repeatedly. 
![[Pasted image 20250416175920.png]]
Almost never use the repeater in nowadays. 

![[Pasted image 20250416175941.png]]

Doesn’t blindly copy, instead, the bridge listens to one subnet, and if it has to send it to another subnet, then it retransmits it. For example, for the case D→A, does not repeat. If D→E, then the transmits over. 
![[Pasted image 20250416180058.png]]
The price you are paying: speed. Has to do operations to decide to send or not. 
![[Pasted image 20250416180313.png]]
Routeris more complicated to the bridge. Since bridge already know the address of the other sides. However, the router doesn’t know all the ip addresses. They need to generate the router table. ![[Pasted image 20250416180404.png]]
So router decide which network this message should go to. Routers will connect different networks, technology together. Routers know which way to go. ![[Pasted image 20250416180614.png]]
Gateway is the device that goes all the way up to the application layer. For example what kind of application they are sending, security etc. 
