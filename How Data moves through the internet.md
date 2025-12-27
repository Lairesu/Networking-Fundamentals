# Packet Traveling  
## Final Understanding — Network Fundamentals

This document summarizes how data (packets) travel through a network, combining switching, routing, and protocol behavior.

---

## How Hosts Communicate

- Within the same network (LAN)  
  → Communication happens using switches  
  → This process is called switching

- Across different networks  
  → Communication happens using routers  
  → This process is called routing

---

## Core Tables Used in Networking

Data forwarding depends on three important tables:

### 1. MAC Address Table (Switch)
- Maps MAC address to switch port
- Used only by switches
- Built dynamically by observing source MAC addresses

---

### 2. ARP Table / Cache
- Maps IP address to MAC address
- Used by hosts and routers
- Populated dynamically using ARP requests and responses
- Works only inside a local network (broadcast domain)

---

### 3. Routing Table
- Maps destination network to next hop or interface
- Used by hosts and routers
- Must exist before traffic is forwarded
- If no matching route exists, the packet is dropped

---

## Network Scenario

### Hosts and Networks

| Host | Network | IP Address |
|----|--------|-----------|
| A | 11.8.8.0 /24 | 11.8.8.11 |
| B | 22.7.8.0 /24 | 22.7.8.22 |
| C | 33.5.3.0 /24 | 33.5.3.33 |

Each network has one router with `.1` as its IP address.

---

### MAC Addresses

| Device | MAC Address |
|-----|-------------|
| Host A | a1a1 |
| Host B | b2b2 |
| Host C | c3c3 |
| Router A | aaa1 |
| Router B | aaa2 |
| Router C | aaa3 |

---

### Switch (Host A Network)
- Port 4 → Host A
- Port 5 → Router A
- Uses only a MAC address table

---

## Tables per Device

| Device | Tables Used |
|-----|-------------|
| Switch | MAC Address Table |
| Router | ARP Table, Routing Table |
| Host | ARP Table, Routing Table |

---

## Important Rules

- MAC and ARP tables are dynamic
- Routing tables must already exist
- Routers do not forward ARP broadcasts
- Layer 2 headers change at every hop
- Layer 3 IP addresses stay end-to-end

---

## Routing Tables

### Host Routing Tables
| Host   | Destination | Next Hop |
|--------|-------------|----------|
| Host A | 0.0.0.0 / 0 | 11.8.8.1 |
| Host B | 0.0.0.0 / 0 | 22.7.8.1 |
| Host C | 0.0.0.0 / 0 | 33.5.3.1 |


The default route points to the local router.

---

### Router Routing Tables
### Router A
| Destination     | Next Hop             |
|-----------------|----------------------|
| 11.8.8.0 / 24   | Directly Connected   |
| 0.0.0.0 / 0     | Internet             |

### Router B
| Destination     | Next Hop             |
|-----------------|----------------------|
| 22.7.8.0 / 24   | Directly Connected   |
| 0.0.0.0 / 0     | Internet             |

### Router C
| Destination     | Next Hop             |
|-----------------|----------------------|
| 33.5.3.0 / 24   | Directly Connected   |
| 0.0.0.0 / 0     | Internet             |



---

## Packet Flow: Host A to Host B

### 1. Layer 3 Decision (Host A)
- Host A compares destination IP with its subnet mask
- Host B is not in the same network
- Packet must be sent to the default gateway (Router A)

---

### 2. ARP Resolution (Layer 2)
- Host A does not know Router A’s MAC address
- Host A sends an ARP broadcast:  
Who has 11.8.8.1?
- Router A replies with its MAC address
- ARP table and switch MAC table are updated

---

### 3. Switching to Router A
- Frame sent:
- Source MAC: a1a1
- Destination MAC: aaa1
- Switch forwards the frame to Router A

---

### 4. Routing Across Networks
- Router A removes the Layer 2 header
- Examines the destination IP (Layer 3)
- Uses routing table to forward the packet toward Router B
- Packet hops router-to-router across the Internet

---

### 5. Delivery in Host B Network
- Router B sees the destination network is directly connected
- Router B sends an ARP request for Host B
- Host B replies with its MAC address
- Router B sends the frame to Host B

---

### 6. Host B Receives Data
- Host B removes the Layer 2 header
- Confirms the Layer 3 destination IP
- Data is passed to upper layers
- Application processes the bits (1s and 0s)

---

## Return Traffic
- ARP and MAC tables are already populated
- Response traffic is faster
- No broadcasts are needed unless entries expire

---

## Final Takeaways

- Switching uses MAC addresses
- Routing uses IP addresses
- ARP connects Layer 3 to Layer 2
- Routing tables guide packet direction
- Layer 2 changes at every hop
- Layer 3 stays end-to-end

This is how packets travel through a network.

