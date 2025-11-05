# Networking Fundamentals Notes

## 1. Host

A **host** is any device that sends or receives traffic within a network.  
Examples include:

- Laptops, computers, smartphones
- Cloud components like servers
- IoT devices like smart TVs, printers, speakers

### Categories of Hosts

Hosts can be categorized into **clients** and **servers**:

- **Client:** Initiates requests (e.g., a browser requesting data from a server).
- **Server:** Responds to client requests (e.g., a backend providing data).

> Note: The terms _client_ and _server_ are relative depending on the communication.  
> Example:
>
> - The browser (client) requests data from the backend (server).
> - The backend (client) requests data from the database (server).
> - When the database needs updates, the updater becomes the server, and the database becomes the client.

---

## 2. IP Address

An **IP address** uniquely identifies a host in a network.

### Structure

- Represented in **32 bits**, divided into **4 octets** (8 bits each).
- Each octet ranges from **0–255**.
- Example: `127.10.255.0`

### Hierarchical Assignment

IP addresses are assigned in a hierarchy.  
For example:

- Denmark: `50.x.x.x`
- Copenhagen: `50.40.x.x`
- Glostrup: `50.30.20.x`

This structure shows how IPs represent regions or networks within larger networks.

---

## 3. Network

A **network** connects hosts to transport traffic between them.

### Before Networks

Data was exchanged using **physical media** like CDs, USB drives, and flash drives.

### Logical Grouping

A network is a **logical grouping of hosts** requiring similar connectivity.  
Examples:

- Home Wi-Fi: connects smartphones, TVs, and computers
- Coffee shop Wi-Fi: connects customer devices
- College (network) with classrooms (sub-networks)

### Central Connectivity

All networks connect to a **central resource** — the **Internet**.  
**ISP (Internet Service Provider)** acts like an API that provides connectivity between networks.

> ISPs are a deeper topic and will be studied later.

---

**Summary:**

- A **host** sends or receives data.
- An **IP address** uniquely identifies a host.
- A **network** connects multiple hosts for communication.

## 4. Repeater

When hosts are far apart, the signal weakens over distance — this is called signal decay.  
**Repeaters** are devices that amplify or regenerate the weakened signal and resend it so it can reach other connected devices.

They don’t change or interpret the data — they simply **repeat** the signal.

---

## 5. Hub

A **Hub** is a multi-port repeater that allows multiple computers (hosts) to connect and share information.  
When one host sends data, the hub **copies and broadcasts** it to all other connected devices.

However, this can cause **data collisions** and unnecessary traffic since all devices receive the same data, even if it wasn’t meant for them.

---

## 6. Bridge

To fix hub limitations, we use a **Bridge**.  
A bridge connects two network segments (usually two hubs) and has **two ports**.  
It learns which hosts are on which side and forwards data **only** where it’s needed.

Example: Denmark and Sweden are connected by a real-world bridge; in networking, a bridge works similarly, forwarding traffic intelligently.

---

## 7. The Switch

Previously, we learned about **Hubs** and **Bridges**:

- **Hub** – Broadcasts data to all connected devices → causes _data collisions_.
- **Bridge** – Removes collisions by dividing the network but can only connect _two ports_.

Now, to overcome these limitations, we use a **Switch**.

### 🧩 What is a Switch?

A **Switch** is like a combination of a **Hub** and a **Bridge**:

- It has **multiple ports** (unlike bridges which connect only two).
- It **learns which hosts** are connected to each port.
- It **facilitates communication within a network** efficiently.

> Think of a switch as a local “postmaster” that knows exactly which port (or host) to deliver data to, instead of shouting to everyone.

### 🏙️ Example:

In a city (like Denmark 🇩🇰), if data needs to be sent from **Copenhagen to another city**, the switch ensures it reaches only that city — **not all other cities**.  
So, switches make communication faster and more direct within a **local network**.

---

### 🧮 Summary Table – Networking Devices

| Device     | Ports | Learns Addresses | Broadcasts Data | Main Limitation                            |
| :--------- | :---: | :--------------: | :-------------: | :----------------------------------------- |
| **Hub**    | Many  |      ❌ No       |     ✅ Yes      | Causes **collisions** and **inefficiency** |
| **Bridge** |   2   |      ✅ Yes      |      ❌ No      | Can only connect **2 networks**            |
| **Switch** | Many  |      ✅ Yes      |      ❌ No      | Most **efficient** for local networking    |

---

## 8. Router

When networks grow and multiple subnets exist, we use **Routers** to connect them.  
A router facilitates communication **between networks** (not just within).

Routers manage:

- **Routing paths** (deciding the best way to reach the destination)
- **Traffic control** (security, filtering, redirecting)
- **Gateway connections** (entry and exit points of networks)

### Example:

Denmark (IP: 134.3.3.x) and Japan (IP: 123.2.4.x).  
To send data from Copenhagen (134.3.3.8) to Kyoto (123.2.4.7), packets travel through routers using **routing paths** — each router forwards it to the next until it reaches the destination.

### Gateway:

A **Gateway** is the “way out” of a local network — the router interface that connects to the outside world.

---

## 9. Routing vs Switching

- **Routing** → Moving data **between** networks (handled by Routers).
- **Switching** → Moving data **within** a network (handled by Switches).

---

## 10. Other Network Devices

- **Access Points:** Allow wireless devices to connect to wired networks.
- **Firewalls:** Filter incoming and outgoing traffic for security.
- **Load Balancers:** Distribute network traffic across multiple servers.
- **Proxies:** Intermediaries that handle requests and responses for clients.
- **Virtual Routers/Switches:** Software-based versions used in cloud environments.

All these devices ultimately perform **routing or switching** in one form or another.
