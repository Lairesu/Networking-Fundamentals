# DHCP — Dynamic Host Configuration Protocol

---

## Overview

DHCP is a protocol that automatically assigns network configuration
to devices when they join a network.

DHCP automatically assigns:

- IP Address
- Subnet Mask
- Default Gateway
- DNS Server addresses
- Lease Time

> Without DHCP every device would need manual network configuration.

---

## How DHCP Works

- Operates at **Application Layer (Layer 7)**
- Uses **UDP Port 67** (server) and **UDP Port 68** (client)
- Communication process is called **DORA**

---

## The DORA Process

### Sci-Fi Analogy

> A person joins a new world. The overseer senses the arrival and
> calls all robots (DHCP servers) to see who can help. Robots in
> range send offers with a communication device (IP), location
> (subnet), travel routes (gateway) and how long they can keep it
> (lease time). Person picks one offer, tells other robots to stand
> down, and receives the official gift box (ACK) with full network
> configuration.

---

### D — Discover

Device with no IP joins the network and broadcasts:

> "Is any DHCP server out there?"

- Sent as broadcast — device has no IP yet
- Looking for any available DHCP server

---

### O — Offer

DHCP server replies with an available IP and configuration:

- IP Address (carried in **yiaddr** — "your IP address" field)
- Subnet Mask
- Default Gateway
- DNS Server
- Lease Time
- Server Identifier — so client knows who sent the offer

> Can be broadcast or unicast depending on client/network behavior.
> Multiple servers may send offers — client picks one.

---

### R — Request

Client confirms chosen offer and officially requests that IP:

> "I accept this offer, make it official"

- Broadcast so other DHCP servers know their offers were not chosen
- Other servers release their reserved IPs back to their pool

---

### A — Acknowledge

Selected DHCP server sends final confirmation:

> "IP is officially yours — here is your full configuration"

- Officially assigns IP to the client
- Provides complete network configuration parameters

---

## Full DORA Flow

```
Device joins network
        ↓
DISCOVER → "Is any DHCP server out there?"
        ↓
OFFER    → "Here is an IP and configuration"
        ↓
            Client detects IP already in use?
            → DECLINE → back to DISCOVER
        ↓
REQUEST  → "I accept this offer, make it official"
        ↓
            IP outside scope or already allocated?
            → NACK → back to DISCOVER
        ↓
ACK      → "IP is officially yours + full config"

Device configured and connected
        ↓
When leaving → RELEASE → IP returned to pool
```

---

## Extended DHCP Messages

| Message     | Sent By | Purpose                                                                         |
| ----------- | ------- | ------------------------------------------------------------------------------- |
| **NACK**    | Server  | Rejects request — IP outside scope, already allocated, or client moved networks |
| **Decline** | Client  | Detected offered IP already in use on network                                   |
| **Release** | Client  | Returns IP back to server before lease expires                                  |
| **Inform**  | Client  | Client already has IP but needs other network settings                          |

### DHCP Release — Sci-Fi Analogy

> Person is permanently leaving the world.
> Returns communication device back to robot.
> Robot can now give it to the next person who joins.

### DHCP Inform — Sits Outside DORA

```
Device already has manually configured IP
        ↓
INFORM → "I have an IP but need other settings"
        ↓
ACK    → Server sends configuration only, no IP assigned
```

---

## Security Relevance

### Attacks

| Attack                | How it Works                                                                                                                                      |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rogue DHCP Server** | Attacker sets up fake DHCP server, responds to DISCOVER faster than real server, assigns fake gateway → all traffic flows through attacker = MitM |
| **DHCP Starvation**   | Attacker floods network with DISCOVER messages using fake MACs, exhausts IP pool → legitimate devices cannot get IP = DoS                         |

### Defense

| Defense           | How it Works                                                                     |
| ----------------- | -------------------------------------------------------------------------------- |
| **DHCP Snooping** | Switch only trusts DHCP responses from trusted ports — blocks rogue DHCP servers |

### Most Dangerous HTB/Real World Scenario

```
Attacker on network
        ↓
DHCP Starvation → exhausts real server IP pool
        ↓
Rogue DHCP Server spun up
        ↓
Assigns attacker as default gateway to all new devices
        ↓
All traffic flows through attacker = full MitM
```

---

## One Line Summary

```
DHCP   = automatic IP assignment when device joins network
DORA   = the 4 step process to get that IP
Rogue DHCP = attacker hijacks the process = MitM
```

---
