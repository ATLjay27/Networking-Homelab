# Networking-Homelab
Building a basic physical and virtual networking lab to develop hands-on experience after earning my CCNA.
## About This Lab

This project documents my progression from learning networking concepts through the CCNA to applying those concepts in a physical and virtual lab environment. It will be a "warmup" to my first major project which will be a recreation of my current job's network, an enterprise plant network. I will be using this project to get familiar with the tools I will have to use to recreate that network.

## Current Physical Equipment

- Cisco Catalyst 2960 switch
- Dell OptiPlex 5050
- Laptop
- USB to RJ45 console cable
- Cat6 cabling (pure copper)
- RJ45 crimping tools
- Cat6 keystone jacks
- Patch panel

## Labs

### 1. Cisco Console Access

- Connected to the Cisco switch using a console cable
- Identified the COM port in Windows Device Manager
- Configured PuTTY for serial communication
- Accessed the Cisco IOS command-line interface
- Configured the switch hostname
- Erased the memory from old switch owners for a fresh startup config.
- Made a VLAN 99 management IP address of 10.0.0.1


  

### 2. Physical Cabling

- Made multiple RJ45 cables using CAT6W cable in T568B format.
- Tested all of the cables
- Made patch cables, and also used RJ45 Keystone Jacks
- Connected them to patch panel to simulate enterprise environment.


### 3. VLANs and Switching

- Configured SSH to the switch from my laptop, tested it from the Optiplex PC.
- Configured 3 VLANS; HR, Staff, and Guests, put appropriate interfaces in, then created a VLAN for the unused interfaces, and shut them down to prevent VLAN hopping.
- Prepared to use virtualization to connect and form trunks with a virtual Layer-3 Switch.

### 5. VMware Virtualization and Networking

#### Overview

The virtualization portion of this lab introduces a virtual firewall/router using **OPNsense**. The goal was to build a segmented virtual network, configure inter-VLAN gateways, and provide DHCP services for multiple VLANs.

#### Virtual Environment

- **Hypervisor:** VMware Workstation
- **Firewall/Router:** OPNsense
- **Virtual WAN:** Bridged Networking
- **Virtual LAN:** VMware Host-only Network
- **Management Network:** `10.0.0.0/24`
- **OPNsense LAN Address:** `10.0.0.2/24`

#### OPNsense Interface Configuration

The OPNsense VM was configured with two virtual network interfaces:

| Interface | Connection | Purpose | Address |
|---|---|---|---|
| WAN | Bridged | Internet Connectivity | DHCP |
| LAN | Host-only | Internal Lab Network | `10.0.0.2/24` |

### 6. Windows Server & Active Directory

#### Overview

Deployed a Windows Server virtual machine as a Domain Controller for the lab environment. Active Directory Domain Services (AD DS) was installed and configured to provide centralized identity management, authentication, and DNS services for the virtual network.

The domain controller was configured with the `jaylon.lab` Active Directory domain and was connected to the OPNsense LAN through VMware's VMnet1 network.

#### Virtual Environment

- VMware Workstation
- Windows Server
- Windows 11 Client VM
- OPNsense
- VMnet1 Host-only Network

#### Domain Controller Configuration

**Hostname:** `LAB-DC01`

**IP Address:** `10.0.0.4/24`

**Default Gateway:** `10.0.0.2`

**Domain:** `jaylon.lab`

**Roles Installed:**
- Active Directory Domain Services (AD DS)
- DNS Server

#### Active Directory Configuration

Created a new Active Directory forest using:

jaylon.lab

Created an organizational unit for lab users:

Lab Users

Created two test accounts:

- `jaylan.admin` — administrative lab account
- `test.user` — standard user account

The separate administrator and standard-user accounts were used to demonstrate basic identity management and privilege separation within the domain.

### DNS Configuration

Configured `LAB-DC01` to provide DNS for the Active Directory environment.

The domain controller uses:

`10.0.0.4`

as its DNS address.

