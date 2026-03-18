# Everything Switches Do to Facilitate Communication

## Understanding Switching

**Switching** is the process of moving data within a network using devices called **switches**.  
Devices connected to a switch belong to the same IP network, allowing them to communicate efficiently.

A switch maintains something called a **MAC Address Table**, which maps switch ports to connected device MAC addresses.  
Initially, this table is **empty**, but as communication occurs, the switch **learns** and updates it.

---

## Switch Actions: Learn, Flood, Forward

Let’s consider a network of hosts **A, B, C, and D** connected through a switch.

### 1. Learn

When a frame arrives at a switch port, the switch records the **source MAC address** and the **port** it came from.  
This helps the switch know where that particular device is located for future communication.

### 2. Flood

If the **destination MAC address** is **unknown** (not in the MAC Address Table), the switch **floods** the frame — sending it out through **all other ports**.  
Hosts that are not the intended destination ignore the frame, while the correct host (say, Host D) responds back directly.

As a result, the switch **learns** the MAC address of Host D as well.

### 3. Forward

Once the switch knows both the source and destination MAC addresses, it **forwards** the frame directly to the correct port instead of flooding it.  
This makes communication faster and reduces unnecessary traffic.

---

## Example Scenario

If **Host A** sends data to **Host D**:

1. Switch learns A’s MAC address.
2. Floods the frame to all ports.
3. D responds back to A → Switch learns D’s MAC address.
4. Future frames between A and D go directly — no flooding needed.

---

## Traffic "Through" vs "To" the Switch

- **Through the switch:** Normal traffic between hosts (A → D) that the switch simply forwards.
- **To the switch:** Traffic meant for the switch itself. Since the switch is also a host in the network, it has its own **MAC address** and **IP address**.

Thus, a switch acts both as a **network device** and a **host** when necessary.

---

## Unicast vs Broadcast

| Type          | Description                                                              | Switch Behavior                                                                |
| ------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| **Unicast**   | Frame sent from one device to another (specific destination MAC).        | Switch floods only if destination MAC is unknown; otherwise forwards directly. |
| **Broadcast** | Frame sent to **all** devices on the network (MAC: `FF:FF:FF:FF:FF:FF`). | Switch always floods broadcast frames to all connected ports.                  |

🔹 **Flood** is a _switch action_ (when it doesn’t know the destination).  
🔹 **Broadcast** is a _type of frame_ (meant for everyone).

Switches only send broadcasts if the traffic is **to or from** the switch.

---

## VLAN (Virtual Local Area Network)

A **VLAN** divides a single physical switch into multiple **logical** networks.  
Each VLAN acts like an independent “mini-switch.”

Example:  
A single purple switch can be divided into:

- **Red VLAN** — Handles one group of devices
- **Blue VLAN** — Handles another group

Each VLAN:

- Has its own MAC Address Table.
- Performs **Learn**, **Flood**, and **Forward** actions _independently_.
- Keeps traffic **isolated** between VLANs.

This improves security and reduces unnecessary traffic within the network.

---

## Multiple Switches

When networks grow beyond one switch, we connect **multiple switches** together.  
Each switch acts as a separate device that learns its **own MAC Address Table** and performs **Learn**, **Flood**, and **Forward** actions **independently**.

Even when connected, switches **do not automatically share** MAC tables with one another — they each learn dynamically by observing frames as they arrive.

---

### Example Scenario

Let’s take two switches:

- **Red Switch** connects Host A and Host C.
- **Blue Switch** connects Host B and Host D.  
  Both switches are linked together by an **uplink cable** (like a trunk line).

#### Step 1: Initial Communication

Suppose **Host C** (on the Red switch) wants to send data to **Host D** (on the Blue switch).  
Both are in the **same IP network** — meaning they can talk directly (no router needed).

At the start:

- Red Switch’s MAC table: empty
- Blue Switch’s MAC table: empty

---

#### Step 2: Learn and Flood

1. **Host C sends a frame** destined for Host D’s MAC.

   - The Red Switch learns Host C’s MAC and port.
   - The switch does **not know** where D is (not in its table), so it **floods** the frame through all ports, including the uplink to the Blue Switch.

