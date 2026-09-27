# Lessons Learned

This document captures the practical lessons, troubleshooting experiences, and networking concepts I developed while building the Networking Homelab. The goal of the lab was not simply to make everything work, but to understand why it worked, troubleshoot problems when it did not, and connect the concepts learned through the CCNA to a real physical and virtual environment.

## Cisco Console Access

Working with the Cisco Catalyst 2960 provided my first hands-on experience accessing and configuring a physical Cisco switch.

I learned how to:

* Identify the correct COM port in Windows Device Manager.
* Establish a serial console connection to a Cisco switch.
* Configure and navigate the Cisco IOS command-line interface.
* Configure a switch hostname and management IP address.
* Factory reset a Cisco 2960 and remove configuration left by previous owners.

This reinforced the difference between learning Cisco commands in a simulated environment and actually connecting to and configuring physical equipment.

## Physical Cabling

Building my own Ethernet cables ended up being one of the more hands-on parts of the lab. I initially had trouble getting the individual wires through the RJ45 connectors correctly.

One of the main things I learned was that the conductors need to be straightened all the way down to the base of the cable, right where they exit the outer jacket. This made it much easier to get the wires through the connector in the correct order.

I also ran into an issue with my crimping tool. The tool had a locking mechanism that prevented the RJ45 connector from being inserted correctly when I was attempting the final crimp. The solution was simply to unlock the tool.

Cable testing provided another useful troubleshooting lesson. One cable showed a correct 1–8 sequence on one side, but the tester showed an unusual sequence on the other side where the numbers jumped from 5 to 6 and then returned to 5. This indicated that pins 5 and 6 had been switched. Rather than spending additional time trying to troubleshoot the physical connector, I learned that it was often faster and more reliable to re-terminate the problem end.

I also discovered that the RJ45 connectors included with my original kit were not a good match for my CAT6 AWG23 cable. The conductors were difficult to pass through the connectors. After switching to compatible connectors, the termination process became much easier.

These problems reinforced an important lesson: **physical-layer troubleshooting should start with the physical components.** A network problem does not always originate in the switch configuration or software.

## VLANs and Switching

Configuring VLANs gave me practical experience applying Layer 2 concepts from the CCNA to physical equipment.

I configured separate VLANs for different groups and assigned switch interfaces appropriately. I also created a VLAN for unused interfaces and shut those interfaces down. This gave me practical experience with basic switch hardening and reinforced why unused ports should not simply be left active.

I also configured SSH access to the switch and tested access from another system on the network.

One issue occurred when I attempted to SSH into the switch from a Windows PC. The switch was running an older SSH implementation that was not compatible with the SSH client behavior on my Windows system. Troubleshooting this required researching the compatibility issue and finding an appropriate way to establish the connection.

This was a useful reminder that **configuration correctness and compatibility are two different problems**. A service can be configured correctly while still having compatibility issues with the client trying to connect to it.

## VMware Virtualization and Networking

The virtualization portion of the lab significantly expanded my understanding of networking because I had to connect virtual network concepts to actual network behavior.

I learned how to install and perform the initial configuration of OPNsense as a virtual firewall/router and how VMware virtual network adapters correspond to different types of connectivity.

In particular, I gained hands-on experience with:

* Bridged networking
* NAT networking
* Host-only networking
* Separating WAN and LAN interfaces
* Configuring static IPv4 addresses and subnets
* Accessing and managing OPNsense through its WebGUI
* Creating and assigning VLAN interfaces
* Assigning Layer 3 gateway addresses to VLANs
* Placing VLANs into separate IPv4 subnets
* Configuring Kea DHCPv4 for multiple VLANs
* Creating separate DHCP scopes for different subnets
* Understanding the relationship between VLANs, subnets, default gateways, and DHCP

One concept that became much clearer through the lab was that a VLAN ID and the interface or device name assigned by a system are separate concepts. The naming of a virtual interface does not necessarily correspond directly to the VLAN number assigned to it.

I also learned that multiple VLANs can share a physical or virtual connection through **802.1Q trunking** rather than requiring a separate physical interface for every VLAN. This helped connect the virtual environment to the way VLANs are commonly implemented in enterprise networks.

Working with VMware also gave me practical experience troubleshooting virtual hardware, memory allocation, storage devices, and virtual networking. These were issues that would not necessarily appear when working only with Packet Tracer.

Most importantly, implementing these concepts in a real virtualized environment reinforced the CCNA material in a way that simulation alone could not.