DNS was tested by resolving:

- `jaylon.lab`
- `LAB-DC01.jaylon.lab`

Successful DNS resolution confirmed connectivity between the Windows client and the domain controller.

### Windows 11 Domain Client

Created a Windows 11 virtual machine named:

`LAB-CLIENT01`

The client was connected to VMware VMnet1 and configured with a static lab address:

- IP Address: `10.0.0.20`
- Subnet Mask: `255.255.255.0`
- Gateway: `10.0.0.2`
- DNS: `10.0.0.4`

The client successfully communicated with the domain controller and was joined to:

`jaylon.lab`

The `test.user` Active Directory account was then used to successfully log into the Windows 11 client.

### Group Policy

Created a custom Group Policy Object named:

`Lab Workstation Policy`

The policy was linked to the `jaylon.lab` domain and configured to demonstrate centralized policy management.



### 7. Hybrid Physical + Virtual Network

#### Overview

The final phase of the lab combines the physical Cisco networking equipment with the virtual OPNsense environment to simulate a small enterprise network.

#### Planned Network

Physical Connection

Connected the OptiPlex physical Ethernet port to the Cisco Catalyst 2960.
Configured the Cisco switch port as an access port on VLAN 99.
Configured the Cisco management interface with 10.0.99.1/24.
Configured the OptiPlex physical Ethernet interface with 10.0.0.10/24.
Verified physical connectivity by successfully pinging the Cisco switch from the OptiPlex.

#### VMware Bridge

Created a bridged VMware network using VMnet0.
Bridged VMnet0 to the OptiPlex physical Ethernet adapter.
Added a third network adapter to OPNsense.
OPNsense identified the new adapter as em2.
Configured em2 with 10.0.99.254/24.
Verified that em2 reported an active link.
Successfully pinged the Cisco switch from OPNsense through the physical network connection.
Subnet Separation

Initially, both OPNsense interfaces were configured on the same 10.0.0.0/24 network. This created a routing conflict because OPNsense had two interfaces connected to the same subnet.

The network was redesigned using separate subnets:

OPNsense LAN (em1): 10.0.0.2/24
OPNsense physical interface (em2): 10.0.99.254/24
Cisco management interface: 10.0.99.1/24
Separating the networks allowed OPNsense to correctly determine which interface should be used to reach each subnet.

#### Routing

The OptiPlex remained on the 10.0.0.0/24 network while the Cisco management interface was moved to the 10.0.99.0/24 network.

A static route was configured on the OptiPlex so traffic destined for the Cisco management network is sent to OPNsense:

This allows the OptiPlex to reach the physical Cisco management network through the OPNsense router while maintaining its normal network connectivity.

#### Troubleshooting

During testing, OPNsense initially had both em1 and em2 configured within the same 10.0.0.0/24 subnet. A route lookup showed that OPNsense was attempting to reach the Cisco through em1 instead of the physical-side em2 interface.

The issue was resolved by separating the networks into different subnets. After changing the physical-side network to 10.0.99.0/24, OPNsense correctly routed traffic through em2.

Additional connectivity testing was performed using:

Ping, tracert, arp, ifconfig, route

