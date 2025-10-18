# Subnetting
## Quick reference: formulas & masks
- Prefix to mask: /N -> mask with N ones. Example: /26 -> 255.255.255.192
- Hosts per subnet: usable = 2^(32 - N) - 2  (except /31 and /32 special cases)
- Subnet increment (in octet containing host bits) = 256 - value_of_mask_octet
    - Example: /26 mask = 255.255.255.192 -> increment = 256 - 192 = 64
- Network address: set host bits to 0
- Broadcast address: set host bits to 1
- First usable = network + 1, Last usable = broadcast - 1

---

## Step-by-step subnetting method
1. Determine required number of subnets or hosts per subnet.
2. Choose a base prefix that can accommodate the largest requirement.
3. If fixed-size subnets: calculate new prefix (borrow bits) until 2^borrowed >= required subnets OR 2^(host_bits - borrowed) - 2 >= required hosts.
4. Compute increments and enumerate subnets (network/broadcast/usable).
5. Assign ranges and document (IP plan).
6. Verify with tools or CLI commands (ping, show ip route, show ip interface brief).

---

## Binary example (/24 -> /26)
Base: 192.168.10.0/24
- Need 4 subnets -> borrow 2 bits: /24 -> /26
- /26 mask = 255.255.255.192, increment 64
Subnets:
- 192.168.10.0/26  (usable .1 - .62, broadcast .63)
- 192.168.10.64/26 (usable .65 - .126, broadcast .127)
- 192.168.10.128/26 (usable .129 - .190, broadcast .191)
- 192.168.10.192/26 (usable .193 - .254, broadcast .255)

---

## VLSM example (practical planning)
Requirement (one major example):
- Subnet A: 100 hosts
- Subnet B: 50 hosts
- Subnet C: 10 hosts
- Point-to-point links: multiple /30

Start with 10.1.0.0/24:
1. Largest first: 100 hosts -> need /25? /25 = 126 usable -> assign 10.1.0.0/25 (0–127)
2. Next 50 hosts -> need /26 (62 usable) -> assign 10.1.0.128/26 (128–191)
3. Next 10 hosts -> need /28 (14 usable) -> assign 10.1.0.192/28 (192–207)
4. Point-to-point -> use /30 from remaining space (e.g., 10.1.0.208/30 -> .208/.209 usable)

Always allocate largest networks first to prevent fragmentation.

---

## Packet Tracer Lab 1 — Basic multi-subnet with a single router
Goal: Create two subnets from 192.168.10.0/24 as /26 and test connectivity via Router.

Topology:
- Router R1 G0/0 -> Switch -> PCs in Subnet A (192.168.10.0/26)
- Router R1 G0/1 -> Switch -> PCs in Subnet B (192.168.10.64/26)

Commands (Router CLI):
interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.192
 no shutdown
!
interface GigabitEthernet0/1
 ip address 192.168.10.65 255.255.255.192
 no shutdown

PC configuration (example for PC1 in Subnet A):
IP: 192.168.10.10
Mask: 255.255.255.192
Gateway: 192.168.10.1

PC in Subnet B:
IP: 192.168.10.70
Mask: 255.255.255.192
Gateway: 192.168.10.65

Verification:
- From PCs: ping gateway, ping other-subnet host (should succeed via router)
- Router: show ip interface brief, show ip route

Notes:
- If using one physical router interface and VLANs on switch, use subinterfaces (router-on-a-stick):
interface G0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.192
interface G0/0.20
 encapsulation dot1Q 20
 ip address 192.168.10.65 255.255.255.192

---

## Packet Tracer Lab 2 — VLSM + Point-to-point links + OSPF
Goal: Multi-branch topology with efficient address usage and dynamic routing.

Plan:
- HQ LAN: 10.0.0.0/24 subdivided via VLSM for departments
- Branch1-LAN: 10.0.1.0/24
- Use /30 between routers for WAN links
- Run OSPF to auto-exchange routes

Sample commands (Router R1 — HQ):
hostname HQ
interface G0/0
 description LAN-HQ
 ip address 10.0.0.1 255.255.255.128   ! /25 for large LAN
 no shutdown
interface Serial0/0/0
 ip address 192.168.100.1 255.255.255.252   ! /30 to Branch1
 no shutdown

Router Branch1:
interface Serial0/0/0
 ip address 192.168.100.2 255.255.255.252
 no shutdown
interface G0/0
 ip address 10.0.1.1 255.255.255.0
 no shutdown

OSPF (on each router):
router ospf 1
 network 10.0.0.0 0.0.255.255 area 0
 network 192.168.100.0 0.0.0.3 area 0

Verify:
- show ip protocols
- show ip ospf neighbor
- show ip route
- ping across branches

---

## Useful CLI verification commands
- show ip interface brief
- show ip route
- show running-config
- show ip ospf neighbor (if OSPF)
- debug ip packet (use carefully in labs)
- ping, traceroute from devices

---

## Practice exercises
1. Subnet 172.16.0.0/20 into subnets that support 500 hosts each. List first three networks.
2. Design a network for 5 branches. Each branch: 120 hosts. Use VLSM starting from 10.10.0.0/16. Show assignments.
3. Build Packet Tracer: 3 routers in hub-and-spoke. Use /30 for spokes. Configure OSPF and verify inter-branch reachability.

---

## Tips
- Always reserve addresses for routers, servers, NAT devices and documentation.
- Avoid overlapping subnets; double-check base network and mask.
- /31 can be used for point-to-point links in modern IOS (RFC 3021) — reduces waste.
- Use VLSM for efficient IP utilization; plan from largest to smallest.
- In Packet Tracer, remember to connect correct cable types (crossover auto-MDIX often handles this).

---
