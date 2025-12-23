# Networking Protocols

## What is a Protocol?

A **protocol** is a standardized set of rules and message formats that define how data is **formatted, transmitted, received, and interpreted** between devices on a network.

Protocols operate at different layers of the network stack and work together to enable communication over the Internet.

---

## Address Resolution Protocol (ARP)

**ARP (Address Resolution Protocol)** resolves an **IP address to a MAC address** within a **local network (LAN)**.

- Used only inside the same subnet
- Not forwarded by routers
- Works using request/response messages
- Defined in **RFC 826**

**Example:**
Who has 192.168.1.1? Tell 192.168.1.10

---

## File Transfer Protocol (FTP)

**FTP** is used to transfer files between a client and a server.

- Application-layer protocol
- Runs on **TCP**
- Common commands:
  - `RETR` – download file
  - `STOR` – upload file
- Uses:
  - Port 21 (control)
  - Port 20 (data, classic FTP)

---

## Simple Mail Transfer Protocol (SMTP)

**SMTP** is used to **send emails** between mail clients and mail servers.

- Application-layer protocol
- Runs on **TCP**
- Common commands:
  - `HELO`
  - `MAIL FROM`
  - `RCPT TO`
- Common responses:
  - `250 OK`
- Uses TCP port **25**, **587**, or **465**

> Note: Email retrieval uses **POP3** or **IMAP**, not SMTP.

---

## Hypertext Transfer Protocol (HTTP)

**HTTP** is used for communication between web clients and web servers.

- Application-layer protocol
- Transfers many data types:
  - HTML
  - CSS
  - JavaScript
  - Images
  - JSON (APIs)
- Example:
  - Client request: `GET /index.html`
  - Server response: `200 OK`

---

## HTTPS, SSL, and TLS

- **TLS (Transport Layer Security)** encrypts communication between client and server.
- **SSL** is deprecated; TLS is used today.
- HTTPS = **HTTP over TLS**

TLS provides:

- Encryption
- Authentication (certificates)
- Data integrity

Uses TCP port **443**.

---

## Domain Name System (DNS)

**DNS** translates **domain names into IP addresses**.

- Humans use names, networks use IPs
- DNS does not transfer website data
- Uses:
  - UDP port 53 (mostly)
  - TCP port 53 (large responses)

**Example:**
mangadex.org → 104.21.12.34

---

## Protocol Flow When Visiting a Website

When visiting `https://mangadex.org`:

1. DNS resolves domain → IP
2. TCP establishes a connection to IP:443
3. TLS handshake creates a secure tunnel
4. HTTPS exchanges web content
5. IP, ARP, and Physical layers support delivery

---

## Network Configuration Requirements

For a host to access the Internet, it needs **four items**:

1. **IP Address** – host identity
2. **Subnet Mask** – defines local network size
3. **Default Gateway** – router IP for external networks
4. **DNS Server IP** – resolves domain names

**Example:**
IP: 9.1.3.5
Subnet Mask: 255.255.255.0 (/24)
Default Gateway: 9.1.1.1
DNS Server: 8.8.8.8

---

## Dynamic Host Configuration Protocol (DHCP)

**DHCP** automatically assigns network configuration to hosts.

- Provides: IP, Subnet Mask, Gateway, DNS
- Uses **UDP**
  - Server: port 67
  - Client: port 68
- Uses the **DORA** process:
  - Discover
  - Offer
  - Request
  - Acknowledge

DHCP runs **when joining a network**, not during every web request.

---

## Protocols by Layer

| Layer       | Protocols                   |
| ----------- | --------------------------- |
| Application | HTTP, HTTPS, FTP, SMTP, DNS |
| Transport   | TCP, UDP                    |
| Network     | IP                          |
| Data Link   | ARP, Ethernet               |
| Physical    | Bits & signals              |

---

## Key Takeaways

- DNS resolves names to IPs
- TCP connects using IP + port
- TLS secures the connection
- HTTP transfers application data
- DHCP automates network setup
- ARP enables local delivery

Networking works because **each protocol has a clear role** and operates within its layer.
