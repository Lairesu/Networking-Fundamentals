| Feature                   | TCP (Transmission Control Protocol)             | UDP (User Datagram Protocol)           |
| ------------------------- | ----------------------------------------------- | -------------------------------------- |
| Connection                | Connection-oriented; uses a three-way handshake | Connectionless; no handshake           |
| Reliability               | Guarantees reliable data delivery               | Does not guarantee delivery            |
| Acknowledgements          | Uses acknowledgements (ACKs)                    | No acknowledgements                    |
| Retransmission            | Supports retransmission of lost packets         | No retransmission support              |
| Ordering                  | Ensures packets are delivered in order          | Does not ensure ordering               |
| Flow & Congestion Control | Provides flow control and congestion control    | No flow or congestion control          |
| Speed                     | Slower due to higher overhead                   | Faster with minimal overhead           |
| Header Size               | Variable header size (20–60 bytes)              | Fixed header size (8 bytes)            |
| Data Handling             | Treats data as a continuous byte stream         | Treats data as independent messages    |
| Broadcasting/Multicasting | Does not support broadcasting or multicasting   | Supports broadcasting and multicasting |
| Common Uses               | HTTP, HTTPS, FTP, SMTP                          | DNS, DHCP, VoIP, Streaming             |
