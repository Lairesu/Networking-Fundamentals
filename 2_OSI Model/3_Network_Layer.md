# 🌍 Layer 3 – Network Layer

This layer is responsible for **end-to-end delivery** between different networks.

- Uses **IP addresses** for routing data from the **source host** to the **destination host**.
- Handles **logical addressing**, **routing**, and **path determination**.

## IP Address:

- A unique 32-bit address (IPv4) written as 4 octets (e.g., `192.168.1.1`).
- Ensures that data can move across different networks — not just within one.

## Example: Sending Data Across Countries

Suppose we send an email from **Denmark host to Japan host** 🌍  
There are **3 routers** between sender and receiver.

| Hop                 | Source MAC → Destination MAC | IP Source → IP Destination |
| ------------------- | ---------------------------- | -------------------------- |
| Host → Router 1     | Host MAC → Router1 MAC       | Host IP → Receiver IP      |
| Router 1 → Router 2 | Router1 MAC → Router2 MAC    | Host IP → Receiver IP      |
| Router 2 → Router 3 | Router2 MAC → Router3 MAC    | Host IP → Receiver IP      |
| Router 3 → Receiver | Router3 MAC → Receiver MAC   | Host IP → Receiver IP      |

- The **MAC address** changes at every hop (hop-to-hop).
- The **IP address** stays the same (end-to-end).

When the data finally reaches the destination host,  
the data link and network layer information are removed,  
and the host processes the actual data (like an email message).

---

### 🔗 ARP – Address Resolution Protocol

**ARP** acts as the bridge between the **Network Layer (IP)** and the **Data Link Layer (MAC)**.

- It maps **IP addresses → MAC addresses**.
- Enables devices to find the correct hardware address to send data to.

> 🧩 Think of ARP as the translator that connects “logical” addressing (IP) with “physical” addressing (MAC).

---
