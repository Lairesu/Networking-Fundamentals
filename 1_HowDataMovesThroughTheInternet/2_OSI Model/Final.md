# Networking Notes – Switch & OSI Model (Fundamentals)

## Understanding the OSI Model

The main purpose of networking is **to share data** between hosts.  
To make this possible, we follow a layered structure known as the **OSI (Open Systems Interconnection) Model**.

### Analogy:

Just like the human body has different systems:

- Cardiovascular → pumps blood
- Digestive → processes food
- Nervous → enables thought

Each system has its role — and together, they make the body function properly.

Similarly, the OSI model has **layers**, each performing its specific role so that **data can be shared successfully between hosts**.

---

## OSI Model Overview (7 Layers)

1. **Physical Layer**
2. **Data Link Layer**
3. **Network Layer**
4. **Transport Layer**
5. **Session Layer**
6. **Presentation Layer**
7. **Application Layer**

---

###  Layer 1 – Physical Layer

Also called the **“bit transmission” layer**.

- It deals with **transporting raw bits** (0s and 1s) from one host to another.
- Responsible for **putting and retrieving bits** onto the transmission medium.

####  Examples of Physical Technologies:

- Cables (Ethernet, fiber optic)
- Wi-Fi
- Repeaters
- Hubs

> The goal of the Physical Layer: **Transport bits**.

---

### Layer 2 – Data Link Layer

Known as the **hop-to-hop delivery layer**.

- It works **closely with the physical layer**.
- Responsible for **framing** and **error detection** during transmission.
- Uses **MAC (Media Access Control) addresses** to identify devices within the same local network.

#### Examples:

- **NIC (Network Interface Card)** / Wi-Fi Access Card
  - Older NICs were large, but now they fit inside smartwatches or phones!
- **Switches**

#### MAC Address:

A unique identifier assigned to each NIC.

Examples of MAC address formats:

- `95-9c-83-4b-eb-45` → Windows
- `95:9c:83:4b:eb:45` → Linux
- `959c.834b.eb45` → Routers/Switches (divided into 4 groups of 3 hex digits)

> Goal of Layer 2: **Hop-to-hop delivery using MAC addresses**.

---

### Layer 3 – Network Layer

This layer is responsible for **end-to-end delivery** between different networks.

- Uses **IP addresses** for routing data from the **source host** to the **destination host**.
- Handles **logical addressing**, **routing**, and **path determination**.

#### IP Address:

- A unique 32-bit address (IPv4) written as 4 octets (e.g., `192.168.1.1`).
- Ensures that data can move across different networks — not just within one.

#### Example: Sending Data Across Countries

Suppose we send an email from **Denmark to Japan**  
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

### ARP – Address Resolution Protocol

**ARP** acts as the bridge between the **Network Layer (IP)** and the **Data Link Layer (MAC)**.

- It maps **IP addresses → MAC addresses**.
- Enables devices to find the correct hardware address to send data to.

> Think of ARP as the translator that connects “logical” addressing (IP) with “physical” addressing (MAC).

---

## Summary

| Layer | Name      | Function            | Address Type | Example Devices |
| ----- | --------- | ------------------- | ------------ | --------------- |
| 1     | Physical  | Transmit bits       | None         | Cable, Hub      |
| 2     | Data Link | Hop-to-hop delivery | MAC Address  | Switch, NIC     |
| 3     | Network   | End-to-end delivery | IP Address   | Router          |

---

 **In short:**

> - **Layer 1:** Moves bits
> - **Layer 2:** Moves frames (hop-to-hop)
> - **Layer 3:** Moves packets (end-to-end)

---

## Encapsulation (Data Transmission from Host 2 → Host 2)

When **Host 1** sends data to **Host 2**, it passes through the OSI layers.  
Each layer **adds its own header**, creating a new data unit.  
This process is called **Encapsulation**.

### ➡️ Sending Side (Host 1)

| Layer | Function                     | Data Unit   | What It Adds                                  |
| :---- | :--------------------------- | :---------- | :-------------------------------------------- |
| 7–5   | User/application interaction | Data        | (User data)                                   |
| 4     | Transport                    | **Segment** | TCP/UDP Header                                |
| 3     | Network                      | **Packet**  | IP Header (source + destination IP)           |
| 2     | Data Link                    | **Frame**   | MAC Header (source + destination MAC)         |
| 1     | Physical                     | **Bits**    | Converts data into 1s and 0s for transmission |

---

###  Receiving Side (Host 2)

When Host 2 receives the signal, the process is **reversed** (called **De-capsulation**):

| Step | Action                                                   |
| :--- | :------------------------------------------------------- |
| 1️  | Receives binary bits (Layer 1)                           |
| 2️  | Converts to frame → checks MAC address (Layer 2)         |
| 3️  | Extracts packet → checks IP (Layer 3)                    |
| 4️ | Extracts segment → verifies port & reliability (Layer 4) |
| 5️ | Passes final data to application (Layer 7)               |

**Result:** User sees the received data.

---

## Additional Notes

- **Network devices** operate at specific OSI layers  
  (e.g., Hub → L1, Switch → L2, Router → L3, etc.)
- **Network protocols** also belong to certain layers  
  (e.g., IP → L3, TCP → L4, HTTP → L7)
- **Some overlap exists** — for example, **ARP** connects **Layer 2 & Layer 3**.
- **The OSI model is conceptual**, not strict — it defines **what’s required** for communication, not exact implementation.
