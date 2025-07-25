The transport layer cares about sender and receiver which ip didnt care about. The ip cared about every router on the path. We know ip layer is going to deliver the packets to the destination, but transport layer takes cares about sender and reciever itself. Pdu typically called segment. Udp calls it datagram. 

End to end, addressing, multiplexing is the must, reliable delivery, flow control, connection is optional. 

> 6-3 end to end delivery must
> Tcp only cares about the sender and receiver. 

> 6-4 addressing (must)
> Address basically tells transport layer protocol, what application is using. When a packet arrives from the internet, how does it know where it came from ? TCP knwos because it gives different address for different applciations. This is also referred as port number. 

> 6-5 reliable delivery (optional)
> It must have error control, sequence control (right order), loss control, duplication control. 
> 
> Error control-if you want to do it in a network layer, each router has to check the entire packet not jsut the header but also the whole data. If it wants the error control, it needs to get the packet back from sender. So every router should have the buffer. This is expensive in the network layer. At the transport layer, you only deal with sender and receiver. Only the receiver needs to check and ask to sender to send the packet again. So this is the job of the transport layer without bothering the network and data link layer. Does  IP do the error control? No 
> 
> Sequence control  - 
> 
> loss control 
> 
> duplication control
> How can it be duplicated? - loop - resent by sender - problem in the network (router is faulty) 

> 6-10 Flow control done by TCP but not UDP
> Controls the flow of information, again from an end to end perspective. Between of them can be easily determined. Easiest way was to stop and wait. Send a pacekt and wait for ack. It wastes the bandwidth. The sender is idle most of the time. The window protocol. Sender sends few packets, and the amount it negotiated when the connection is established, by reciever suggesting the size of the window. If window is 5000 bytes, you can send the 5000 bytes without waiting for the ACK.
> ![[Pasted image 20250221024500.png]]
> If can be sent area becomes 0, area becomes 0, the sender has to stop, you have to get ack before you send anything. Sent but not acknowledged should be kept to retransmit when you dont get ACK. The rightmost one moves whenever you get the ACK. The receiver can always adjust the size of the window, if the receiver feels it sends too fast, resizes the window to smaller size. The rightmost line moves left in this case. 

> 6-11 multiplexing and demultiplexing
> Multiplexing: converts multiple pdus arriving from higher layer into one outgoing stream. 
> Demultiplexing: converts an incoming stream into multiple PDUs intended for the higher layer. 
> 
> The layer-2 didnt mention higher level protocol. How does it know? Ethernet => LLC. LLC has fields to have what is the higher layer for the packet. IP header has the protocol info in the header. 
> ![[Pasted image 20250221025910.png]]
 IP packet carreis only one TCP segment.
 Next ip packe twill carry another TCP segment. 
 It means mixing different communications into one stream.
 
> From sender

> From receiver
> Looks at the protocol and send it to TCP segment. 
> One of the tcp fields is destination port. 
> It does its own thing. Extracts the payload from the destination port and sends it to the application.  ![[Pasted image 20250221030519.png]]
> 
> One stream coming in , two streams going out. 
> As your going down every PDU has payload and the header. Whole packet has the headers and payload of the lower protocol. 

> 6-14 connection service
> We already know IP connectionless mode. If application requires connection oriented, required in TCP. UDP does not provide this. 
> 1. Connection establishments: sounds easy but tricky. Problem can happen due to old duplicates of either the CR (connection request) or the ACK packets. Receiver gets the CR packets that have expired but can be misinterpreted as new requests. 
>    sender gets ACK packets that have expired but can be misinterpreted as readiness of the receiver to receive. 
>    
>    three way handshake method is introduced. 
> ![[Pasted image 20250221031526.png]]
> The sender sends a request to the receiver and mentions the sequence number that receiver should be expecting. In tcp, every byte is numbered, so sender is telling receiver the first byte i will send you have the sequence number. The receiver sends an ack message, i acknowledge that you will send me from x, and I will also send sequence number Y. as packets are being are exahcnged, both sides can actually talk. Third shake is that sender simply sends the data, and as promised it start with the x, and it also acknowledges the packet thats starts from the y. 

