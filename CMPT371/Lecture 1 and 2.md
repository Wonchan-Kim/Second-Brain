Buyer is fulll, packet is never arrived
The receiver doesn't even know if data is lost. Sender has to do with it/ if sender doesn't revive ack before timer goes out, it retransmits. Amountof time of timer is packet to go, ack to comeback + a bit time for the safety. If ack is not there before timeout, the package is resent. Connectio lost error? Some protocols just quit the connection after several timeout happens. Terminates the connection saying this connection is wrong. 
The receiver received it but ack might be lost. Ack is packet too. In that case, the timer will timeout, sender will send it again, receiver then discards the first packet. If the timer times out at the sender, will the sender ever know it was original packet it was lost or the ack that was lost. The answer is no.

Go back n: data is coming 0-3, ack sent with the expected package, not the last one it received. When the ack 4, it means it is saying I expect 3. 0,1,2 is all good. The sender in fact, has already sent the packet 3 and 4. Meanwhile, receiver finds the error in packet 3. NAK is sent. Then sender restarts from 3 again. After 3 is all garbage. Why discard all even if they are ok? This technology is old. The memory was small especially to be used in the network. We dont have memory! Just send it again. 

In stop and wait, one packet, one ACK, however, in sliding window, u can send multiple packets before you get the acknowledgements. Dont need to acknowledge every packet. TCP usually acknowledged everything. 

If the Fram is lost, for example in the slide 1-54, the receiver receivers the packets 0,1,3. This doesn't make sense since there is no 2. The receiver then sends back the NAK for the packet 2. The packet 4 which was already sent by the sender is discarded, and starts from 2 again. 

GO BACK N the main problem is that discards the good data that already has arrived safely. 

Lost AcK? Sender sends 0,1,2 and the receiver sends ack 3, but it gets lost. The timer runs out and sender, the sender tries again. 

Selective reject : which is an option for TCP. 0,1,2 arrive without problem. But if 2 is wrong, only sends NAK for 2, but now, 3,4,5 is not discarded. It is saved in the buffer. This saves lots of bandwidth. At what cost? The memory, to hold the packets. It's not out of order? The number of the packetsis used to reorder once it has arrived. If the nck is lost? Sends it again. 

Connecting devices and the OSI model. 
Model is the 7 layer model, th internet is not really based on this but TCP, which merges three layers in to the application layer. 

Repeater: hub
Bridge layer 2 switch
Router, layer 3 switch
Gateway: application layer

Shared LAN, reconstruct the signal. 
Function of repeater: corrupted signal -> regenerate

Later 2 switch: bridge listens to each subnet. If each subnet is truing to different subnet, it propagates the signal. If D is trying to send signal to B, which is relocated int he different parts of the internet, then it only propagates the signal. Multiport bridge. If anything has to cross, it will cross. 
Whats the cost? It is good that it doesn't wake the bandwidth. Since it only signal the sped is the cost. The bridge should make the decision, operation , so it slows down a bit. Micro seconds. 
The bridge just doesn't copy conversation blindly. Broadcast only if the subnet is not connected each other. 

Layer 3: router: needs to have routing table, decide which network this msg should go to. One is ring, one is bus tech, and all other technologies is used. The routers will connect the different rechnologioeis and different networks together. In layer 2, it is all the same network. Routers however, the network (LAN: can be different. The routers are resonpsible for connecting different LAN to one another. Decides which way to go. 

Finally the gateway, application layer, we dont have presentation and execution in TCP. The gateway is the on e to the application layer. For example, the security, virus, malsaumware, are they allowed to access the machine in this port. It has to go all the way to the application to see that. 

Switches can work as the sub layer switch. Ex) router can work as a bridge.
_______________________________________________

Lecture 2: Ethernet
Is for the first two laters, physical and data link layer. IEEE 802.3 standard. 802.11 defines the wifi. 
Ethernet used to be the most widely deployed networking technology. What do we do in the data link layer? Logical link control LLC and medium access control MAC. 
Mainly resonpisble for the multiplexing and demultiflexing. If data link receive sth from the network layer, how does it know which technology? It could be different technologies on the sam machine. Data link layer has to multiplex them into one stream. And vice versa. If one stream comes in, decided which technology it should be using,, called demultiplexing. 
 
Ethernet is conncetionlesss ( concept of connectin does not exist): packet loss can be happen, order is not guaranteed.
physical connection (connection oriented: packets all arrive, with order maintained) 

LLC is optional for ethernet

