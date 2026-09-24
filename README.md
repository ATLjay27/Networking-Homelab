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

Set up a Windows Server environment to gain hands-on experience with Windows administration and Active Directory.

#### Planned Configuration

- Install Windows Server
- Configure a static IP address
- Configure the server hostname
- Install the **Active Directory Domain Services (AD DS)** role
- Promote the server to a domain controller
- Create a lab domain
- Create organizational units (OUs)
- Create test users and groups
- Configure basic Group Policy
- Join a Windows client to the domain
- Test authentication and domain connectivity

### 7. Hybrid Physical + Virtual Network

#### Overview

The final phase of the lab combines the physical Cisco networking equipment with the virtual OPNsense environment to simulate a small enterprise network.

#### Planned Network

The physical and virtual environments will be connected using VLANs and trunking.

- **Cisco Catalyst 2960:** Physical access/IDF switch
- **OPNsense:** Virtual firewall/router
- **VMware:** Virtual networking environment
- **VLAN 10:** `10.0.10.0/24`
- **VLAN 20:** `10.0.20.0/24`
- **VLAN 30:** `10.0.30.0/24`
- **802.1Q trunking:** VLAN transport between network devices
- **Inter-VLAN routing:** OPNsense

#### Planned Configuration

- Configure VLANs on the Cisco Catalyst 2960
- Configure access ports
- Configure an 802.1Q trunk
- Connect the physical switch to the virtual network
- Extend VLANs between the physical and virtual environments
- Test connectivity between physical and virtual devices
- Verify DHCP operation across VLANs
- Document the final topology with a network diagram


## What I'm Learning

This lab will be used to practice:

- Cisco IOS
- Ethernet and physical networking
- VLANs
- Trunking
- Spanning Tree Protocol
- Routing
- DHCP
- NAT
- ACLs
- Network troubleshooting
- Virtual networking

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
    
    
