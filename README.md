# Mini Project — Enterprise HQ & Branches Network (BITS 2343)

## Overview

Capstone group project (5–6 members) integrating everything from the semester into one enterprise-style simulation: a headquarters network segmented with VLANs and inter-VLAN routing, two branch networks sized with VLSM, EIGRP dynamic routing across all routers, ACL-enforced communication policy, distributed DHCP with relay agents, and Web/DNS/Mail services topped with a custom-hosted homepage. Deliverables were a Packet Tracer simulation, a written report (physical diagram, logical diagram, addressing table, verification of every requirement), and a recorded video demonstration.

## Objectives

Implement and verify eight requirements:

1. **IP address allocation** — HQ from `192.168.X.64/26` and Branches from `172.X.0.0/16` (X = group number; this group's X = 150), with the ISP simulated as Loopback X
2. **VLANs** — Blue, Red, and Orange VLANs for HQ host groups
3. **VLAN port assignment** — access ports for hosts, trunk ports between switches and to the HQ router
4. **Inter-VLAN routing** — dot1q subinterfaces on the HQ router
5. **Routing & access control** — EIGRP everywhere plus ACLs enforcing the communication policy
6. **DHCP** — dynamic addressing for all hosts except the Orange network, using per-VLAN DHCP servers and `ip helper-address` relay
7. **Web, DNS & Mail servers** — including mail accounts for every group member
8. **Homepage customization** — site hosted at `<group_name>.utem.edu.my` with member photos and names

## Technologies & Tools

* Cisco Packet Tracer
* Cisco IOS CLI (multiple routers and switches; serial DTE/DCE WAN links, UTP Cat5 LAN cabling)
* VLSM, VLANs, 802.1Q trunking and subinterfaces, EIGRP, ACLs, DHCP + DHCP relay (`ip helper-address`), DNS, HTTP, SMTP/POP3

## Network Topology

Two sites joined over serial WAN links, plus an ISP:

* **HQ** (`192.168.X.64/26`): HQ router → switches S1/S2 serving three VLANs — Blue (hosts B1–B2 + Blue DHCP server), Red (hosts M1–M8 + Red DHCP server), Orange (J1–J3 + Orange Web server)
* **Branches** (`172.X.0.0/16`): routers BRCH1 and BRCH2 → switches S3/S4 — Purple network (P1–P3 + Purple DHCP & DNS server, sized for **16,000 addresses**) and Green network (G1–G4, sized for **4,000 addresses**)
* **ISP** reachable via Loopback X

## IP Addressing

Designed per group from the allocated blocks. With X = 150: HQ = `192.168.150.64/26`, Branches = `172.150.0.0/16`. VLSM sizing constraints: Purple ≥ 16,000 hosts (/18-scale block) and Green ≥ 4,000 hosts (/20-scale block). The final per-device addressing table is part of the submitted report — *specific assignments not included in this worksheet, see the project report*.

## Communication Policy (enforced with EIGRP + ACLs)

* All Branch hosts communicate with each other, and with **only** the Orange network at HQ
* Routes taken by Branch hosts are identified and documented
* Red VLAN and Blue VLAN hosts communicate only **within their own VLAN**, plus the Web Server (Orange server)

## Network Configuration Highlights

Representative configuration areas required by the spec (full configs are in the report appendix):

```text
! Inter-VLAN routing on HQ router
interface GigabitEthernet0/0.<VLAN_NUMBER>
 encapsulation dot1q <VLAN_NUMBER>
 ip address <subif-ip> <mask>

! EIGRP on all HQ and Branch routers
router eigrp <AS>
 network ...

! DHCP relay on router interfaces whose clients use a remote DHCP server
interface <client-facing-interface>
 ip helper-address <dhcp-server-ip>
```

DHCP assignments: Red VLAN ← Red Server, Blue VLAN ← Blue Server, Green + Purple networks ← Purple Server (in the Branches network). Orange network hosts are statically addressed.

## Verification & Testing

Per the report requirements, every listed requirement was verified with a suitable command (e.g., `ping`/`traceroute` for the policy matrix, `show ip route` for EIGRP routes, `ipconfig` for DHCP leases, browser access to `<group_name>.utem.edu.my`, and sending/receiving mail between member accounts), captured in the Result & Discussion section and demonstrated in the video.

## Results

A converged multi-site network meeting all eight requirements, documented with physical/logical diagrams, a full addressing table, command-verified results per requirement, and complete router/switch configurations in the appendix. *(Detailed per-test outputs live in the group report, which is not part of this worksheet.)*

## Key Learning Outcomes

* Designing a complete enterprise addressing plan (VLSM at scale — 16k and 4k host blocks)
* Combining VLAN segmentation, EIGRP routing, and ACL policy into one coherent architecture
* Centralized DHCP with relay agents across routed boundaries
* Team-based delivery: documentation, verification evidence, and presentation

## Skills Demonstrated

* Enterprise network design (multi-site, HQ/branch)
* VLSM subnetting at scale
* VLAN + 802.1Q + inter-VLAN routing
* EIGRP configuration
* ACL security policy design
* DHCP relay (`ip helper-address`)
* Web/DNS/Mail service deployment
* Technical documentation and team collaboration

## Portfolio Relevance

This is the closest coursework analogue to a real enterprise deployment: requirements-driven design, security policy translated into ACLs, multi-site routing, and formal documentation with verification evidence. It demonstrates the ability to integrate individual skills into a working system — precisely what internship projects demand.