What is ethernet?
One wire going across (ether) maximum cable size depending on the which technology. 10 Base 5 means 10 megabit per second, base means baseband signal, no modulation (just bits), allowing maximum 5 segments connected with 4 repeaters, no longer than 500 meters. 
Maximum 5 subnets. 

How do you actually send the bit?
Manchester encoding.  10000101111
In ideal situation 1 is high, 0 is low, for ethernet, 0 is -8.5 V, 1 is + 8.5 V. why not 0 for 0, 1.7 V for 1? The capacitor stall. Wanna make sure the compensates the charge. The average value will be 0. The circuit without bias. 

The actual encoding
Clock. Twice of fast of transmission. If transmission speed is 5 megabits, the clock is at 10 mega hertz. Why? In manchester encoding, the value changes during transmission of the bit, with the XOR operation with the clock. Why? Becuase, they want it constantly synchoriznie between sender and receiver. : In early digital circuits and communication systems, it was challenging to separately deliver an accurate clock between the transmitter and the receiver. With only the data signal available, it was difficult to reliably determine when a bit started and ended.
Manchester Encoding Solution: Manchester encoding is designed so that the signal must transition at the midpoint of each bit interval (either from 0 to 1 or from 1 to 0). This guarantees that the receiver can easily lock onto the bit timing using these transitions. This property is known as “self-clocking,” which allows synchronization without the need for an additional clock signal line. 

Every machine in the ethernet can hear to each other. Collision may happen if two machines sends at the same time. CSMA/CD cd is for collision detection, simultanesouly, sender sends and listens. Only you hear is your own message, it s good. Even if you hear one bit that was not yours, it means there was collision, therefore you sends the jamming signal, telling to ignore. 

How to decide how long to wait? Randomly?
After i successive collision, a random number between 0 and 2^i - 1 is chosen, known as binary exponential backoff. The station waits for that many time slots, and ‘how many ‘depends on the technology. The time slot is 51.2 micro seconds, check assignment 1. Transmission time for single bits if 10 mebagits per second? 100 nano seconds. Up to 16 successive collisions? 
All the connection and the collision is happening on the MAC layer. If using LLC, connection oriented service, it will ask MAC layer to try again . If you didnt choose connection oriented, and chose connectionless, which is no guarantee, in that case, the application sending it somehow takes care of it. If it goes up to transport layer, and meets TCP, the TCP will take care of it, however, UDP will just dont care about it. Packet is lost forever unless the application layer takes care about it. Application layer? Like fortnite, any application doesnt use TCP have to make its own decision with the lost packets, so application willl take care of it somehow. 

Ethernet traffic is quite intermixed. We know which segment is going where and how by the ethernet MAC layer format. Ethernet 802.3 packet. First 8 bytes is preamble + SOF. Destination address 8 bytes, who is supposed to be the receiver. Source address, who is the sender, length indicating how long it the data, 0-46 padding, 4 bytes of checksum. Ethernet header is consisted of destination address, source address and length. And checksum is the trailer. What about the padding? It is only used when the data is so small (to make segment at least 64 byte). Preamble and SOF is for synchronization. First seven octets are 10101010. Last octet is start of frame, execpt for the last bit, receiver know that synchronizatino is over. The reaosn why destinatino address is the first of the headers is that so if it was not for you, you can just ignore the packet. 이더넷에서 맨체스터 인코딩을 사용하면, 프리앰블의 10101010 패턴이 다음과 같이 변환됩니다.

기본 맨체스터 인코딩 (IEEE 802.3 기준)
0 → High-to-Low transition (↑↓)
1 → Low-to-High transition (↓↑)
프리앰블이 맨체스터 인코딩을 거치면 다음과 같이 변환됨
10101010 → ↓↑ ↓↑ ↓↑ ↓↑ ... (규칙적인 전이 발생)
이렇게 되면 매 비트 주기마다 전이가 보장되므로, 수신기가 정확한 클럭을 복원할 수 있습니다.

Ethernet address
48 bit long and, disaplated in 12 hexadecimal digits. Each 6 octets is displayed in 2 hexadecimal digits, making total 12 hexadecimal digits. How can i get the address?
The address must be globally unique. How to they make sure there is no dupicated oned?
First threee octests are used for organizationally unique identifire, assigned by the IEEE and indicate a amanufactirer, who assigns the remianing octest and programs the hwole address in to the network interface ccard. The company doesnt get only one designated cards, however, gets a bunch. The ethernet address is MAC address also known as the physical address or wifi address. All same thing. 
The least significant bit of the first octet is  a 0 for ordinary adddresses and 1 for group addresses which allow multiple stations to listen to a single address. 
Sending to a group stations is called multicasting, unless you send to absolutely everyone on the network which is balled broadcasting and the address is all 1 s, so FFFFFFFF

