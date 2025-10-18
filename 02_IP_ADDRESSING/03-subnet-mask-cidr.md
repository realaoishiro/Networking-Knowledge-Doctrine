# Subnet Mask and CIDR

## Overview
Concise reference for IPv4 subnetting and Classless Inter-Domain Routing (CIDR). Covers concepts, calculations, common masks, subnetting steps, examples, and special cases.

---

## Key definitions
- IP address: 32-bit value (IPv4) usually shown as dotted decimal (a.b.c.d).
- Subnet mask: 32-bit mask that divides address into network and host portions. Shown as dotted decimal (e.g., 255.255.255.0) or prefix length (/24).
- CIDR (Classless Inter-Domain Routing): notation that uses prefix length to indicate network bits: a.b.c.d/n.
- Network address: all host bits = 0.
- Broadcast address: all host bits = 1 (for that subnet).
- Usable hosts: host addresses between network and broadcast addresses (except special cases).

---

## Binary fundamentals
- Each octet = 8 bits. Mask bits set to 1 = network; 0 = host.
- Convert mask or prefix to binary to find boundary and calculate ranges.

Example: /26
- Binary mask: 11111111.11111111.11111111.11000000
- Dotted decimal: 255.255.255.192
- Host bits = 6 (32 - 26)

---

## Basic formulas
- Host bits = 32 - prefix
- Number of addresses = 2^(host bits)
- Usable hosts (traditional) = 2^(host bits) - 2 (network + broadcast)
- Number of subnets when borrowing b bits = 2^b (relative to the original network)

Note: /31 and /32 are special (see below).

---

## Common prefixes / masks (quick reference)
- /8  = 255.0.0.0         — 16,777,214 usable hosts
- /16 = 255.255.0.0       — 65,534 usable hosts
- /24 = 255.255.255.0     — 254 usable hosts
- /25 = 255.255.255.128   — 126 usable hosts
- /26 = 255.255.255.192   — 62 usable hosts
- /27 = 255.255.255.224   — 30 usable hosts
- /28 = 255.255.255.240   — 14 usable hosts
- /29 = 255.255.255.248   — 6 usable hosts
- /30 = 255.255.255.252   — 2 usable hosts (classic P2P)
- /31 = 255.255.255.254   — 2 addresses, used for point-to-point (RFC 3021), no broadcast
- /32 = 255.255.255.255   — single host route

---

## Wildcard mask
- Wildcard mask = bitwise inverse of subnet mask.
- Example: 255.255.255.0 → wildcard 0.0.0.255
- Used in ACLs and route summarization tools.

---

## Subnetting steps (practical)
1. Determine network prefix (given or classless).
2. Decide desired subnet size (target prefix).
3. Calculate borrowed bits = target_prefix - original_prefix.
4. Calculate subnet increment = 2^(host bits in last octet position).
    - Or compute subnet block size from mask octet.
5. List subnets by adding increments to the appropriate octet.
6. For each subnet calculate:
    - Network address (first address)
    - First usable (network + 1) — unless /31 or /32
    - Last usable (broadcast - 1)
    - Broadcast address (all host bits = 1)

---

## Example 1 — Subnet a /24 into /26
Given: 192.168.1.0/24 → split into /26 subnets.
- Host bits in /26 = 6 → 2^6 = 64 addresses per subnet (62 usable).
- Subnets (increment 64 in last octet):
  - 192.168.1.0/26 → net: .0, usable: .1–.62, broadcast: .63
  - 192.168.1.64/26 → net: .64, usable: .65–.126, broadcast: .127
  - 192.168.1.128/26 → net: .128, usable: .129–.190, broadcast: .191
  - 192.168.1.192/26 → net: .192, usable: .193–.254, broadcast: .255

---

## Example 2 — Determine prefix and hosts
Given mask 255.255.248.0:
- Binary: 11111111.11111111.11111000.00000000
- Prefix = 21 (16 + 5)
- Host bits = 11 → addresses = 2048 → usable = 2046

---

## VLSM (Variable Length Subnet Mask)
- Allocate subnets of different sizes within the same major network.
- Process: satisfy largest subnet requirement first, allocate progressively smaller subnets from remaining address space.
- Reduces waste vs fixed-size subnetting.

---

## Supernetting / Route summarization
- Combine contiguous networks by reducing prefix length where possible.
- Summarize only where networks are contiguous and binary network prefixes match in high-order bits.
- Use binary alignment to find common prefix.

---

## Special cases and best practices
- /31: valid for point-to-point links (RFC 3021) — no broadcast, both addresses usable.
- /32: identifies a single host route.
- Avoid using network or broadcast addresses as hosts unless protocol allows.
- Plan addressing hierarchically to ease summarization and routing.
- Document subnets, purpose, gateway, and VLAN associations.

---

## Quick checklist when subnetting
- Verify required hosts per subnet (+growth).
- Choose smallest prefix that meets hosts.
- Calculate range and record network/broadcast/gateway.
- Reserve space for management, future expansion, and summarization.

---