These tools were used to identify whether issues were occurring at the physical, Layer 2, or Layer 3 level.

  ## Lessons Learned

  ### Cisco Console Access

  - Learned how to identify the correct COM port in Windows using the "Device Manager."
  - Learned how to establish a serial connection to a Cisco switch.
  - Learned how to factory reset a Cisco 2960 switch using the CLI.
    
 
  ### Physical Cabling

  - Had trouble passing through the wires into the RJ45 connectors.
  - Turns out I needed to straighten the wires all the way down at the base, right where they exit the jacket.
  - When using certain crimping tools, there is a lock on it, when trying to do the final crimp, I couldnt connect the RJ45 connecter into it. I had to simply unlock it.
  - The cable tester tested a clean 1-8 sequence on one side, but on the other it would skip from 5, then go to 6 then back to 5. I learned that that usually means that the pin in 5 and 6 are switched and therefore in the wrong spot, and it’s better just to terminate that side again rather than trying to troubleshoot further.
  - Figured out that the RJ45 connectors that came in the kit I bought were not compatible with my CAT6 AWG23 cable, had trouble passing through at first, but with the new connectors, I got them easily.

    ### VLANs and Switching

    -When trying to SSH into switch from PC, I got an error due to the switch running the older SSH servers that Windows does not support/use. I had to ask Claude, and it ultimately gave me a long command to use when SSH into the switch.

    ### VMware Virtualization and Networking

    -Learned how to install and perform the initial configuration of OPNsense as a virtual firewall/router.
    
    -Learned how VMware virtual network adapters map to different types of network connectivity.
    
    -Learned the difference between Bridged, NAT, and Host-only networking in VMware.
    
    -Learned how to separate a virtual firewall’s WAN and LAN interfaces.
    
    -Learned how to configure a static IPv4 address and subnet on an OPNsense interface.
    
    -Learned how to access and manage OPNsense through its WebGUI.
    
    -Learned how to create and assign VLAN interfaces in OPNsense.
    
    -Learned that VLAN IDs and automatically generated interface/device names are separate concepts.
    
    -Learned how to assign Layer 3 gateway addresses to VLAN interfaces.
    
    -Learned how VLANs can be placed into separate IPv4 subnets for network segmentation.
    
    -Learned how to configure Kea DHCPv4 for multiple VLANs.
    
    -Learned how to create separate DHCP scopes for different subnets.
    
    -Learned the relationship between a VLAN, subnet, default gateway, and DHCP scope.
    
    -Learned that multiple VLANs can share a physical/virtual connection through 802.1Q trunking rather than requiring one interface per VLAN.
    
    -Learned how virtual networking concepts translate to real-world enterprise network architecture.
    
    -Gained practical experience troubleshooting VMware virtual hardware, memory allocation, storage devices, and virtual networking.
    
    -Reinforced CCNA concepts by implementing them in an actual virtualized environment rather than only using Packet Tracer.
    
    -Learned that network troubleshooting often requires separating the problem into layers instead of assuming the network configuration is the cause.

    ### Active Directory & Windows Server

    - Installing and configuring Windows Server in VMware
  
    - Deploying Active Directory Domain Service
    
    - Creating an Active Directory forest and domain
    
    - Understanding the role of a Domain Controller
      
    - Creating Organizational Units (OUs)
    
    - Creating and managing domain user accounts
      
    - Separating administrative and standard user accounts
    
    - Understanding the relationship between Active Directory and DNS
    
    - Configuring a static IP address for a Domain Controller
    
    - Configuring a Windows client for domain connectivity
      
    - Joining a Windows workstation to an Active Directory domain
      
    - Authenticating to Windows using a domain account
      
    - Creating and linking Group Policy Objects
    
    - Understanding centralized identity and policy management
      
    - Troubleshooting virtual networking and DHCP issues
      
    - Using VMware VMnet1 to connect virtual machines within an isolated lab network
      
   ### Hybrid Physical + Virtual Network

    - How VMware bridged networking connects virtual machines to a physical network adapter.
      
    - How a physical Ethernet adapter can connect a virtualized network environment to physical networking equipment.
      
    - Why different OPNsense interfaces should normally use different IP subnets.
      
    - How routers determine which interface to use based on their routing table.
      
    - How overlapping subnets on multiple router interfaces can cause routing problems.
      
    - How Layer 3 routing connects separate IP networks.
      
    - How static routes can direct traffic to a specific remote subnet.
      
    - How to troubleshoot connectivity by testing each section of a network individually.
      
    - How to distinguish physical connectivity, Layer 2 connectivity, and Layer 3 routing problems.
      
    - How a Cisco Catalyst 2960 can provide Layer 2 switching while OPNsense performs Layer 3 routing between networks.
