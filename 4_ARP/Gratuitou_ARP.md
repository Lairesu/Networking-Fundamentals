# Gratuitous ARP (Address Resolution Protocol)

Gratuitous ARP is an unsolicited ARP Reply sent without a prior ARP Request.
A device uses it to announce or update its own IP-to-MAC mapping to all
devices on the local network segment.

Unlike traditional ARP (where a node requests another node's MAC address)
and Proxy ARP (where a node answers on behalf of another), Gratuitous ARP
is a self-initiated broadcast — no one asked for it.

---

## Key Concept

A device sends a broadcast ARP Reply stating:

> "I am this IP address, and this is my MAC address."

This allows other devices to update their ARP tables without performing
a traditional ARP request-response cycle.

---

## Packet Characteristics

| Field       | Value                                |
| ----------- | ------------------------------------ |
| Opcode      | 2 (ARP Reply)                        |
| Destination | Broadcast (ff:ff:ff:ff:ff:ff)        |
| Sender IP   | Own IP address                       |
| Target IP   | Own IP address (same as Sender IP)   |
| Sender MAC  | Own MAC address                      |
| Target MAC  | Own MAC address (same as Sender MAC) |

> The Sender IP = Target IP is what makes this packet "gratuitous" —
> it is a self-announcement rather than a response to a real request.

---

## Use Cases

### 1. Updating ARP Tables After MAC Change

Sent when a device's MAC address changes so other hosts update
their cached mappings.

Common scenarios:

- User manually changes their MAC address
- Virtual machine migrates to a new physical host
- Virtualized environments where IP stays the same but MAC changes

Result: Other hosts update their ARP cache with the new IP-to-MAC mapping
without needing to go through the traditional ARP process.

---

### 2. Announcing Presence on the Network

When a new device joins the network, it may send a Gratuitous ARP to:

- Announce its IP-to-MAC mapping to the network
- Populate ARP caches on other hosts proactively
- Detect duplicate IP addresses (if another host replies, conflict exists)

**Important limitation:** There is no protocol mandate requiring hosts to
cache every Gratuitous ARP they receive. Because of this, the benefit of
presence announcement is limited — however, since the harm is also not
significant, the behavior is not discouraged.

---

### 3. Redundancy and Failover

Gratuitous ARP is critical in high-availability and redundant network designs.
This is the foundation of how FHRP protocols (HSRP, VRRP, GLBP) operate
under the hood.

#### Case A — Same IP, Different MAC (Redundant IP)

Two devices share a virtual IP address but each has its own MAC address.
Example: Two routers sharing a default gateway IP.

Failover process:

1. Active device fails
2. Standby device takes ownership of the shared IP
3. Standby device sends Gratuitous ARP broadcasting the new IP-to-MAC mapping
4. All hosts on the network update their ARP tables
5. Traffic continues flowing to the correct device seamlessly

> Note: This is not limited to routers — any redundant device can use
> this mechanism.

---

#### Case B — Same IP and Same MAC (Redundant IP + MAC)

Both devices share the same IP address AND the same MAC address.

Behavior on hosts:

- ARP table does NOT need updating — the mapping never changed

Why Gratuitous ARP is still sent:

- Switches maintain a MAC address table that maps MAC addresses to
  physical ports
- After failover, the shared MAC address is now coming from a
  different physical port
- Switches learn MAC-to-port mappings from the **source MAC** of
  received frames
- The Gratuitous ARP frame causes the switch to update its MAC
  address table to the correct port

Result: Traffic is forwarded out the correct port even though hosts
never needed to update their ARP tables.

---

## Security Implications

Gratuitous ARP is inherently trust-based — hosts accept and cache
unsolicited ARP replies without verification. This makes it a
well-known attack surface:

| Attack                       | Description                                             |
| ---------------------------- | ------------------------------------------------------- |
| **ARP Spoofing**             | Attacker sends fake Gratuitous ARP to poison ARP caches |
| **ARP Poisoning**            | Redirects traffic through attacker's machine            |
| **Man-in-the-Middle (MitM)** | Attacker intercepts traffic between two hosts           |

Common tools that abuse Gratuitous ARP:

- `arpspoof`
- `Ettercap`
- `Bettercap`

Defenses:

- Dynamic ARP Inspection (DAI) on managed switches
- Static ARP entries for critical hosts
- Network monitoring for unexpected Gratuitous ARP floods

---

## Important Notes

- Most devices update their ARP cache upon receiving Gratuitous ARP
- Acceptance behavior is not strictly enforced by protocol but
  widely supported
- Commonly seen in routers, servers, firewalls, and virtual environments

---

## Summary

Gratuitous ARP is an unsolicited ARP Reply used to announce or update
a device's IP-to-MAC mapping across the local network. It plays a key
role in keeping ARP tables accurate after MAC changes, announcing
device presence, and ensuring seamless failover in redundant setups.
Because hosts accept these replies without verification, Gratuitous ARP
is also a primary vector for ARP-based attacks like spoofing and
Man-in-the-Middle interception.

---

_Module 8 – ARP | Networking Fundamentals_
_Part of: Solidifying Networking for Cybersecurity_
