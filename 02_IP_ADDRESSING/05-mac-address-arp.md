# MAC Address and ARP
## MAC address (Layer 2)
- Format: 48-bit (6 bytes) usually shown as 00:11:22:33:44:55 or 0011.2233.4455.  
- Assignment: burned-in (NIC) or locally administered (changed via software).  
- Types:
    - Unicast: single destination (first octet LSB = 0).  
    - Multicast: group (first octet LSB = 1).  
    - Broadcast: FF:FF:FF:FF:FF:FF (all hosts on link).  
- Use: used by switching and Ethernet frames to deliver to a host on the same L2 segment.
---
## ARP (Address Resolution Protocol) — purpose and flow (IPv4)
- Purpose: map IPv4 address -> MAC address on a broadcast LAN so a host can encapsulate an IP packet in an Ethernet frame.
- Typical flow (same subnet):
    1. Host A needs MAC for Host B (IP known). Check local ARP cache.  
    2. If unknown: send ARP Request: broadcast (FF:FF:...); payload asks “Who has IP x.x.x.x? Tell A_IP / A_MAC”.  
    3. Host B sees request, replies with ARP Reply: unicast to A containing B_MAC.  
    4. A caches mapping (ARP table) and sends frames to B_MAC.
- ARP cache: dynamic entries (timeout, e.g., 4–20 min), static entries possible.
- Variants / notes:
    - Gratuitous ARP: host announces its IP->MAC (used for detection, proxy updates).  
    - Proxy ARP: router answers ARP for another IP to forward traffic.  
    - Security: ARP spoofing/poisoning is a common attack.
---
## How MAC and ARP interlink
- IP is Layer 3 identifier; MAC is Layer 2 identifier.  
- For local delivery, an IP packet must be placed into an Ethernet frame addressed to the destination MAC. ARP provides that mapping.  
- If destination IP is in another subnet, host sends to its default gateway MAC (gateway resolved via ARP); router then forwards at L3 and uses ARP on outgoing link.
---
## Cisco / Packet Tracer example (simple LAN)
Topology:
- PC1 (192.168.1.10/24) <--> Switch <--> PC2 (192.168.1.20/24)  
Optional: Router for gateway (192.168.1.1)

Steps:
1. Configure IPs on PCs:
     - PC1 IP: 192.168.1.10, mask 255.255.255.0, gateway 192.168.1.1 (if using router).  
     - PC2 IP: 192.168.1.20, mask 255.255.255.0, gateway 192.168.1.1.
2. In Packet Tracer, use Simulation mode to observe packets.
3. From PC1, ping PC2:
     - PC1 checks ARP cache; if missing, it broadcasts ARP Request.
     - PC2 replies with ARP Reply. Packet Tracer will show ARP Request (broadcast) and ARP Reply (unicast).
4. Verify tables:
     - On PC (Windows simulated): arp -a (or view in ARP table UI).
     - On Cisco switch: show mac address-table
     - On Cisco router: show ip arp
5. Example expected outputs:
     - show mac address-table
         - VLAN 1  10   00-11-22-33-44-55  Dynamic  Gi0/1
     - show ip arp
         - 192.168.1.10  00-11-22-33-44-55  ARPA  GigabitEthernet0/0

Packet Tracer tips:
- Use Simulation > Capture/Forward to step through ARP Request -> ARP Reply -> ICMP echo.  
- Filter for ARP or Ethernet type to reduce noise.
---
## Useful commands
- Windows:
    - ipconfig /all
    - arp -a
- Linux:
    - ip addr
    - ip neigh show
    - arp -n
- Cisco IOS:
    - show ip arp
    - show arp
    - show mac address-table
    - clear arp-cache        (clear router ARP table)
    - clear mac address-table dynamic
---
## Troubleshooting checklist
- Are IPs and masks correct (same subnet)?  
- Is default gateway set (if crossing subnets)?  
- Check ARP cache (is mapping present and correct?).  
- Use ping in Simulation to observe ARP exchange.  
- Look for duplicate IPs (gratuitous ARP can reveal).  
- On a switch, check MAC table aging or port security.
---
## Short example CLI (Cisco router + two PCs)
- Router:
    - interface GigabitEthernet0/0
        ip address 192.168.1.1 255.255.255.0
        no shutdown
- After PC1 pings PC2:
    - Router (or host) -> show ip arp
    - Switch -> show mac address-table

---
