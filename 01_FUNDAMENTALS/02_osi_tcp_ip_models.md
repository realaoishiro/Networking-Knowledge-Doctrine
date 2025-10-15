# OSI & TCP/IP Models: Fundamental Notes

## OSI Model (Open Systems Interconnection)

The OSI model is a conceptual framework that standardizes the functions of a communication system into seven distinct layers.

| Layer | Name              | Function                                      | Example Protocols      |
|-------|-------------------|-----------------------------------------------|------------------------|
| 7     | Application       | User interface, network services              | HTTP, FTP, SMTP        |
| 6     | Presentation      | Data translation, encryption, compression     | SSL, TLS, JPEG         |
| 5     | Session           | Session management, dialog control            | NetBIOS, RPC           |
| 4     | Transport         | Reliable data transfer, flow control          | TCP, UDP               |
| 3     | Network           | Logical addressing, routing                   | IP, ICMP, IPsec        |
| 2     | Data Link         | Physical addressing, error detection/correction| Ethernet, PPP, Switch  |
| 1     | Physical          | Transmission of raw bits over medium          | Cables, Hubs, Fiber    |

**Key Points:**
- Each layer serves the layer above and is served by the layer below.
- Promotes interoperability and modular engineering.

---

## TCP/IP Model

The TCP/IP model is a practical framework used for real-world networking, especially the Internet. It has four layers:

| Layer         | Corresponds to OSI Layers | Function                        | Example Protocols      |
|---------------|--------------------------|---------------------------------|------------------------|
| Application   | 7, 6, 5                  | Network process to application  | HTTP, FTP, DNS, SMTP   |
| Transport     | 4                        | End-to-end communication        | TCP, UDP               |
| Internet      | 3                        | Logical addressing, routing     | IP, ICMP, ARP          |
| Network Access| 2, 1                     | Physical transmission           | Ethernet, Wi-Fi        |

**Key Points:**
- More streamlined than OSI; designed for implementation.
- Foundation of the Internet protocol suite.

---

## OSI vs TCP/IP

| Aspect         | OSI Model         | TCP/IP Model      |
|----------------|-------------------|-------------------|
| Layers         | 7                 | 4                 |
| Development    | Theoretical       | Practical         |
| Protocols      | General reference | Protocol-specific |
| Adoption       | Rare in practice  | Widely used       |

---

## Summary Table

| OSI Layer      | TCP/IP Layer      | Example Protocols      |
|----------------|-------------------|------------------------|
| Application    | Application       | HTTP, FTP, SMTP        |
| Presentation   | Application       | SSL, TLS               |
| Session        | Application       | NetBIOS, RPC           |
| Transport      | Transport         | TCP, UDP               |
| Network        | Internet          | IP, ICMP               |
| Data Link      | Network Access    | Ethernet, PPP          |
| Physical       | Network Access    | Cables, Hubs           |

---

## Key Takeaways

- **OSI**: Conceptual, 7 layers, educational reference.
- **TCP/IP**: Practical, 4 layers, basis of the Internet.
- Both models help understand and troubleshoot network communications.