Header - length
2 bytes, tells how long ==data== is included. When asking the size of the maximum frame size you can have, the preamble is not included, the address (12 bytes), length 2 bytes, data maximum 1500, padding 0 when data is maxed, checksum 4. Total 1518.

There is also minimum frame length which is 64 bytes, in order to distinguish a valid frame for noise, prevent the sender from completing its transmissino of a short frame before a possible jamming isgnal reaches the sender. 

A and B are at extreme ends. Two extreme ends. A starts transmitting the frame. For ethernet it is frame. Suppose the frame is so small. Before the frame reaches to the B, the B sends sth, and collision hapends. B realizes the collision, however the A takes time to realize the collision. If A has already fidnished transmitting before the collison happens, it will think that there was no collision no matter what, which is not acceptable. The minimum length of the packet has to be something such that the very last bit of the frame, the other side bit should be coming in, to check if there was collision or not. Eveything here is based on one frame. 

Whats  included in data?
The header of IP protocol, some information from the application, tcp header, 
In slide 1-23, one segment on the TCP header needs 44 frame of the ethernet. 
Either LLC or the networking later. 

Pading is discarded. It reads the length field, so it know the length of actual data and discards the padding.


>  slide 2-15
>  checksum in trailer, is used to detect error CRC field that helps the error in the transmission. . If the sender has sent the frame and calculated the checksum, put in the trailer, and the reciever does the checksum again and should get the same result. If the fframe has errors, ethernet just discards the packet. Does it send any ACK? By defualt, it doesnt do anything. If LLC requires connection oreiented or requried ACk it needs. 
>  
>  ip udp tcp wifi also have the same checksum. What do they do? Trnasmitting the grame, 1101011111, first predefine the generator. So the sender and generator both know what this is. In this example, generator is 10011. Basically, divide by the generator (binary division). We hve the remainder, and just adds it at the end of the frame. The remainder is always the length of grnerator - 1. The receiver gets it, divided by generator, expects to get 0. If not, there is some bit error, and drop the packet. How is generator pre determined? The generator is fixed in the standard. 
>  how is division actually happening? First append the n zeros where n is length of generator - 1. The division is done using xor. If two bits are same 0, otherwise get one
> 
> The reason why it is in the trailer, as you are abnout to transmit the frame, and then the checksum comes after so you can calculate. Checksum doesnt need to be stored. Trnasmitting the frame while checksum is being calculated. And dependin gon the result, the frame can be kept or thrown. 
> 
> If they are reading the data when jamming happends, and the checksum will not succeed. Purpose of the jamming signal is to destroy the cheksum. Jamming signal is usualy 6 buytes, and it doesnt matter about the pattern but the sender make sure its not exact crc so checksum will fail.

> 2-17 
> The last bit of the checksum has been processed, and sending next one immediately is not possible. Intergrame gap is a pause to allow the receiver to prepare for another pacekt, resetting counters, and emptyin buffers, clock recovery, etc. Of 12 bytes. Nothing is transmitted for the time that would take for 12 bytes. How long? Depends on the techonology. 
> It is 0 V. neither 8.5 nor -8.5 V. 

> 2-18
> Ethernet efficiency
> Depending on how many stations are trying to send, the channel efficiency varies. 

>  2-19
>  tapping in the line became unreliable. The conncetion of the cable could break or loose, causing the problem to the entire line. Hub (layer 1) was introduced. Removing the idea of the one long physical line but making segments.
>  ok, why not switch 2? Bridge? So it only repeats to the line that actually needs to listen to the message. This drastically reduces the collision into 0. The catch is you can still have collision if you are sending to A, and A sends to other receiver at the same time. The switch itself detects the collision, so it retransmits again. Switch has buffer and processor. This switch is not a data layer. Each connection is isolated so it could be switched. The only thing switch has to know it what is connected to the end of this line. So it has to knwo MAC addresses of each nodes. 
>  every line is half duplex line. If it is full duplex, there is 0 chance of collision, since they will have seperate buffers for the sending and receiving. 

Ethernet 2, known as DIX, there is no length field in the header but the type. It tells what the upper layer protocol was. IPv 4, appletalk, novell netware. Then how do you know the length? It is decided at the physical layer. By the end of dat astream symbol or by loss of carrier signal. IN 802.3 LLC header was packed in betwen the length and data. 
