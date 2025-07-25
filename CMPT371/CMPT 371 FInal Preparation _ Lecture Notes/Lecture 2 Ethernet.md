Physical and data link layer. 
IEEE 802.3. 
![[Pasted image 20250416181215.png]]
Data link itself is consisted of LLC and MAC. Logical link control sublayer, medium access control sublayer. 
LLC is responsible for multiplexing, demultiplexing, since the network layer might be different (IPV4, IPV6.. Etc), so has to multiplex into the one stream and demultiplex deciding which one it will follow. LLC is also optionally checking the flow and the error control. 
LLC services provide unacknowledged connectionless service (mandatory), connectionless means concept of connection does not exists. All the packets will not necessarly arrive, and not guaranteed to be in the correct order. 

LLC is optional for the ehternet.
![[Pasted image 20250416181451.png]]
Maximum cable size: 10Base2, 10Base5…etc. 
10 means it is 10Mbps. Base means baseband signal (baseband vs passband) means no modulation. It just sends the bits. Each has own maximum length, 10Base5,500m, with 4 repeaters 5 segments connected. How do you actually send the bit?

![[Pasted image 20250416181800.png]]


The value changes during the transmission of the bit. It gets xored with the clock. The clock changes value during the transmission of a single bit. Why? To constantly resynchronize between the sender and receiver. This is a shared one line, and everyone just taps in. ![[Pasted image 20250416183418.png]]
Collision can happen in this shared medium. 

![[Pasted image 20250416183730.png]]
![[Pasted image 20250416183834.png]]
Time slot has to cover the entire transmission, and the jamming signal coming back in the extreme case, where sender and receivers are at the end of the line. Sender should be sending until the jamming signal comes back otherwise the sender will never know. 10Mbps, how long for one single bit? Inverse of it. 

Transmission is done on the MAC layer. IF it was set as connection oriented service in LLC, LLC will actually take care of 16 successive failure case. To use Mac layer. 
![[Pasted image 20250416184739.png]]
Etherent header: address + length
Ethernet Trailer: check-sum 
Padding is only used when the data is less than 64 bytes. 
Preamble is just for the clock synchronization. 

![[Pasted image 20250416184947.png]]
Why would the destination address be in the first part of the header? If the destination address is not you, you can just ignore the entire stuff. ![[Pasted image 20250416185209.png]]
6 bytes. 6 octets are displayed into 12 Hexadecimal digit each. How to get the ethernet address": ethernet address is already hardcoded. First three octets are used for organizationally unique identifier, assigned by the IEEE and indicate a manufacturer, who assigns the remaining octet and programs the whole address in to the network interface card. The company doesn’t get only one designated cards, however, gets a bunch. The ethernet address is MAC address also known as the physical address or wifi address. All same thing.

![[Pasted image 20250416190102.png]]
The length is the length of the data fields in bytes. Not including the padding. Maximum is 1500bytes. The maximum frame size : 12 + 2 + 4 = 18 + 1518 (no padding in this case, preamble is not included). Minimum frame length is 64 bytes (only for 10Base5), to prevent from sender to finish transmission before the sender can receive the jamming signal. When about to send the very last bit, the first bit have to be coming in. 

In ethernet 802.3, ![[Pasted image 20250416191724.png]]
LLC header goes in after the length field. 
![[Pasted image 20250416191845.png]]
![[Pasted image 20250416191909.png]]
Padding is discarded. Since the real data length is defined in the length field. 

![[Pasted image 20250416192021.png]]
Field to detect any error during the transmission. The remainder is always n-1 of n bit generator. By putting the checksum at the last bit, you can calculate the checksum while transmitting. 
In the jamming signal, sender makes sure that jamming signal crc is not the same as the original packet. ![[Pasted image 20250416193302.png]]
Duration last without sending. Interframe gap differs by technology. Is included as part of the frame as an idle pattern which is 0 voltage. ![[Pasted image 20250416193359.png]]
Never 100% for any shared medium due to the collision. 

![[Pasted image 20250416193442.png]]
These taps for ethernet became unreliable after number of nodes increased.
Hub is simply connecting all incoming wires, coming from the ports. Every machine is still connected to each other. Layer 1 switch hub=repeater
![[Pasted image 20250416193614.png]]
Bring a layer 2 switch, only to the line that listens, This drastically reduces the collision to 0. Switch however have the buffer to prevent the possible collision in that if the message sending to the receiver A, collides with the A sending message to other ones at the same time. To prevent this, the switch detects the collision, when with the buffer, waits for retransmission to avoid the collision. The switch therefore has to know the mac address. 

1. Switch has buffer to avoid the possible collision
2. Be smart enough to know when to switch and not to switch
3. Has to know the MAC address of every line
This is only assumption for half duplex line. If it were a full duplex line, there will be no collision. Why? Because the switch will have buffer for both In and OUt. 

Actually in real world, 802.3 ethernet is not the mainstream, instead the Ethernet II is. 
![[Pasted image 20250416194346.png]]
Type has the information what the upper layer protocol is using such as IPV4, remember LLC used to to that. LLC is not in Ethernet 2. No length field, the end of frame is determined at the physical layers, by the end of data stream symbol or by loss of carrier signal. How to they inter operate? Look at type length/type. If it is <=1500, then it is 802.3. 