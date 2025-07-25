![[Pasted image 20250417162118.png]]
10Base5 maximum 2.5KM, with 5 of 500 m segments. 
![[Pasted image 20250417162203.png]]
Message is only forwarded when belongs to other bridge. The bridge drops a frame otherwise. ![[Pasted image 20250417162411.png]]
![[Pasted image 20250417173459.png]]
Deletes the old entries periodically. If the machine is quite for a few minutes, any traffic destined to will have to be flooded until the machine sends the frame itself. 
The routing procedure is therefore
1. If the port for the destination is same as the source port, discard it.
2. If the port for the desination address and the source port are different, forward the frame on to the destination port.
3. If the destination port is unknown, use flooding and send the frame on all ports except the source port.
![[Pasted image 20250417173909.png]]
Whatsapp, tcp, ip.

![[Pasted image 20250417173930.png]]
![[Pasted image 20250417174040.png]]
![[Pasted image 20250417174633.png]]
![[Pasted image 20250417174901.png]]
![[Pasted image 20250417174933.png]]
![[Pasted image 20250417175155.png]]
