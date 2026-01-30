# Communication Between Hosts in Different Networks (Across Routers)

Let’s consider:

- **Host A** (Source) in Network 1
- **Router R** connecting both networks
- **Host C** (Destination) in Network 2 (foreign network)

Each has its own **IP address** and **MAC address**.

---

## Step-by-Step Process

### 1. Host A prepares to send data to Host C

- Host A knows **Host C’s IP address** (maybe from DNS resolution or manual entry).
- Host A checks the **network portion** of Host C’s IP and realizes:
  > “Host C is not in my local network.”

---

### 2. IP Layer (Layer 3): End-to-End delivery

- Host A creates an **IP header** with:
  - **Source IP:** Host A’s IP
  - **Destination IP:** Host C’s IP

This **IP header stays the same** throughout the journey from Host A to Host C — even across multiple routers.  
Because Layer 3 ensures **end-to-end delivery**.

---

### 3. Data Link Layer (Layer 2): Hop-to-Hop delivery

- Host A now needs to send the frame **to its next hop**, which is the **router** (default gateway).
- But Layer 2 (Ethernet) requires **MAC addresses** for local delivery.
  - **Source MAC:** Host A’s MAC
  - **Destination MAC:** Router’s MAC

---

### 4. ARP (Address Resolution Protocol)

- Host A doesn’t know the router’s MAC address.
- It sends an **ARP request**:
  > “Who has this IP (Router’s IP)? Tell me your MAC address.”
- Router replies with its MAC.
- Host A **stores it in ARP cache** for future communication.

---

### 5. Frame Transmission

- Host A encapsulates the IP packet inside a **Layer 2 frame** and sends it:
  ```
  [Ethernet Frame:  MAC A → MAC Router]
      ↳ [IP Packet: IP A → IP C]
  ```

---

### 6. Router’s Role

- Router receives the frame.
- It **removes** the Layer 2 header (because MAC A → MAC Router is complete).
- It checks the **destination IP (Host C)** — realizes Host C belongs to a **different network**.
- Router consults its **routing table** to find the next hop (possibly another router or directly Host C’s network).

Then:

- Router **creates a new Layer 2 frame** for the next hop:
  - **Source MAC:** Router’s outgoing interface
  - **Destination MAC:** Next hop’s MAC (could be Host C’s MAC if directly connected)

---

### 7. Arrival at Host C

- Finally, the packet reaches Host C’s network.
- The router forwards the frame:
  ```
  [Ethernet Frame: MAC Router → MAC Host C]
      ↳ [IP Packet: IP A → IP C]
  ```
- Host C receives the frame, removes the L2 header, checks the IP header (destination = itself), and passes the data up to Layer 4 and above.

---

## Summary of Encapsulation and Decapsulation

| Layer     | Host A (Send)           | Router (Forward)        | Host C (Receive)       |
| --------- | ----------------------- | ----------------------- | ---------------------- |
| **L2**    | MAC A → MAC Router      | MAC Router → MAC Host C | Remove L2              |
| **L3**    | IP A → IP C (unchanged) | IP A → IP C (unchanged) | Check destination      |
| **L4-L7** | Same end-to-end         | Same                    | Deliver to application |

---

## Key Points

- **Layer 3 (IP)** ensures **end-to-end addressing** (unchanged across hops).
- **Layer 2 (MAC)** ensures **hop-to-hop delivery** (changes every time the packet crosses a router).
- **ARP** is used to map IP ↔ MAC within a local network.
- **Routers** strip and rebuild Layer 2 headers as packets move between networks.