## Active Directory and Windows Server

Building the Windows Server portion of the lab introduced another side of network administration: centralized identity, authentication, DNS, and policy management.

I learned how to:

* Install and configure Windows Server in VMware.
* Deploy Active Directory Domain Services.
* Create an Active Directory forest and domain.
* Understand the role of a Domain Controller.
* Create Organizational Units.
* Create and manage domain user accounts.
* Separate administrative and standard user accounts.
* Understand the relationship between Active Directory and DNS.
* Configure a static IP address for a Domain Controller.
* Configure a Windows client for domain connectivity.
* Join a Windows workstation to an Active Directory domain.
* Authenticate to Windows using a domain account.
* Create and link Group Policy Objects.
* Understand centralized identity and policy management.
* Troubleshoot virtual networking and DHCP issues.
* Use VMware VMnet1 to connect virtual machines within an isolated lab network.

Creating separate administrative and standard user accounts also helped demonstrate the importance of privilege separation. Rather than using an administrator account for everything, the lab provided an opportunity to see how different levels of access can be represented within a domain environment.

Working with Active Directory also reinforced how dependent many Windows network services are on DNS. Successfully resolving the domain and Domain Controller demonstrated that connectivity is not simply a matter of being able to ping another machine; the underlying services also have to function correctly.

## Hybrid Physical and Virtual Networking

The hybrid portion of the lab was one of the most valuable parts because it connected the physical Cisco environment to the virtual OPNsense environment.

I learned how VMware bridged networking can connect a virtual machine to a physical network adapter and how a physical Ethernet connection can be used to connect a virtualized network environment to physical networking equipment.

The lab also demonstrated why different router interfaces should normally use different IP subnets.

Initially, both OPNsense interfaces were configured on the same `10.0.0.0/24` network. This created a routing conflict because OPNsense had multiple interfaces connected to the same subnet.

A route lookup showed that OPNsense was attempting to reach the Cisco switch through the wrong interface. The problem was not the physical cable or the Cisco switch itself; the issue was the network design.

I resolved this by separating the networks into different subnets:

* OPNsense LAN: `10.0.0.2/24`
* OPNsense physical-side interface: `10.0.99.254/24`
* Cisco management interface: `10.0.99.1/24`

After separating the networks, OPNsense correctly determined which interface should be used to reach the Cisco management network.

This was one of the clearest examples in the lab of why understanding routing is more important than simply memorizing commands. The configuration initially appeared reasonable, but the overlapping subnets created an ambiguous routing situation.

I also learned how static routes can direct traffic toward a specific remote subnet and how a Layer 3 device can connect otherwise separate IP networks.

## Troubleshooting Methodology

One of the biggest lessons from the lab was that troubleshooting should be approached systematically rather than by changing random configuration settings.

During the hybrid networking phase, I used tools such as:

```text
ping
tracert
arp
ifconfig
route
```

These tools helped determine whether a problem was occurring at the physical, Layer 2, or Layer 3 level.

The lab reinforced the importance of breaking a problem into smaller sections:

1. Is the physical connection working?
2. Is the interface active?
3. Is Layer 2 connectivity working?
4. Is the device in the correct VLAN?
5. Is the IP address and subnet correct?
6. Is the default gateway correct?
7. Does the routing table contain the expected route?
8. Is the traffic reaching the correct interface?
9. Are services such as DNS or DHCP functioning?

This approach helped me avoid assuming that every connectivity problem was caused by a configuration error on the device I was currently looking at.

## Overall Takeaways

The biggest benefit of this homelab was being able to turn concepts I had studied for the CCNA into working systems.

The lab reinforced that networking is not just about knowing commands. It requires understanding how the physical layer, switching, VLANs, IP addressing, routing, DNS, DHCP, virtualization, and operating systems interact.

I also learned that troubleshooting is often more valuable than simply getting a configuration working on the first attempt. The problems I encountered with cable termination, SSH compatibility, VMware networking, VLANs, and overlapping subnets forced me to investigate what was actually happening instead of relying on assumptions.

The hybrid network was particularly useful because it demonstrated how a Cisco Catalyst 2960 can provide Layer 2 switching while OPNsense handles Layer 3 routing between networks.

Overall, the project gave me practical experience that I could not get from studying theory alone. It also established a foundation for more advanced networking projects involving larger topologies, routing, virtualization, Windows infrastructure, and enterprise-style network design.