2. **Blue Switch receives** the flooded frame from the uplink.

   - It learns that Host C’s MAC is reachable **via the uplink port**.
   - Since it also doesn’t know where Host D is yet, it **floods** the frame to all of its local ports.

3. **Host D receives** the frame, recognizes its MAC, and **responds** directly to Host C.

---

#### Step 3: Building MAC Tables

- The **response from Host D** goes through the Blue Switch.

  - The Blue Switch learns Host D’s MAC → stored as:  
    `Host D → Port 2`
  - Forwards the frame through the uplink (since Host C’s MAC is known there).

- The **Red Switch** receives the frame from the uplink.
  - Learns that Host D’s MAC is reachable via the uplink port.
  - Forwards the frame directly to Host C.

Now both switches have **complete mappings**:

- Red Switch knows:  
  `Host C → Port 1`  
  `Host D → Uplink`
- Blue Switch knows:  
  `Host D → Port 2`  
  `Host C → Uplink`

---

#### Step 4: Future Communication

From now on, whenever C sends to D (or vice versa), there’s **no flooding**.  
Frames go directly through the correct port using the MAC table.  
This drastically improves efficiency and reduces unnecessary traffic.

---

### Important Notes

1. **Independent Learning**  
   Each switch builds and maintains its own MAC address table — no switch directly tells another what it knows.

2. **Loop Risks**  
   When multiple switches are interconnected in complex topologies, redundant links can cause **broadcast loops**.  
   For example, a broadcast frame could circulate forever between switches.  
   To prevent this, modern switches use the **Spanning Tree Protocol (STP)** — which automatically disables redundant paths while keeping backups ready.

3. **Inter-VLAN Communication**  
   If VLANs are used, the uplink between switches must be configured as a **trunk link**, carrying multiple VLANs using tagging (usually **IEEE 802.1Q**).  
   Each VLAN still maintains its own isolated MAC table across switches.

4. **Scalability**  
   This architecture allows a network to scale by adding more switches and hosts without breaking communication logic.  
   Every switch learns automatically through normal traffic.

---

### Summary Example Table

| Device      | Connected Hosts | Learns From     | Sends To        | Notes              |
| ----------- | --------------- | --------------- | --------------- | ------------------ |
| Red Switch  | A, C            | A and C locally | Blue via uplink | Learns D from Blue |
| Blue Switch | B, D            | B and D locally | Red via uplink  | Learns C from Red  |

---

### Visualization (Conceptually)

```
[Host A]         [Host C]
    |                 |
    |                 |
+---+-----------------+---+
|       Red Switch       |
+---+-----------------+---+
    |       (uplink)
    |
+---+-----------------+---+
|       Blue Switch      |
+---+-----------------+---+
    |                 |
[Host B]          [Host D]
```

In this setup:

- The Red and Blue switches communicate via an uplink.
- Each learns where hosts are located based on frame observation.
- Traffic becomes direct and efficient after initial flooding and learning.

---

### Final Understanding

Multiple switches extend a single network across physical distance while keeping communication **local**, **efficient**, and **loop-free**.  
Each switch:

- Operates independently but cooperatively.
- Maintains its own MAC address mapping.
- Uses flooding only when necessary.
- Learns dynamically and forwards intelligently.

Together, they form the **foundation of scalable Ethernet networking**.

---

## Summary Table

| Action      | Description                  | Example Behavior           |
| ----------- | ---------------------------- | -------------------------- |
| **Learn**   | Maps MAC address to port     | Learns Host A on Port 1    |
| **Flood**   | Broadcasts when MAC unknown  | Sends to all except source |
| **Forward** | Sends directly to known port | Host A → Host D            |

| Type          | MAC Address           | Flooded?        | Notes                      |
| ------------- | --------------------- | --------------- | -------------------------- |
| **Unicast**   | Device’s specific MAC | Only if unknown | Sent directly once learned |
| **Broadcast** | `FF:FF:FF:FF:FF:FF`   | Always          | Sent to all devices        |

---

### In Short

Switches are the heart of internal network communication.  
They:

- Learn device locations,
- Reduce unnecessary traffic,
- Support logical segmentation (VLANs), and
- Keep data transfer efficient and organized.
