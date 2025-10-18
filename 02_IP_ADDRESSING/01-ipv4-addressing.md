# IPv4 Structure and Classes

## 1. Introduction to IPv4

IPv4 (Internet Protocol version 4) is the fourth version of the Internet Protocol (IP) and is one of the core protocols of standards-based internetworking methods in the Internet and other packet-switched networks. IPv4 uses a 32-bit address scheme allowing for a total of 4,294,967,296 (2³²) unique addresses.

---

## 2. IPv4 Address Structure

- **Format:** Dotted-decimal notation (e.g., 192.168.1.1)
- **Binary Representation:** 32 bits, divided into four 8-bit octets
- **Example:**
    - Decimal: `192.168.1.1`
    - Binary: `11000000.10101000.00000001.00000001`

### 2.1. Components

- **Network Portion:** Identifies the specific network.
- **Host Portion:** Identifies the specific device (host) on that network.

The division between network and host portions depends on the subnet mask or the address class.

---

## 3. IPv4 Address Classes

IPv4 addresses are divided into five classes (A, B, C, D, E) based on their leading bits and intended usage.

### 3.1. Class A

- **Range:** 1.0.0.0 to 126.255.255.255
- **Default Subnet Mask:** 255.0.0.0 (/8)
- **Leading Bits:** 0xxxxxxx
- **Number of Networks:** 128 (0 and 127 are reserved)
- **Hosts per Network:** ~16 million
- **Usage:** Very large networks (e.g., ISPs, large corporations)

### 3.2. Class B

- **Range:** 128.0.0.0 to 191.255.255.255
- **Default Subnet Mask:** 255.255.0.0 (/16)
- **Leading Bits:** 10xxxxxx
- **Number of Networks:** 16,384
- **Hosts per Network:** ~65,000
- **Usage:** Medium-sized networks (e.g., universities)

### 3.3. Class C

- **Range:** 192.0.0.0 to 223.255.255.255
- **Default Subnet Mask:** 255.255.255.0 (/24)
- **Leading Bits:** 110xxxxx
- **Number of Networks:** 2,097,152
- **Hosts per Network:** 254
- **Usage:** Small networks (e.g., small businesses)

### 3.4. Class D (Multicast)

- **Range:** 224.0.0.0 to 239.255.255.255
- **Leading Bits:** 1110xxxx
- **Usage:** Multicast groups (not for regular host addressing)

### 3.5. Class E (Experimental)

- **Range:** 240.0.0.0 to 255.255.255.255
- **Leading Bits:** 1111xxxx
- **Usage:** Reserved for experimental purposes

---

## 4. Special IPv4 Addresses

- **0.0.0.0:** Default route, "this host"
- **127.0.0.0/8:** Loopback addresses (localhost)
- **169.254.0.0/16:** APIPA (Automatic Private IP Addressing)
- **255.255.255.255:** Broadcast address

---

## 5. Private IPv4 Address Ranges

Used for internal networks, not routable on the public Internet.

| Class | Private Range           | Subnet Mask      |
|-------|------------------------|------------------|
| A     | 10.0.0.0 – 10.255.255.255 | 255.0.0.0      |
| B     | 172.16.0.0 – 172.31.255.255 | 255.240.0.0   |
| C     | 192.168.0.0 – 192.168.255.255 | 255.255.0.0 |

---

## 6. Subnetting

Subnetting divides a network into smaller sub-networks, improving routing efficiency and security. It uses a subnet mask to determine the network and host portions.

- **Subnet Mask Example:** 255.255.255.0 (/24)
- **CIDR Notation:** e.g., 192.168.1.0/24

---

## 7. Limitations of IPv4

- **Address Exhaustion:** Limited to ~4.3 billion addresses
- **Security:** No built-in encryption or authentication
- **Fragmentation:** Inefficient for modern, large-scale networks

---

## 8. Brief Overview of IPv6

IPv6 (Internet Protocol version 6) was developed to address IPv4 limitations.

- **Address Length:** 128 bits (vs. 32 bits in IPv4)
- **Address Format:** Hexadecimal, colon-separated (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334)
- **Number of Addresses:** 3.4 x 10³⁸ (virtually unlimited)
- **Features:**
    - Simplified header format
    - Built-in security (IPsec)
    - No need for NAT (Network Address Translation)
    - Improved multicast and mobility support

---

## 9. Summary Table

| Class | Address Range                | Default Mask      | Hosts per Network | Usage         |
|-------|------------------------------|-------------------|-------------------|--------------|
| A     | 1.0.0.0 – 126.255.255.255    | 255.0.0.0 (/8)    | ~16 million       | Large nets   |
| B     | 128.0.0.0 – 191.255.255.255  | 255.255.0.0 (/16) | ~65,000           | Medium nets  |
| C     | 192.0.0.0 – 223.255.255.255  | 255.255.255.0 (/24)| 254              | Small nets   |
| D     | 224.0.0.0 – 239.255.255.255  | N/A               | N/A               | Multicast    |
| E     | 240.0.0.0 – 255.255.255.255  | N/A               | N/A               | Experimental |

---

## 10. References

- [RFC 791 - Internet Protocol](https://datatracker.ietf.org/doc/html/rfc791)
- [RFC 2460 - IPv6 Specification](https://datatracker.ietf.org/doc/html/rfc2460)
- [IANA IPv4 Address Space Registry](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xhtml)
