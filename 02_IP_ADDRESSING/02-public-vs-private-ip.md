# Public vs Private IP — Guide (from the CNO perspective)

## Purpose
This document explains public and private IPv4 addressing, address classes and ranges, key differences, implementation guidance, and operational best practices for enterprise networks.

---

## Quick summary
- Public IPs are globally routable and assigned by ISPs or registries (RIPE/APNIC/ARIN).
- Private IPs (RFC 1918) are non-routable on the public Internet and used inside organizations.
- NAT/DHCP/VLANs and routing design are the core mechanisms to safely and efficiently use private addressing while accessing the Internet.

---

## IP fundamentals
- IPv4 address: 32 bits, usually written as dotted decimal (a.b.c.d).
- Network vs host bits determined by mask (CIDR notation: /n).
- Special/reserved:
    - 127.0.0.0/8 — loopback
    - 0.0.0.0 — default route / unspecified
    - Broadcast is highest host in a subnet.

---

## IPv4 Classes (historical)
Classful addressing is deprecated but useful for historical context:
- Class A: 1.0.0.0–126.255.255.255 — default mask /8
- Class B: 128.0.0.0–191.255.255.255 — /16
- Class C: 192.0.0.0–223.255.255.255 — /24
- Class D: 224.0.0.0–239.255.255.255 — multicast
- Class E: 240.0.0.0–255.255.255.255 — reserved

Prefer CIDR/subnetting in modern designs.

---

## RFC 1918 private ranges
- 10.0.0.0/8 (10.0.0.0–10.255.255.255) — large sites
- 172.16.0.0/12 (172.16.0.0–172.31.255.255) — medium sites
- 192.168.0.0/16 (192.168.0.0–192.168.255.255) — small/home networks

These are non-routable on the public Internet and safe for internal use.

---

## Public vs Private — key differences
- **Routability:** Public routable globally; private not routable on Internet.
- **Address assignment:** Public allocated by RIRs/ISPs; private assigned internally.
- **Security:** Private addresses provide an isolation layer but are not a security control by themselves.
- **Scale:** Private ranges allow unlimited internal use but can conflict across merged networks — require renumbering or translation.
- **Cost:** Public addresses may be scarce/costly; NAT reduces public address requirements.

---

## When to use which
- Private for internal hosts, VMs, IoT, management networks.
- Public for devices that must be directly reachable from the Internet (public-facing services) or when you require unique global addressing (BGP multihoming, peering).
- For cloud services, use provider-assigned public IPs or NAT gateways as appropriate.

---

## Addressing design best practices
1. Plan hierarchical addressing aligned to organization structure (region → site → building → floor → VLAN).
2. Use CIDR subnets sized for projected growth; avoid many /24s when /22 or /21 is more appropriate.
3. Use descriptive documentation and IPAM (IP Address Management) tools.
4. Avoid using overlapping private ranges across business units that will later connect — standardize on one or use plan for renumbering/translation.
5. Reserve ranges for infrastructure (management, network devices, jump hosts).
6. Use dedicated management VLANs and subnets; limit access via ACLs/Firewalls.

---

## NAT and translation
- Purpose: allow private hosts to communicate outbound using public IP(s).
- Types:
    - Static NAT (1:1) — maps a single private IP to a public IP.
    - Dynamic NAT — pool-based mapping.
    - PAT (NAT overload) — many-to-one using port translation (commonly used).
- Considerations:
    - PAT conserves public addresses but complicates inbound reachability.
    - Static NAT or public IP assignment required for Internet-facing services.
    - Log and monitor translations for troubleshooting.

Example Cisco IOS PAT (simple):
```interface GigabitEthernet0/0
 ip address x.x.x.x 255.255.255.248
 ip nat outside
!
interface GigabitEthernet0/1
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
!
ip nat inside source list 10 interface GigabitEthernet0/0 overload
access-list 10 permit 192.168.1.0 0.0.0.255
```
Static NAT example:
`ip nat inside source static tcp 192.168.1.10 80 x.x.x.100 80`

---

## DHCP and address allocation
- Use DHCP for dynamic hosts; reserve static IPs for servers and infrastructure.
- Example Cisco DHCP pool:
ip dhcp pool OFFICE
 network 10.10.10.0 255.255.255.0
 default-router 10.10.10.1
 dns-server 8.8.8.8 8.8.4.4
 excluded-address 10.10.10.1 10.10.10.10

- Use DHCP reservations (or static DHCP) for devices that need predictable addressing.

---

## Routing and Internet connectivity
- Single-ISP: default route to ISP; NAT for private hosts.
- Multi-homed enterprises: use public addressing or BGP with real public prefixes; consider provider-independent (PI) space if multihoming long-term.
- Ensure RPF, ACLs, anti-spoofing (uRPF, prefix-lists) to prevent source address spoofing.

---

## Security and operational controls
- Use perimeter firewalls for control of inbound/outbound traffic.
- Implement NAT + firewall policies; do not rely on private addressing for security.
- Use ACLs and segmentation (VLANs, VRFs) to limit lateral movement.
- Monitor NAT, address usage, and DNS to detect anomalies.
- Maintain IPAM and configuration backups for troubleshooting and auditing.

---

## IPv6 note
- IPv6 eliminates IPv4 scarcity and NAT; use ULA (unique local addresses) for internal addressing and global unicast for public.
- Plan dual-stack transition when migrating.

---

## Implementation checklist (practical)
1. Inventory current addressing and services.
2. Choose private range(s) and document plan in IPAM.
3. Design subnet sizes and VLAN mapping.
4. Configure core routing and VLANs.
5. Configure DHCP scopes and exclusions.
6. Configure NAT/PAT and any required static NAT for public services.
7. Implement firewall rules and ACLs for inter-subnet and Internet access.
8. Test connectivity, NAT translations, and service reachability.
9. Monitor (flow, NAT logs, DNS) and iterate.

---

## Troubleshooting tips
- Verify mask and gateway on endpoints.
- Confirm route to public IP (traceroute).
- Check NAT translations and logs (show ip nat translations).
- Validate ACLs and firewall logs for drops.
- Use packet captures when necessary between boundary interfaces.

---

## Recommended reading / RFCs
- RFC 1918 — Address Allocation for Private Internets
- RFC 3021 — /31 Subnet usage for point-to-point links
- Operational vendor docs for NAT and BGP best practices (use vendor-provided configuration guides)

---