B) indicates the duplicate or late CR. 
> If there is a duplicate from the previous connection, it should be handled, with the rejection ack message. The reason of duplication might be unstable connection or delays in connection. Sends the rejection of either delayed or duplicate request. It is only caught because the sender knows which sequence number it is talking with. Both connection request and actual Data can be duplicated. Case b and c each. 
> ==one scenario that sender cant detect the old connection== : unlikely situation, however, around the same time host 1 actually sends the CR, and it happens to have exactly same sequence number. This is likely to happen due to the sequence number range is predetermined. 16 bits. 
> 
1.  Connection release
- Asymmetric: either side just disconnect, should be avoided due to possible data loss
- Symmetric: each side releases gracefully, waiting until the other side acknowledges disconnection.

>  two army question:
	If both section of blue army, attacks white at the same time, it will outnumber and can win the war, but not vice versa. Blue army 1 sends the spy to army 2, with the messsage that the attack is tmr 6 am. 
	If you are general of the blue army 1, are you gonna attack at 6 am?
	No, if the spy was caught they lose the fight. Spy should come back.
	If you are general of the blue army 2, are you gonna attack at 6 am?
	No, the spy might have been caught on the way back. 

> For release, there is no reliable method.
-![[Pasted image 20250221035231.png]]


> 6-20 UDP user datagram protocol
> 
> Connectionless method
> Multiplexing, demultiplexing so has the address (port)
> Error detection, X control
> X flow control
> X congestion control
> 
> Congestion happens in the network layer, more than its capacity. Capacity is determined by the speed, buffers,,, etc. 
> 
> What does it do then? Just for addressing the applications
> Advantages: no connection setup → fast 
> Dont need connection establishment. For example, real time video chatting, streaming services. Now streaming services using buffering. In UDP, just send again if there is any problem in packets.

> 6-22 Multiplexing/demultiplexing
> UDP header contains the port number of the source and destination. Source is of the sender destinatino port is the receivers port. Machiens are identified by the ip address but the ports (applications) are identifeid by port numbers. Even if several applications transmits at the same time, each UDP packet is identified with the unique header.
> OS of the host checks the address of destination and transfers the data into the application,.

> 6-23 IANA ranges
'> 0-1023 assigned, 1024-49151, registered. Upto 65535, it is dynamic/ it is assigned when the application is launched. Fixed ones are for the server side. When application is launched, TCP gives the application random port numbers. It will tell you the server side of that port numbers. The client side, is going to have different port numbers. The port number of the client side could change every time. 

> 6-24 queues
> Becuase the messages will be forwarded to the ports, one queue for outgoing and other one for incoming. Each for client and server. Buffers are not infinite, so flow control be determined 

> 6-25 UDP header format
> ![[Pasted image 20250221182711.png]]
> Does error detection. 32 bits of headers, source port, destination port, udp length, udp checksum. Cant be smaller than 8 octets to cover the header. Maximum 65515 to fit in IP packet. Slightly smaller than ip packet. 20 octets less. Becuase of the header. But UDP header is 8 octest. Then Why ? Also has to be TCP compliant, which has 20 octets of header. 
> 
> 
> TCP UDP IP are already built in the devices.


> 6-26
> Udp checksum. Protects header and data, using the pseudo header.
> 
> ![[Pasted image 20250221042057.png]]
> UDP assumes that pesudo header exists before doing checksum. Why? To make sure that source and destination correct. How can the ip address can be wrong? Used to have misdelivered ip address, so counting on the transport layer to catch it. Historical reasons, so much not affect nowadays. Just in case the lower layers could not catch the bug in the packet.
> 
> 
> Cheksum: 16 bit summation with the ones complement sum. 
> ===key difference between IP and UDP=== : the checksum is done only on the header vs done with all

> 6-28 Multicasting
> Shared medium X. used the ip multicasting. It deosnt work beacuse of business reasons. 
> All ip router should be multicasting compliant. This is a lot of work. Secondly, sending just one packet and uncountable number of users will receive it. Who will pay for that? 
> 
> The way ip multicasting works is by using the class D. just pick one address and it will represent the group. No central authority decides which ip address to use, so collision can happen. Still used in data centers but not on the internet. 
> However, if you really want to multicast, the protocol avaiable is only UDP. 
> 
> Multicase vs broadcast: selection vs every all of them



