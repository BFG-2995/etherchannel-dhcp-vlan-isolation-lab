# Secure VLAN & EtherChannel Lab (Cisco Packet Tracer)

## 🎯 Objective
This project demonstrates how to configure VLAN segmentation with EtherChannel and centralized DHCP using a Cisco router and switches. Inter-VLAN routing is intentionally disabled to improve network security between departments.

## 🧩 Topology
- **R1** – Router with subinterfaces for VLAN 10 and VLAN 20  
- **S1 & S2** – Layer 2 switches configured with EtherChannel  
- **Server** – Central DHCP server (192.168.100.10)  
- **PCs** – Receive IPs from DHCP server according to VLAN

## ⚙️ Configuration Summary
### VLANs
- VLAN 10 – sales 
- VLAN 20 – HR
- VLAN 99 – Management (Native VLAN)

### EtherChannel
- Port-channel1 between S1 and S2 using LACP (`channel-group 1 mode active`)

### DHCP
- Central DHCP server assigns IPs for VLAN 10 and VLAN 20  
- Router uses `ip helper-address` to forward DHCP requests

### Security
- Inter-VLAN routing disabled to maintain departmental isolation

## 🔍 Commands Used

interface GigabitEthernet0/0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.254 255.255.255.0
 ip helper-address 192.168.100.10
!
interface Port-channel1
 switchport mode trunk
 switchport trunk native vlan 99


🔹 SW2 (LACP)
SW2(config)# interface range fa0/22 - 24
SW2(config-if-range)# channel-group 2 mode active
SW2(config-if-range)# switchport mode trunk
SW2(config-if-range)# switchport trunk allowed vlan 10,20,99


Both trunk links use VLAN 99 as the native VLAN:

SW1(config-if)# switchport trunk native vlan 99
SW2(config-if)# switchport trunk native vlan 99

🧠 Router Configuration (R1)

The router is not performing inter-VLAN routing, but only relaying DHCP requests between VLANs and the DHCP server.

interface GigabitEthernet0/0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.254 255.255.255.0
 ip helper-address 192.168.100.10

interface GigabitEthernet0/0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.254 255.255.255.0
 ip helper-address 192.168.100.10

interface GigabitEthernet0/0/0.99
 encapsulation dot1Q 99 native
 ip address 192.168.99.1 255.255.255.0

interface GigabitEthernet0/0/1
 ip address 192.168.100.254 255.255.255.0


Each subinterface acts as a DHCP relay for its VLAN.
PCs get their IPs from the DHCP server through the router.

🧩 DHCP Server Configuration
VLAN	Network	Default Gateway	DNS Server	Range
10	192.168.10.0	192.168.10.254	8.8.8.8	192.168.10.50–192.168.10.100
20	192.168.20.0	192.168.20.254	8.8.8.8	192.168.20.50–192.168.20.100

Server Interface:

IP Address: 192.168.100.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.100.254

🔒 Security Configuration

All devices include banner and authentication settings for secure access.

***********************************************
*  WARNING: Authorized Access Only!            *
*  Unauthorized use is prohibited and monitored *
***********************************************

secrets R1123/S1123/S2123


VLANs cannot ping each other

Router only forwards DHCP packets between VLANs and server

Each VLAN remains isolated as per design

🧰 Tools Used

Cisco Packet Tracer 8.x or later

## How to Run
1. Open the `.pkt` file in Cisco Packet Tracer.
2. Verify VLANs and trunk configurations.
3. Test DHCP allocation on VLAN clients.
4. Check EtherChannel load balancing with `show etherchannel summary`.

## Author
**Thiwanka Rathnayaka**  
Undergraduate | BICT | University of Sri Jayewardenepura
