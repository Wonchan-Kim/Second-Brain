Either a repeater or a bridge can conect seperate physcial networks together with the bridge being a switch that makes switching decisions.

A bridge can also attach multiple seperate LANs together to connect the existing LANs to 
1. Connect the entire LAN in one vuilding
2. Overcome the maximum cable length limits
3. Divided a busy LAN to make smaller less busy LANs

> 4-3 
> Bridge must know whether to 1) forward or drop a frame 1-1) which port to forward to
> Same mechanism for wireless/wired

> 4-4 learning bridge
> Each bridge has a hash table that lists which port is connected to which MAC address destination
> 

> 4-5 updating the hash table
> After while bridge set the hash table, for example if unpluf and plug to other subnet.. Etc
> The connection is dynamic. Therefore the bridge dleetes the entires in the hash table few minutes old. Going to delete old entries. This is the way to keep the dynamic in the system. If a machine is quiet for few minutes, any traffic destined to it will have to be flooded until that machine sends a frame itself. Either the machine has never talked, or long enough time, we are going to broadcast beacuse we dont know the location of the device.
> 1. If the port for the destination address = source port discard the frame
> 2. If the port for the destination address and the source port are different, forward the frame on to the desination port.
> 3. If the destination port is unknown, use flooding and send the frame on all ports except for the source port. 
> The routing is done in layer 2. A bridge is layer 2 switch. Switching operation is deciding to change the flow. 

> 4-6 robustness with multiple links
> Putting two wires into the bridge. This may cause the loop. Solution to this is using the spanning tree.

> 4-7 spanning tree
> Graph that conects all nodes within tree. Any two nodes are connected by only one path. Spanning tree has no loops. Breaks if we break even one link. It is made by the spanning tree protocol.
> First choose a root, whoever has the lowest bridge id. Concatenation of the bridge priority, and the mac address. 
> Then each bridge finds its shortest distance to the root. In case of equal paths being available, the lowest neighbor BID is chosen. 
> The remaining links are blocked.

> 4-8 -10Virtual Lan
> Depending of the usage of the users or room, the LAN might be connected with different purposes. It is not logically makes sense to ask the user to relocate regarding the usage, instead they proposed the virtual Lan, grouping LAN by the unit of project. So there is no physically separate LANs but virtually separate LANS. 
> 
> How? The hosts and the devices should be VLAN compliant. Configuration tables are set up in BALN compliant bridges to match VLANs to ports. To form VLANS, we must decide, how many VLANS will ther be, which device will be on which vlan, and what the vlan will be called.
> 
> VLan compatible devices generates the tagged frame with VLAN tag, the legacy devices that does not support VLAN generates general ethernet frame. If bridge that is compatible with VLAN receives the frame without the VLAN tag, it can add VLAN tag. Also, when transmistting to the device that doesnt need VLAN tag, it can remove the tag. Routing table can be established both manually and dynamically. 
> ![[Pasted image 20250220200544.png]]
> In ths diagram, hexagon with grey colored supports VLAN, and the white ones are the legacy devices. Tagged frame is the frame that is added with the VLAN tag, used in communicatino between VLAN-compliant devices. 
> 
> Trunk is the port that connects devices between VLAN-aware devices. For example, B 1 and B 2 devices are using trunk.
> 
> However, access port is for the legacy devices, that can not perceive the VLAN, VLAN-aware devices send with removal of the VLAN tag, and convert it into the legacy frame. 
> 
> VLAN compliant devices should remove the VLAN tag to not face any trouble when communicating with legacy devices. 
> 
> If device does not give a VLAN number, the bridge will know that it is a legacy device, so it will add a VLAN field, but it will put some dummy numbers. And it willl jsut go thorugh and the last bridge handles it actuallyr emoves the field and gives to the station. 

> 4-11 VLAN Field
> 
> VLan field is increased with extra 4 bytes, so it increases the maximum size of the frame as well to the 1522 bytes. The first two bytes are just VLAN protocol ID, which is always 0 x 8100, they chose it like this so it doesnt get messed up with the actual length, because the length should be something betwenn 0 and 1500. It doesn’t also interfere with the ethernet 2, which had type instead of the length. 3 bits for the priority field, for the quality of service, this message should be going fast, this message should be dropped, etc. Then you have a CFI, which is always 0 except when a token ring frame is going through ehternet. And 12 bits for VLAN identifiers. 

Wifi: does it need a minimum frame size? We needed it in ethernet to detect the jamming signal. Do we need a jamming signal in wifi? We have ack instead. We identify the failure of the transmission by ack, so we dont have to worry about the situation that sender sends without knowing if transmission was successful or not.  