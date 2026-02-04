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
Static NAT Configuration
The DMZ web server was published using static NAT: ip nat inside source static 192.168.2.10 192.168.3.1
Interface NAT roles:
interface g0/1
 ip nat inside
interface g0/2
 ip nat outside
ACL 100 – External HTTP Access to DMZ
access-list 100 permit tcp any host 192.168.3.1 eq www
access-list 100 deny ip any any
Applied inbound on External interface:
interface g0/2
 ip access-group 100 in
ACL 110 – Block DMZ → LAN Traffic
access-list 110 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
access-list 110 permit ip any any
Applied inbound on DMZ interface:
interface g0/1
 ip access-group 110 in
5. Verifications Performed:
Successful Tests

PC_Internal → Router gateway ping ✅

PC_External → Router gateway ping ✅

PC_External → DMZ Web Server via NAT (HTTP) ✅

DMZ → LAN ping blocked (security enforcement) ✅

Expected Failures (Security Behavior)

PC_External → ICMP to DMZ public IP ❌ (blocked by ACL)

DMZ → LAN access ❌ (blocked by ACL)

Observed Issue

PC_Internal → DMZ Web Server (HTTP)
Result: ❌ Request timed out

This behavior is consistent with ACL placement and filtering applied on the DMZ interface, which restricts inbound access to the DMZ network.
6. Conclusions and Recommendations

This lab demonstrated how NAT and ACLs can be combined to securely expose a DMZ service while protecting internal resources.

Key takeaways:

Interface placement of ACLs is critical and directly impacts allowed traffic

Basic connectivity should always be verified before applying security rules

Security enforcement was successfully validated through controlled failures

For improvement, future configurations could explicitly allow LAN-to-DMZ HTTP traffic if required by business rules.
