![[Pasted image 20250417145649.png]]
IOE802.11
Access point is where everyone has to connect to. Goal is to connect to the other parts of networks. APs are typically connected to a wired network, normally an ethernet. Distributed system is each circled separate network. 
![[Pasted image 20250417145948.png]]
Ethernet was wired, this is wireless network. For ethernet, physical layer ww
as pretty siumple, just transmitting the bits through the wire using manchester encoding. In Wiresless LAN, transmission baseband signals modulated at typically 2.4 Ghz or 5Ghz. Which is considered to be in the microwave rang of the electromagnetic frequency spectrum. Transmission is impacted by multipath fading, singals are reflected off solid objects and multiple echoes may reach  recevier along different paths. Some echoes can cancel or reinforce each other, cuasing the received signal to fluctuate greatly. 
The solution is to send the data in to multiple independent paths, so the datas are sent correctly even if the some of the paths are faulty. This is done by using different antennas. 
Or using different frequencies simultaneously during the modulation. These independent paths are typically built in to the digital modualtion scheme, using different ferquencies accross the allowed band, following diefferent paths between different pairs of antennas, or repeating bits over different periods of time. ![[Pasted image 20250417150832.png]]
DSSS and OFDM are the ones used the most recent days, mostly OFDM, also added MIMO later. 
![[Pasted image 20250417151123.png]]
DSSS white curve is baseband signal. If there is a noise that might affect the narrowband waveform, it will be faulty. So to mitigate the affect of the noise, the waveform is then spread wider, into the spread waveform. The baseband signal is modulated with a spreading sequence, which is predetermined, called as Pseudonoise Sequence, this is also known as chip. Chip are chosen such that a much higher frequency, to spread into the wide waveform using XOR operation. The receiver uses the same chip in order to demodulate and extract the baseband signal. Slower than sending each bit directly, but it is a tradeoff between the reliability. 
>  what if you have majority of them are flipped becuase of the noise? There is a checksum. 

OFDM
![[Pasted image 20250417152022.png]]
OFDM is orthogonal frequency division multiplexing, divides the frequency spectrum into a number of sub bands, each carrying bits from the data, so multiple btis are transmitted in parallel. OFDM is resilient against severe channel interference because it uses many slow narrowband signals rather than one fast wideband signal. Subcarriers are orthogonal, each has its peak where all others all zero. Look at the graph, it is evident. Each graph depicts one transmission. ![[Pasted image 20250417153000.png]]

### MAC LAYER
![[Pasted image 20250417153019.png]]
CSMA was when they had the shared medium. Can we apply for the wireless? Same shared medium which is air this time, in the wireless however, there is a range of each signal. A, B, C
C and A are not talking to each other. A sends talking but C can’t really tell since when listening from B, the channel is idle. Now B is getting both signal mixed, now has the garbage. 
![[Pasted image 20250417153325.png]]
To overcome this limitation, CA is used. ![[Pasted image 20250417153626.png]]
Exposed terminal: when B is sending to A, C is also hearing to B and backs off. However, this is not the desired behaviour since there will be no collision when C is sending to D. 
![[Pasted image 20250417154158.png]]
Check the CA diagram again, the sender always gets the ACK for the packet, since there is no other way for the sender to know if it received without any problem. Then the NAV is in the header of each frame and indicates in mircosecond units, how long the sequence which this frame is part of will take to complete, including the time for ack.

 Thats how ‘wait for idle’ is determined. This kinds of solves the hidden terminal problem to some extent. Why? The others might not hear the message to the receipent however they know that the receiver will send the aCK anyway to the sender, so even if they dont hear the receiver the others will remain quiet. Receiver also sends the NAV value, so every devices related to the hidden terminal should remain quiet. When NAV is over, other devices can use the channel again. 

If the sender is not in your vicinity then it is not a problem. 
> 1. Problem: receiver is in your vicinity and also the vicinity of the sender. You dont know that sender is going to send an ACK, so that there is a danger that you send a message during the ack and that will cause the interference 

![[Pasted image 20250417155029.png]]
A RTS to B, C is going to hear that as well. Starts the NAV. B has to reply back to A, even though B is not talking to A, B might be hearing from others. Lide D for example. When B sends the CTS to A, D also can hear that, also idle for NAV. This solves the hidden terminal problem. Even though D and C are not hearing to each other, D and C knows A is talking to B. 

![[Pasted image 20250417155507.png]]
Can D and E talk? No, the signal from D also reaches B and will cause collision. 
Can E talk to D? Yes, no problem here. However, in the protocol, E can not talk to D since D is remaining in silent mode. Some routers actually turn of RTS CTS options in order to utilize bandwidth fully in the risk of collision. 

![[Pasted image 20250417155849.png]]
When frames lost, too much noise or interference, lower the transmission rate, reduce the frame size. Latter is called the fragment. This size is set by the AP. This fragment are individually numbered and acknowledged using a stop and wait protocol. ![[Pasted image 20250417160913.png]]
Duration is used for local NAV. Address is destination, sender and AP. If an AD-HOC, AP address is meaningless. Sequence to determine how frames will be reordered.

![[Pasted image 20250417161136.png]]
![[Pasted image 20250417161200.png]]
![[Pasted image 20250417161758.png]]
