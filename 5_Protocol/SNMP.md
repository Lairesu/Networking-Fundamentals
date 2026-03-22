# SNMP — Simple Network Management Protocol

---

## Overview

SNMP is a protocol used to monitor and manage network devices.
It monitors device health, performance, sensors, uptime,
interfaces and more.

Common monitored devices:

- Routers
- Switches
- Servers
- Printers
- Any network connected device

---

## SNMP Architecture

Three main components:

| Component        | Role                                             |
| ---------------- | ------------------------------------------------ |
| **SNMP Manager** | The monitoring system — checks on all devices    |
| **SNMP Agent**   | Runs on each monitored device — reports its data |
| **MIB**          | Database of everything the agent can report      |

```
SNMP Manager → "How are you doing router?"
SNMP Agent   → checks MIB → "Here is my status"
```

### SNMP Ports

| Port        | Purpose                           |
| ----------- | --------------------------------- |
| **UDP 161** | SNMP queries — manager asks agent |
| **UDP 162** | SNMP traps — agent alerts manager |

### SNMP Trap

Normal SNMP = Manager asks Agent for data
SNMP Trap = Agent alerts Manager without being asked

```
"Something just went wrong — here is the alert"
```

---

## OID and MIB

### OID — Object Identifier

Every piece of information on a device has a unique OID.
Examples of objects:

- Device name
- Uptime
- Interfaces
- Routing table
- CPU usage

OIDs look similar to IP addresses:

```
1.3.6.1.2.1.1.1.0 → System Description
1.3.6.1.2.1.1.3.0 → System Uptime
```

### MIB — Management Information Base

A database that stores all OIDs in a tree structure.

```
Root
├── 1.3 (identified organization)
│   └── 1.3.6 (DoD)
│       └── 1.3.6.1 (internet)
│           └── 1.3.6.1.2.1 (MIB-2)
│               ├── System
│               ├── Interfaces
│               └── Routing Table
```

---

## SNMP Versions

| Version | Bits   | Security                                    | Notes                              |
| ------- | ------ | ------------------------------------------- | ---------------------------------- |
| **v1**  | 32 bit | Community strings, plaintext                | Original version, very insecure    |
| **v2c** | 64 bit | Community strings, plaintext                | Better performance, still insecure |
| **v3**  | 64 bit | Encryption + Authentication + User controls | Current secure standard            |

> The c in v2c stands for community.
> v1 32 bit counters will overflow in 2038 — similar to Y2K problem.
> Finding v1 or v2c on a target = major vulnerability.

---

## Community Strings

Community strings are essentially passwords to access SNMP data.

| String      | Access       | Default Value |
| ----------- | ------------ | ------------- |
| **public**  | READ only    | public        |
| **private** | READ + WRITE | private       |

> Default community strings almost never changed = massive vulnerability.
> Write access = can modify device configuration remotely.
> v3 replaced community strings with proper user based authentication.

---

## SNMP Versions Security Comparison

```
v1  = plaintext community strings, 32 bit = very insecure
v2c = plaintext community strings, 64 bit = still insecure
v3  = encrypted OIDs + MIB, user based controls = secure
```

---

## Security Relevance

### Attacks and Misconfigurations

| Topic                         | Security Relevance                                             |
| ----------------------------- | -------------------------------------------------------------- |
| **Default community strings** | public/private unchanged = full device access                  |
| **v1/v2c plaintext**          | Community strings captured easily with Wireshark               |
| **Write access**              | Modify device configuration remotely                           |
| **Information disclosure**    | Dumps usernames, interfaces, routing tables, software versions |
| **SNMP enumeration**          | Tool snmpwalk dumps everything from a device                   |

### Most Common HTB Scenario

```
Nmap UDP scan shows port 161 open
        ↓
Try default community string:
snmpwalk -c public -v2c <target IP>
        ↓
Dumps all device information
        ↓
Find usernames, system info, running processes
        ↓
Use that info to pivot further
```

### Capturing SNMP Traffic

```
v1 or v2c in use
        ↓
Run Wireshark on network
        ↓
Filter for UDP port 161
        ↓
Community strings visible in plaintext
```

---

## One Line Summary

```
SNMP     = monitor and manage network devices
OID      = unique identifier for each piece of device data
MIB      = tree database of all OIDs
v1/v2c   = insecure, plaintext community strings
v3       = encrypted, user based, secure
public   = default read string = always try first in HTB
```
