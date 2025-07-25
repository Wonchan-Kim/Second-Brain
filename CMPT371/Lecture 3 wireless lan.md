Wireless lan or known as wifi, standardized by 802.11 family, 1997 1 mebabit ps, to 303 Gbps now. Wireless techonology. Instead of the wire, they are travelling through the air. 
Two modes of wireless lan. There is AP where everyone have to conenct to. APs are connected together. 

Distributed system: if APs are connected wirelessly.

>  3-3 
>  infrastructure mode and ad-hoc mode. Both of these modes are suppported in the standard.

> 3-4 physical layer
> In the ethernet, ony sending with the manchester encoding and synchornization with the clock. Everything is going with the wire.
> However, for the wifi, it sends with typically two frequency, with 2.5 Ghz or 5 Ghz. Transmission is impacted by multipath fading, reflected off solid objects and multiple echoes may reach a receiver along different paths. Waves can bve added to form new waves. The echoes cancel or reinforce each other, causing the received signal to fluctuate greatly. 
> Solution to this either send data with multiploe independent path, using different antennas. Or using different frequencies simultaneosly during modulation. 

>  3-5 DSSS
>  direct sequence spread spectrum
>  the signal will be deteiroated if noise, not decoding as expected. Lets spread the larger frequency band, if there is noise, there are still larger frequency and lower amplitutes, so mitigates the affect of the noise. They modulate the baseband signal, with a preading sequence, which is predetermined, called as pseudo noise (PN) which has a much higher frequency than the baseband signal. Xor operation with the data bit, then transmit. Receiver uses the exact same chip (PN) to demodulate the signal. 
>  slower than sending each bit directly, but it is a tradeoff between the reliability. 
>  what if you have majority of them are flipped becuase of the noise? There is a checksum. 

> 3-6 OFDM predominent technology now
> Orthogonal frequency division multiplexing. Due to the modulation you can take the baseband signal and take it to the different frequency. Each radio station is at different frequency, so differnet channel can be opened. Some of the frequency band can intersect with each other. There is a interference! But the signal they are putting together is orthogonal. Meaning, there is a individuality between the signals. See the slide, when the blue is at the peak, the other singal is apparentely at the bottom (0). Even though there are intersections, detecting the peak is easy. Not only increases the capacities, resilence against the channel interfernece, using small nano bands, how is it used? Depends on the technology. 
> 

> 3-7 ieee 802.1 variants
> MIMO 
> The basic technology is the same

------- 
Mac layer
> 3-8
> CSMA was used for the collision detection. Ethernet was the shared medium. Everyone can sense the communication. MA stands for multiple access, since multiple users can access/ if there is a collision, you can detect it, and send the jamming signal, and try to retrnasmit. This was how it was for ethernet. 
> 
> Can we apply same for wireless? Wireless is also shared medium.  The medium is air. The asnwer is yes and no, but it wont be same, since not everyone can listen to everybodym becuase of the range of the wireless devices. 
> Look at the illustration, A can listen to B, but not C. so C and A cannot send the singal directly to each other.
> Problem: suppose A and C wants to talk to B at the same time. If ethernet, there is no collision since C will wait for A or vice versa. However, in wifi, since A and C can not hear to each other, the B will receiving the garbage (mix of A and C). Only B knows there is a problem. We cant use collision detection using CSMA, as not only station is aware of the collision. 



> 3-8Solution:  
> CSMA/CA, collision avoidance is used. 

![[Pasted image 20250220121253.png]]
B wants to send to D. listens to the medium, A is talking. It knows A is talking to D, since B can get the frame, and see the destination and source address. B starts with a random backoff from 0 - 15 timeslots. Be aware of the fact that it waits for idle first then starts with random backoff. In the illustration, both B and C starts the random backoff. In this example, C’s random backoff time was shorter than B. meanwhile, B *pauses* the back off. Once the C fisnihes communication with D, B resumes the backoff, instead of restarting the backoff, and then sends the data to D. 

> 3-9 Hidden & Exposed terminal

> 3-11 Network allocation vector
> Virtual sensing. Every frame one of the fields in the header is NAV, and indicates in microsecond units, how long the sequence which this frame is part of will take to complete, including the time for ack. In wifi, ack is necessary. Why? Collision can not be detected. The only way sender can assure that receiver got the message or not is to get a response from the receiver saying i got it. Ethernet it is not necessary since it is a shared medium, so everyone can hear. Therefore, in NAV, includes the procesing time including the ACK. Therefore, everyone sharing the medium will know how long to stay idle minimum. 
> Thats how ‘wait for idle’ is determined. This kinds of solves the hidden terminal problem to some extent. Why? The others might not hear the message to the receipent however they know that the receiver will send the aCK anyway to the sender, so even if they dont hear the receiver the others will remain quiet. Receiver also sends the NAV value, so every devices related to the hidden terminal should remain quiet. When NAV is over, other devices can use the channel again. 

If the sender is not in your vicinity then it is not a problem. 
> 1. Problem: receiver is in your vicinity and also the vicinity of the sender. You dont know that sender is going to send an ACK, so that there is a danger that you send a message during the ack and that will cause the interference 

> 3-12 Solution: RTS, CTS
> ![[Pasted image 20250220131133.png]]
> C can hear A, A can hear C, B, B can hear A, D d can hear B. 
> 
> When A sends to B, A sends RTS, so C can catch. C starts NAV.
> B has to reply back to A, clear to send. Even though A is not hearing anything, someone like D might be talking to B. so you have to wait CTS from the receiver, indicating that it is free to send. After B sends CTS, D starts NAV. Even D did not get RTS, got CTS from B, everyone here knows A and D are talking. This solves the hidden terminal problem. 
> 
> Why?
> C is hidden form B, D is hidden form B, yet D and C knows there is a communication between A and B. 

If D and A sends B the RTS at almost the same time, from the perspective of B, the signals will be garbage and not do anything. Both A and D back off and retry after the random time. 

 > 3-13

 > if too many frames are lost, too much of noise or interference. 
 > 1. Lower the transmission rate: once its getting ACK again, it tries with the higher for testing. 
 > 2. Reduce the frame size: break the frame into small pieces called fragments. Each of them ahs own CRC (checksum). The fragmentsize is set by AP. Individuallu numbered and acknowledged using a stop -wait protocol. 
 > can do both. 
 
 > 3-14 802.11 frame
 > ![[Pasted image 20250220145657.png]]
 > data part has more maximum capacity. 
 > first two bytes are frame control, duration: how long this frame+ack will take. As long as other take this part, they will start their own local NAV. 
 > unlike ethernet, there are three addresses. Address 3? AP. It has to go to the AP first, and then AP will relate to the recipient. If it is Ad-hoc, address 3 wont be used. 
 > sequence is the number of the frame. Ethernet, everything is in serial. Whenever we need to deliver in the correct order, there is sequence in the header.
 
> 3-16 Frame Types
> Management - handles service such as association, diassociation, authentication, etc.
> Control: ACK, RTS, CTS handling
These two things dont send data, 
> Data: handles the actual data.

> 3-15 frame control
> 
