---
# **Everything Routers Do to Facilitate Communication**

Routers play a crucial role in connecting multiple networks and ensuring data can travel from one network to another. Below is a detailed explanation of how routers work, what routing is, how routing tables operate, and the full step-by-step process of packet forwarding.
---

# **1. What Are Routers & What Is Routing?**

### **Routers**

- A router is a device that connects **multiple networks**.
- It has **an IP address and a MAC address for every network** it is connected to.
- It **forwards packets** that are _not_ destined for itself (unlike hosts, which drop unknown traffic).
- It maintains two important tables:

  - **ARP Table** (IP → MAC)
  - **Routing Table** (Destination Networks → Next Hop)

### **Routing**

Routing is the process of moving data **between networks**.

Switches = move traffic _within a network_
Routers = move traffic _between networks_

---

# **2. How Routing Tables Are Populated**

Routers learn about networks in **three ways**:

## **2.1 Directly Connected Routes (DC)**

- Added automatically when the router is configured with an IP address.
- Example:
  If a router has IPs:

  - `10.0.44.1/24`
  - `10.0.55.1/24`

- Then routing table will have:

  - `10.0.44.0/24 → directly connected`
  - `10.0.55.0/24 → directly connected`

## **2.2 Static Routes**

- Manually added by the administrator.
- Example:

  ```text
  ip route 10.0.66.0/24 10.0.55.2
  ```

- Meaning: “To reach the 10.0.66.x network, send traffic to 10.0.55.2.”

## **2.3 Dynamic Routes**

Routers automatically share networks using routing protocols:

- **RIP**
- **EIGRP**
- **OSPF**
- **BGP**
- **IS-IS**

In dynamic routing:

- R2 tells R1: _“I know how to reach these networks.”_
- R1 tells R2 the same.
- Both learn each other’s networks automatically.

---

# **3. ARP Table (Layer 3 → Layer 2 Mapping)**

Routers also maintain an **ARP cache**:

- Maps **IP address → MAC address**
- Starts empty
- Filled only when needed (on demand)
- Filled using **ARP Request → ARP Reply**

This is how routers build L2 headers for each hop.

---

# **4. Full Example: Communication From Host A → Host B Through Two Routers**

### **Network Diagram (simplified)**

```
Host A ---- R1 ---- R2 ---- Host B
44.x        55.x    66.x
```

Goal: **Host A (10.0.44.9) → Host B (10.0.66.7)**

---

# **5. Step-by-Step: What Happens When A Sends Data to B**

## **5.1 Host A Prepares the Packet**

- Host A knows B’s IP: `10.0.66.7`
- Creates **L3 header**:

  - Source IP = 10.0.44.9
  - Destination IP = 10.0.66.7

## **5.2 Host A Needs the L2 Header**

For Layer 2 (MAC addresses), A must send the frame to **its default gateway (R1)**.

But A does **NOT** know R1’s MAC yet → ARP needed:

### **A → ARP Request: “Who has 10.0.44.1?”**

- Broadcasted to entire 44.x network.

### **R1 → ARP Reply: “10.0.44.1 is a9a9”**

- Host A stores entry in ARP cache.

Now Host A can send the frame to R1.

---

# **5.3 Inside Router R1**

R1 receives the frame:

### **Step 1: Discards L2 header**

It only cares about the **L3 packet**.

### **Step 2: Looks up destination IP**

- Destination = `10.0.66.7`
- R1 routing table says:

  ```
  10.0.66.0/24 → next-hop 10.0.55.2 (R2)
  ```

### **Step 3: R1 needs R2’s MAC**

R1 does not know MAC of 10.0.55.2 yet → ARP again.

### **R1 → ARP Request: “Who has 10.0.55.2?”**

### **R2 → ARP Reply**

- R1 stores MAC (eee2)

Now R1 can send the frame to R2.

---

# **5.4 Inside Router R2**

R2 receives frame from R1:

### **Step 1: Discards L2 header**

### **Step 2: Looks up destination**

- Destination = 10.0.66.7
- R2 routing table (directly connected):

  ```
  10.0.66.0/24 → Left
  ```

### **Step 3: Needs Host B’s MAC**

R2 sends ARP request:

- **“Who has 10.0.66.7?”**

Host B replies:

- **“I do. My MAC is c7c7.”**

Now R2 sends the packet to Host B.

---

# **6. When Host B Replies to Host A**

Everything becomes **much faster**:

- ARP tables already filled
- Routing tables already known
- No more broadcast required

Routers simply forward traffic instantly.

---

# **7. How This Scales to the Internet**

The internet is basically:

```
A huge chain of routers,
each forwarding packets based on routing tables.
```

Dynamic routing protocols (e.g., BGP) keep the entire worldwide routing fabric updated automatically.

This allows:

- A host in Denmark to reach a host in Japan
- A phone in Nepal to reach a server in the USA
- Data to move across 10–30 routers in milliseconds

---

# **✔ Summary: What Routers Do**

### **Routers:**

- Connect networks
- Forward packets between networks
- Maintain:

  - Routing tables (where to send packets)
  - ARP tables (how to send packets at L2)

- Remove & rebuild Layer 2 headers for each hop
- Make forwarding decisions using:

  - Direct routes
  - Static routes
  - Dynamic routing protocols

### **Routers enable global communication. Without them, the internet cannot exist.**

---

## Illustrative Version

```

                  ┌──────────────────────────────┐
                  │         Router R1            │
                  │   IPs: 10.0.44.1 / 10.0.55.1 │
                  │   MAC: eee1 / eee3           │
                  └──────────────────────────────┘
                         /                \
                        /                  \
                       v                    v
           (44.x LAN)                              (55.x LAN)
   ┌──────────────────┐                    ┌──────────────────┐
   │  Host A          │                    │  R2              │
   │  IP 10.0.44.9    │                    │  10.0.55.2       │
   │  MAC a9a9        │                    │  MAC eee2        │
   └──────────────────┘                    └──────────────────┘
```
