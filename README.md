# Networking-Homelab
Building a physical and virtual networking lab to develop hands-on experience after earning my CCNA.
## About This Lab

This project documents my progression from learning networking concepts through the CCNA to applying those concepts in a physical and virtual lab environment.

## Current Equipment

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

### 2. Physical Cabling
- Made multiple RJ45 cables using CAT6W cable.
- Tested all of the cables
- Made patch cables, and also used RJ45 Keystone Jacks
- Connected them to patch panel, and from my LabPC to the Switch.


### 3. VLANs and Switching

Coming soon.

### 4. Routing

Coming soon.

### 5. Virtualization

Coming soon.

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
 
  ### Physical Cabling

  - Had trouble passing through the wires into the RJ45 connectors.
  - Turns out I needed to straighten the wires all the way down at the base, right where they exit the jacket.
  - When using certain crimping tools, there is a lock on it, when trying to do the final crimp, I couldnt connect the RJ45 connecter into it. I had to simply unlock it.
  - The cable tester tested a clean 1-8 sequence on one side, but on the other it would skip from 5, then go to 6 then back to 5. I learned that that usually means that the pin in 5 and 6 are switched and therefore in the wrong spot, and it’s better just to terminate that side again rather than trying to troubleshoot further.
    
