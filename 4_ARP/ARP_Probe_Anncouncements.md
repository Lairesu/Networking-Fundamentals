# ARP Probe and ARP Announcement

ARP Probe and ARP Announcement work together as a two-step process
that a device performs before claiming an IP address on a network.
They are primarily used to detect IP address conflicts before they cause problems.

---

## ARP Probe

### What it is

A check sent by a new device before claiming an IP address.
The device is essentially asking:

> "Is anyone already using this IP address?"

### Packet Characteristics

| Field       | Value                         |
| ----------- | ----------------------------- |
| Opcode      | 1 (ARP Request)               |
| Destination | Broadcast (ff:ff:ff:ff:ff:ff) |
| Sender IP   | 0.0.0.0 (not yet claimed)     |
| Target IP   | IP address being tested       |

> Sender IP is 0.0.0.0 because the device has not officially claimed
> the IP yet — it is just peeking.

### Possible Outcomes

| Response        | Meaning                                            |
| --------------- | -------------------------------------------------- |
| No reply        | IP is free — safe to claim                         |
| Someone replies | IP conflict detected — device will not use this IP |

---

## ARP Announcement

### What it is

After a successful probe with no conflicts detected, the device
sends an ARP Announcement to officially claim the IP address.

> "I checked — nobody had it. This IP is now mine."

### Packet Characteristics

- Same structure as Gratuitous ARP
- Opcode: 2 (ARP Reply)
- Sender IP = Target IP (own IP address)
- Sent as broadcast to inform all hosts

---

## The Full Flow

```
New device joins network
        ↓
ARP Probe — "Is this IP taken?" (Sender IP = 0.0.0.0)
        ↓
No reply → ARP Announcement — "This IP is mine now"
        ↓
Device officially joins, all hosts aware of new mapping

Conflict detected → Device does not claim IP
```

---

## Security Relevance

If an attacker attempts to use a duplicate IP already active on the network:

- The legitimate owner may reply to the probe
- Conflict is detected — attacker's presence can be revealed
- This is a passive way networks can expose rogue or
  misconfigured devices

---

## Summary

ARP Probe is a pre-claim check — a device peeks at the network
before taking an IP. ARP Announcement is the official claim after
confirming no conflict exists. Together they protect networks from
accidental or malicious IP duplication.

---
