# Overview

Wireshark is a network packet analyzer that captures and
inspects every packet flowing in and out of your device.
It provides complete visibility into network traffic in real time.

> "If it travels through your network — Wireshark can see it"

---

## How Wireshark Works

- Wireshark captures packets using libpcap (Linux/Mac)
  or Npcap (Windows) — its own built in capture library.

### Promiscuous Mode

- Normal mode:
  - captures only packets meant for YOUR device

- Promiscuous mode:
  - captures ALL packets on the network
  - even ones not meant for you
  - Wireshark enables this automatically
  - how attackers sniff network traffic

---

## Three Panels

| Panel              | Location | Purpose                                             |
| ------------------ | -------- | --------------------------------------------------- |
| **Packet List**    | Top      | Every captured packet, one row per packet           |
| **Packet Details** | Middle   | Breakdown of selected packet, every OSI layer shown |
| **Packet Bytes**   | Bottom   | Raw hex data, actual bytes of packet                |

---

## Display Filters

Filters are the most important skill in Wireshark.
Without filters = overwhelming.
With filters = surgical precision.

| Filter                         | Purpose                                              |
| ------------------------------ | ---------------------------------------------------- |
| `dns`                          | Show only DNS traffic — reveals resolver and queries |
| `http`                         | Show only HTTP traffic — requests and responses      |
| `arp`                          | Show ARP traffic — broadcasts, Gratuitous ARP        |
| `tcp.port == 80`               | HTTP traffic — unencrypted, fully readable           |
| `tcp.port == 443`              | HTTPS traffic — TLS encrypted                        |
| `http.response.code == 200`    | Successful responses only                            |
| `http.response.code == 404`    | Not found responses                                  |
| `http.request.method == "GET"` | GET requests only                                    |
| `tcp.flags.syn == 1`           | SYN packets only (1=true, 0=false)                   |
| `ip.addr == x.x.x.x`           | Traffic to/from specific IP                          |

---

## What I Saw in Real Traffic

### DNS Traffic

- Router acting as DNS resolver
- Standard queries going out
- Standard responses coming back
- A records (IPv4) and AAAA records (IPv6)
- CNAME records in responses
- Firefox prefetching DNS for frequent sites

### HTTP Traffic

- Complete HTTP request headers:
  Host, User-Agent, Accept,
  Accept-Encoding, Cookie, Connection

- Complete HTTP response headers:
  Server version, Content-Type,
  Content-Length, Content-Encoding

- Followed TCP Stream:
  - RED = request (client)
  - BLUE = response (server)
  - Body = gzip compressed (gibberish)

### ARP Traffic

- Broadcast ARP requests:
  "Who has this IP, tell this IP"

- Gratuitous ARP captured live:
  - Opcode: 2 (reply)
  - Sender IP = Target IP (same!)
  - Destination: broadcast ff:ff:ff:ff:ff:ff
  - Is Gratuitous: true (Wireshark confirmed)

### HTTPS Traffic (port 443)

```bash
→ TLSv1.3 encryption
→ Application Data = encrypted payload
→ Everything unreadable
→ Only metadata visible:
IP addresses, packet sizes, timing
```

### Background Traffic

```bash
→ Pop!OS checking for updates
regular 204 responses
"no content = no updates"
happens at intervals automatically
→ This pattern = same as malware C2
regular check-ins to remote server
```

### Gzip vs TLS — Key Difference

**Gzip compression:**

- headers still readable
- only body is gibberish
- HTTP sites (port 80)
- not encryption, just compression

**TLS encryption:**

- everything encrypted
- headers and body unreadable
- HTTPS sites (port 443)
- real encryption

---

## TCP Stream Following

- Right click any packet
  → Follow → TCP Stream

- Shows complete conversation:
  - RED = client (your requests)
  - BLUE = server (responses)

**HTTP stream** = readable headers
gzip body

**HTTPS stream** = all gibberish
TLS encrypted

---

## Cybersecurity Use

### Attacker Uses

- Sniff HTTP credentials on public WiFi
- Steal session cookies
- Capture login forms (POST requests)
- ARP spoofing → MitM → capture all traffic
- Detect what services are running
- Analyze malware traffic patterns

### Defender Uses

- Detect suspicious outbound connections
- Find unknown IPs receiving data
- Identify C2 communication patterns
  (regular interval check-ins)
- Detect ARP spoofing attacks
- Analyze malware behavior
- Investigate security incidents
- Read PCAP files in HTB challenges

### Public WiFi Danger

```

Attacker on same WiFi network:
↓
Runs ARP spoofing (Ettercap/Bettercap)
↓
Becomes Man in the Middle
↓
Runs Wireshark
↓
Captures ALL HTTP traffic
↓
Steals cookies → session hijacking
Reads credentials → account takeover
Makes victim scapegoat

```

### Protection:

- Always use HTTPS sites
- Use VPN on public WiFi
- Never login on HTTP sites
- Check for HTTPS padlock
- HTTPS = encrypted even if MitM

## One Line Summary

**Wireshark** = window into every packet
traveling through your network

**HTTP** = fully readable, dangerous on public WiFi

**HTTPS** = encrypted, safe even if captured

**ARP** = see devices discovering each other

**DNS** = see every domain your device queries

**Filters** = what makes Wireshark useful not overwhelming

```

```
