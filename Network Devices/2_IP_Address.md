# IP Address

## Definition

An **IP (Internet Protocol) address** is a unique identifier assigned to each host on a network.  
It allows devices to find and communicate with each other.

---

## structure of an IPv4 Address

- IPv4 uses **32 bits** divided into **4 octets** (each octet = 8 bits).
- Each octet’s value ranges from **0 to 255** (since 8 bits can represent 2⁸ = 256 values).
- Written in **dotted decimal format**, e.g.:

`127.10.255.0`

### Example Breakdown

| Octet     | Binary Bits | Example Decimal |
| :-------- | :---------- | :-------------- |
| 1st       | 8 bits      | 127             |
| 2nd       | 8 bits      | 10              |
| 3rd       | 8 bits      | 255             |
| 4th       | 8 bits      | 0               |
| **Total** | **32 bits** |                 |

---

##  Hierarchy in IP Addresses

IP addresses are **hierarchical**, meaning they represent a structured network layout — similar to how addresses work in the real world.

### Real-world Analogy (using Denmark 🇩🇰)

| Real-world Analogy | IP Example  | Meaning                               |
| :----------------- | :---------- | :------------------------------------ |
| Country            | 50.x.x.x    | Larger network (e.g., Denmark)        |
| City               | 50.40.x.x   | Sub-network (e.g., Copenhagen)        |
| Municipality       | 50.40.20.x  | Smaller sub-network (e.g., Glostrup)  |
| Street/House       | 50.40.20.15 | Individual host (e.g., your computer) |

**Key idea:**  
> Each part of the IP address gives more specific location information within the network.

---

## Network & Host Parts

An IP address is divided into two main parts:

1. **Network ID** – identifies the network (like the “country + city”)
2. **Host ID** – identifies a specific device within that network (like the “house number”)

### Example:

`192.168.1.10`

If the **subnet mask** is `255.255.255.0`:

- **Network ID:** 192.168.1
- **Host ID:** 10

---

## Types of IP Addresses

| Type          | Example               | Description                               |
| :------------ | :-------------------- | :---------------------------------------- |
| **Public**    | 8.8.8.8               | Used on the internet (globally unique)    |
| **Private**   | 192.168.x.x, 10.x.x.x | Used in local networks (e.g., home Wi-Fi) |
| **Loopback**  | 127.0.0.1             | Used for testing your own device          |
| **Broadcast** | 255.255.255.255       | Sends data to all hosts on a network      |
