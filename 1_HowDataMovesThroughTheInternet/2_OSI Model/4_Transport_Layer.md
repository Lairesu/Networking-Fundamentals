# Transport Layer (Layer 4)

The **Transport Layer** in the OSI model provides *service-to-service delivery* — it ensures data sent from one device reaches the correct application or process on the destination device.

---

## Purpose

While the **Network Layer (Layer 3)** ensures end-to-end delivery between devices, the **Transport Layer** ensures that data reaches the **correct program or service** on those devices.

> Example: Just like a postal system delivers mail to a building (Network Layer), the Transport Layer ensures it reaches the right apartment (specific service or process).

---

## Addressing Scheme: Port Numbers

Every application that communicates over a network uses a **port number** as an address to distinguish itself from other applications.

- **Ports range from 0–65535**
- Two main protocols use ports: **TCP** and **UDP**

| Protocol | Range | Reliability | Connection | Common Uses |
|-----------|--------|--------------|-------------|--------------|
| **TCP (Transmission Control Protocol)** | 0–65535 |  Reliable (acknowledgments, retransmission, ordered) | Connection-oriented | Web browsing (HTTP/HTTPS), email, file transfer |
| **UDP (User Datagram Protocol)** | 0–65535 |  Fast (no acknowledgment, unordered) | Connectionless | Games, streaming, video calls |

### Port Ranges
- **0–1023** → Well-known ports (HTTP 80, HTTPS 443, SSH 22)
- **1024–49151** → Registered ports (used by common software)
- **49152–65535** → Ephemeral ports (temporary client-side ports)

---

## Example

Imagine your computer is running:

- A Slack chat server  
- A Minecraft game server  
- A web server  

Each service uses a unique port number:

| Application | IP Address | Port | Example |
|--------------|-------------|------|----------|
| Slack Server | 2.2.2.2 | 6667 | `2.2.2.2:6667` |
| Web Server | 4.4.4.4 | 80 | `4.4.4.4:80` |
| Game Server | 3.3.3.3 | 437 | `3.3.3.3:437` |

When clients connect to these services, each connection uses a **unique port pair**:
- Source: `1.1.1.1:9999`
- Destination: `2.2.2.2:80`

---

## TCP vs UDP Comparison

| Feature | TCP | UDP |
|----------|-----|-----|
| Reliability |  Reliable |  Unreliable |
| Ordering |  Ordered |  Not guaranteed |
| Speed |  Slower |  Faster |
| Connection | Requires handshake | No handshake |
| Use Case | Websites, email, file transfer | Games, streaming, calls |

---

## Encapsulation Reminder

When data is sent across the network, each layer adds its own *header*:

1. **Layer 2 (Data Link)** → MAC addresses (Hop-to-hop)
2. **Layer 3 (Network)** → IP addresses (End-to-end)
3. **Layer 4 (Transport)** → Port numbers (Service-to-service)

Together, these headers form the complete **packet** that travels across the network.

---

## Analogy: The Postal System

| OSI Layer | Function | Analogy |
|------------|-----------|----------|
| Layer 1 – Physical | Sends bits through cables or Wi-Fi | The road/postman |
| Layer 2 – Data Link | Hop-to-hop delivery | Local post office |
| Layer 3 – Network | End-to-end routing | National postal routing |
| Layer 4 – Transport | Service-to-service delivery | Apartment/office number |

---

**Summary:**  
The Transport Layer ensures the data reaches the *correct* process on the *correct* device, making network communication organized and reliable.
