# DMZ Configuration Report with Cisco Packet Tracer

---

## 1. Lab Objective

The objective of this lab was to design and configure a secure DMZ architecture using Cisco Packet Tracer.  
The lab focuses on implementing routing, static NAT, and Access Control Lists (ACLs) to control and secure traffic between the Internal LAN, the DMZ web server, and the External network.

The goal was to:
- Allow controlled HTTP access to the DMZ web server
- Block unauthorized traffic between network zones
- Validate security behavior using connectivity tests

---

## 2. Implemented Topology

The network was implemented using three isolated zones connected through a single firewall router.

### Network Zones
- **LAN (Internal Network)** – Trusted internal users
- **DMZ Network** – Public-facing web server
- **External Network** – Simulated Internet users

### Devices Used
- 1 × Cisco Router (Router_FW)
- 1 × PC_Internal
- 1 × PC_External
- 1 × Server_PT (Web_DMZ)

---

## 3. IP Addressing Plan

| Device                  | IP Address       | Subnet Mask       | Default Gateway |
|-------------------------|------------------|-------------------|-----------------|
| PC_Internal             | 192.168.1.10     | 255.255.255.0     | 192.168.1.1     |
| Server_DMZ              | 192.168.2.10     | 255.255.255.0     | 192.168.2.1     |
| PC_External             | 192.168.3.10     | 255.255.255.0     | 192.168.3.1     |
| Router_FW Gi0/0 (LAN)   | 192.168.1.1      | 255.255.255.0     | N/A             |
| Router_FW Gi0/1 (DMZ)   | 192.168.2.1      | 255.255.255.0     | N/A             |
| Router_FW Gi0/2 (Ext)   | 192.168.3.1      | 255.255.255.0     | N/A             |

---

## 4. Applied Configuration (Summary)

### Router Interface Configuration
```bash
enable
configure terminal

interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
 exit

interface GigabitEthernet0/1
 ip address 192.168.2.1 255.255.255.0
 no shutdown
 exit

interface GigabitEthernet0/2
 ip address 192.168.3.1 255.255.255.0
 no shutdown
 exit

end
write memory
