Penetration teset (Pentest) is an organized, targeted, and authorized attack attempt to test IT infrastructure and its defenders to determine their susceptibility to IT security vulnerabilities. A pentest uses methods and techniques that real attackers use. As penetration testers, we apply various techniques and analyses to gauge (measure) the impact that  particular vulnerability or chain of vulnerabilities may have on the confidentiality, integrity and availability of an organization; s IT systems and data.

---
> [!note] External vs Internal
> Perform testing from our own host or from a VPS. When we perform testing from within the corporate network

> [!note] Box types
> - Blackbox: minimal, only the essential information, such as IP addresses and domains is provided
> - Greybox: extended. Provided with some additional information, such as specific URLs, hostnames, subnets, and similar
> - Whitebox: Maximum. Everything is disclosed to us. This gives an internal view of the entire structure, which allows us to prepare an attack using internal information

---
## Process

Deterministic vs stochastic
Process in which each state is casually dependent on and determined by other previous states and events. A stochastic process is one in which one state follows from other states with a certain probability. 
Penetration testing process is defined by successive steps and events performed by the penetration tester to find a path to the predefined objective.

==penetration testing is deterministic!==

____
## Vulnerability assessment

During this phase, we examine and analyze the information gathered during the information gathering phase. The vulnerability assessment phase is an analytical process based on the  findings. 
An analysis is a detailed examination of an event or process, describing its origin and impact, that with the help of certain precautions and actions, can be triggered to support or prevent future occurrences. Any analysis can be very complicated, as many different factors and their interdependencies play a significant role. Apart form the fact that we work with the three different times, during each analysis, the origin and destination play a significant role. There are four different types of analysis. 

Descriptive: essential in any data analysis. On the other hand, it describes a data set based on individual characteristics. It helps to detect possible errors in data collection or outliers in the dat set. 

Suppose we found open TCP port 2121 on a host during the information gathering phase. Other than the fact that this port is open, Nmap did not show us anything else. We must now ask ourselves that what conclusions can be drawn from this result. 

> [!note] We know..
> - a TCP port 2121. TCP already means that this service is connection oriented. 
> - this is not a standard port, system ports resides on 0-1023
> - are there any familiar looking ports in TCP ? Yes, port 21 (FTP). 

Based on this information, we can try to connect to the service using NETCAT or an FTP client and try to establish a connection to confirm or disprove our guess. 
While connecting to the service, we noticed that the connection took longer than usual. There are some services whose connection speed, or response time, can be configured.
Now that we know that an FTP server is running on the ports, we can deduce the origin of our failed scan. We could confirm this again by specifying the minimum probe round trip time (RTT) in Nmap to 15 or 20 seconds and rerunning the scan.

___
## Vulnerability Research and Analysis

We have identified a version of a service or application through information gathering and found a Common Vulnerabilities and Exposures (CVE), it is very likely that this vulnerability is still present. 

Several sources can be used such as CVEdetails, Exploit DB, Vulners, Packet Storm Security, NIST. Once we have found a published vulnerabilities, we can diagnose it to determine what is causing or has caused the vulnerability. Here, we must understand the functionality of POC (proof of concept) code or the application or service itself as best as possible, as many manual configurations by administrators will require some customization for the POC. Each POC is tailored to a specific case that we will also need to adapt to ours in most cases. 

___
###

Suppose we are unable to detect or identify potential vulnerabilities from our analysis. In that case, we will return to the Information Gathering stage and look for more in depth information than we have gathered so far. It is important to note that these two stages; information gathering and vulnerability assessment, often overlap resulting in regular back and forth movement between them. In a CTF, the goal is to get on the target machine and capture the flags with the highest privileges as fast as possible instead of exposing all potential weaknesses in the system. 
