
How the internet actually does the routing: function that responsible to deliver the packet from the source to destination. Each of the routers on the way make decision which way to send the packet to arrive on the correct destination. Some protocols may use both path. ![[Pasted image 20250329201514.png]]
The router decides based on the routing table. Looks at the destination IP address, check the destination IP address, and gain the routing table. How does it gain the routing table at the first place? The internet is humungous. This is the job of the routing table. Sometimes routing is called static routing, which means, things are done manually, which only works on the small scale. On the larger scale, dynamic/adaptive routing is required. 

![[Pasted image 20250329201815.png]]

Interior vs Exterior protocols

IGP: used for routing within the autonomous system. That is controlled by the single administrator. For example, the SFU net. Also known as intra domain routing. RIP had to essentially know every other routers, pretty soon by 1979 it became impossible with too many routers. Switching to link state routing. Now link state routing is used, which is intermediate system to intermediate system (IS-IS). Mostly used by IGP, within their own network, like TELUS. OSPF is used by organization. 

EGP: designed for sending packets between autonomous system. It has to look up the different information than the IGP, since it has no access to the autonomous system itself. Also known as inter domain routing. EGP is both the category and protocol, facing the limit that is not scalable, switching to the Border Gateway Protocol (BGP). 

---
OSPF (open short path first)
![[Pasted image 20250329202723.png]]
Open: this is an open standard, people don’t have to pay for.
Shortest path: dykstra algorithm
OSPF looks the network as the graph, directed graph, with arrows, originally acyclical, associating the costs to the link. The cost is defined by dividing the reference bandwidth by the interface bandwidth. Interface bandwidth: exist between the two nodes, Reference bandwidth: fixed bandwidth, default value is 100Mbps. For example, if your link is 50Mbps, the cost will be 2. The lower your bandwidth, the higher is the cost. Some routers are actually connected within the shared LAN. OSPF has the rule for that. To go from the LAN to the router, the cost is 0. This is just the rule. Other than that the cost can never be less than 1. This is tghe problem. If you have 10 Gbps line for example, they all get the same cost of 1, making the routing algorithm very inefficient. What we do in reality is that the network operator actually changes the reference bandwidth, usually to the highest bandwidth they have in the network. Then OSPF starts functioning efficiently in the end. 
At lan 4, you can see there is no coming back from the LAN4. We are only interested in arriving to the destination. 

It is possible to divide the network by areas in OSPF. This will make changes more localized. For example. ![[Pasted image 20250329204923.png]]
If new router is added on the area 1, area2 doesn’t really have to take care about it, since it can still reach. However, Area0 is connected to all areas, and areas must go through the backbone. No two areas should not be connected directly. Every area can be connected via the backbone. This is how OSPF manages routing table to become a bit simpler. An area that has only one border router is called a stub. Area 2 is stub in this illustration. How does this translate into different types of routers. ![[Pasted image 20250329205504.png]]
![[Pasted image 20250329205725.png]]
R3, R4, R5 designated router.

![[Pasted image 20250329205810.png]]
LSA is a message that tells you the cost of the links from one to another. LSAs are sent inside the Link State Update (LSU) messages. Each LSA is associated with a sequence number that is incremented for each new packet sent. Alike TCP, an LSA also has an age to protect against sequence number wrap around, router going down and losing its last sequence number. Routers keep track of all the source router, sequence and age. 
![[Pasted image 20250329210222.png]] Each of the link ID is LSA. ![[Pasted image 20250329210413.png]]
Useful information building the routing table. By itself not enough, need to know the shortest path within each routers. Each routers has this database, except for the area border routers. Because they are connected to more than one area, they have more than LSDBs. Area border routers have this database for each area they are connected to. For every area they are connected to has one of these databases. Other than that, within the area, all routers have the exact same copy. Except for the area border routers. 

![[Pasted image 20250329212449.png]]
Neighbours are discovered by OSPF messages, sitting on top of IP. Each pair of neighbours determines are also adjacent (done by protocol in OSPF). Once you determine they are adjacent each other, then you are going to exchange information with that router. As soon as the adjacency is established, the first thing you do is you exchange two routers, what is known as database description. Why not just sharing LSDB itself? It can be really large. When two routers become adjacent, initially, they just send summary of their LSDB. DBD Is just the summary of the all LSAs. If a router notices a newer LSA in the DBD, it asks the sender for that LSA via the Link State Request message (what is this router?), receives the answer via an LSU about the new router, and sends an acknowledgement via the Link State ACK message. Happens only once in the beginning. If they are updates, ex, cost change, link down, new router join, new network, it will send LSA to adjacent routers, expecting the ACK. 

Up to this point, every router is getting from its adjacent routers, information about what other routers or networks, they can reach at what cost. Routers are exchanging information about the topology.  ![[Pasted image 20250329213444.png]]

![[Pasted image 20250329213638.png]]
When these information are exchanged, area border routers is going to send all the LSAs to the backbone. Backbone routers then computer the shortest paths to every other router in the entier Autonomous system. This way, ![[Pasted image 20250329213839.png]]
Not really much to decide, however

![[Pasted image 20250329213855.png]]
For this case, for the router in the area 1, there are options to choose, based on the information coming from the backbone. 

![[Pasted image 20250329214003.png]]
This is the OSPF packet, the type can be hello  for example, send to all of outgoing links, and see what comes back. Link state update, answer to the link state request, which has to be acknowledge, description is DBD message. Therefore, it contains the information that LSAs that specific routers holds. This packet is embedded within the IP packet. How does IP know? From the protocol field, 89 is reserved for the OSPF. UDP is 17, TCP is 8. ICMP is also there. 

![[Pasted image 20250329214258.png]]
/24 means subnet. Topology information. 
Shortest path. OSPF uses the shortest path to create the routing table. ![[Pasted image 20250329214538.png]]
2 paths may have the same cost, OSPF does the equal cost multi path, ECMP, the OSMP is notifying the routers, split them between the different paths, do the load balancing. In the earlier protocol, just choose the first one. Which will cause the congestion. 

The Dijkstra’s algorithm was used. 
![[Pasted image 20250329214940.png]]
The conclusion, whatever comes through A, send to B. Anything that is heading to D will choose B. A does not control the table of B. the routing table is going to choose E, F, H, D from B when heading to the D. 
When there is any cost update, LSA message will be send with update message then re-calculate the entire table. 