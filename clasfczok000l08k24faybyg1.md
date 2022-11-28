# Small Office Network Re-design Project

## Network Project
My job is about 50% development and 50% IT, it's the perfect balance for me to stay busy and not be burned out by either half. This week, I began a project to replace our current network switches. This is not as simple as plug and play since we are working with VLANs and multiple networks. The 5,000 square-foot office consists of 12 users, 20 workstations 6 network printers, 15 VOIP phones, 6 Cameras, Access Control, Environmental Monitoring Equipment and 5 physical servers, along with 13 virtual servers. 

## Backstory
I inherited this network setup from my predecessor and have been supporting it for almost 3 years. The network is setup with 4 HP 1950 switches setup with 3 VLANs across three combined switches using trunking, and 1 switch that is standalone. The three switches have specific ports that are setup with Untagged VLAN ports for some of the VLAN traffic that they pass, and the 4th standalone switch is used to segregate un-trusted network devices. There are VLANs setup for all servers/workstations/printers/phones, then a separate one for WiFi, and an separate one for cameras. The problem is there is no separation for management network for the hypervisor host servers, the phone traffic is on the same VLAN as the workstation traffic, and un-trusted devices such as environmental monitoring equipment and mail machine that only need outbound access are on the trusted VLAN network. 

### Issues
- Age of current equipment (~7 years)
- Compatibility with current cyber insurance policies for MFA
- Many inactive ports are live
- No solid map of current layout
- No separation for management network traffic
- Un-trusted devices (mail machine, environmental monitoring devices, etc.) on the same network as trusted network devices. 

## Project

### Goal
The goal of this project was to create a more secure network infrastructure that is well documented, easy to support, and under $5,000 in materials. 

### Plan
I have devised a full redesign of the network that will create a more secure network space that is well documented and easy to support. I created network segments for each type of network traffic, reduced to only the minimum ports necessary, setup Multi-Factor Authentication to protect the network equipment, and created a detailed map of the network layout that corresponds to each office, type of equipment, VLAN associated, and switch port used. 

### New Equipment
To replace the current HP 1950 switches, I decided to budget for and purchase Ubiquiti 48-port POE Pro switches which will accommodate the POE requirements for my VOIP devices and meets MFA requirements for cyber security insurance policy. I already had other Ubiquiti devices on the network so it made since to make this move for a unified network system where devices work cohesively. 

I linked these switches together using 10Gbps connections to ensure minimal latency when traversing switches. Ubiquiti also provides a feature rich dashboard which provides essential insight into the network and will help me optimize network setup to reduce bottlenecks. 

### Layout
I located the office tenant-improvement blueprints from when our company moved into this space and reviewed the network drops. I then went around to every port and mapped the ports to the blueprint. I then identified which ports were in use and which ports were not. Of the ports in use, I notated their purpose (Server, Workstation, Security Camera, Access Point, Printer, etc.). 

Now I know which ports are required, which ports are not needed, and what is connected to each port, and which VLANs each port will need to be connected to on the patch panel. 

![blueprint of office space](https://cdn.hashnode.com/res/hashnode/image/upload/v1669131252566/OVCW0Tfl0.png align="center")

Above you can see a small portion of the office space and some identified ports
**Markings:**
- X = Not in use
- D = Data (PC / Printer)
- V = Voice (VOIP phone)

### VLANs
I have devised the following VLAN structure for our office with color markings to create network segmentation that separates each type of device based on the minimum network connection required, ensuring only like devices can talk to other like devices, and if one segment is compromised it won't directly have access to other devices. 

- VLAN 10 - Management (White)
- VLAN 20 - VOIP (Blue)
- VLAN 30 - LAN (Trusted Workstations / Servers) (Green)
- VLAN 40 - WiFi (Red)
- VLAN 50 - Cameras (Pink)
- VLAN 60 - Access Control (Purple)
- VLAN 1 - Un-trusted (outbound only devices) (Yellow)


![color coded blueprint of layout ](https://cdn.hashnode.com/res/hashnode/image/upload/v1669131372635/WqPxsTFl6.png align="center")

### Rack Setup
Now that we know our Ports, and our VLAN setup, we can map out the patch panel ports and the switch ports to correspond to the new setup. 

Here you can see the patch panel ports which is two 48-port rack mounted patch panel units, and three 48-port Ubiquiti POE Pro switches. I'm opting for 3 switches to create load balancing and to have redundancy along with continuity if one were to fail. 

![Image of network patch panel and switches with color coded labels](https://cdn.hashnode.com/res/hashnode/image/upload/v1669132715316/hnc8yVf2C.png align="center")

## Conclusion
This was a great small office network redesign project. The goals of this project were to create a more secure network structure that was well documented and easy to support. The overall cost of this project in materials was $3,976 before tax. 

It was absolutely worth all the time it took to plan and prepare for this project and a full network redesign. I did not want to carry forward problems such as lack of documentation, lack of segmentation, and over exposure of network port access. The business network is now more secure, less complex, and with new hardware will last 5-8 years without needing to be replaced. The next person to support the network after me will have a much easier time due to the documentation created as a result of this project. 