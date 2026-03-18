# IP Address

An **IP Address** is a numerical identifier assigned to a device (host) on a network.  
It allows devices to **identify and communicate with each other over a network or the Internet**.

IPv4 addresses are written as four numbers separated by dots:

Example:

192.168.1.4

IPv6 addresses use hexadecimal numbers and letters, but IPv4 is easier to understand first.

---

# Key Components of Network Configuration

For a device to communicate on a network, three important pieces of information are required:

- **IP Address**
- **Subnet Mask** (called *Netmask* in Linux/macOS)
- **Default Gateway**

These three work together to allow devices to communicate within a network and with external networks like the Internet.

---

# How Devices Get an IP Address

When a device connects to a network (for example Wi-Fi), it usually receives an IP address automatically using **DHCP (Dynamic Host Configuration Protocol)**.

Typically, the **router** in a home or office network runs a DHCP server that assigns IP addresses to connected devices.

So the process looks like this:

1. Device joins the network
2. Device requests an IP address
3. Router's DHCP server assigns one

---

# Network Portion vs Host Portion

Consider the address:

192.168.1.4

In most home networks using the subnet mask:

255.255.255.0

The IP address is divided into two parts:

| Portion | Meaning |
|-------|--------|
| Network Portion | Identifies the network |
| Host Portion | Identifies the device within that network |

For the example above:

Network: **192.168.1**  
Host: **4**

This means the device is host number **4** on the **192.168.1** network.

---

# Communication Inside the Same Network

If **Host A (192.168.1.4)** wants to talk to **Host B (192.168.1.7)** and both are on the same network:

They communicate **directly through the switch/router**, without leaving the local network.

---

# Communication with a Different Network

If **Host A** wants to communicate with something outside its network, such as:

**netflix.com**


The process changes:

1. Host A sends the request to the **Default Gateway**
2. The **Default Gateway (router)** forwards the packet
3. The packet travels through multiple routers on the Internet
4. Eventually it reaches the destination server

This process is called **routing**.

---

# Subnet Mask

Example subnet mask:

255.255.255.0

The subnet mask tells the device:

- which part of the IP address identifies the **network**
- which part identifies the **host**

It allows the device to determine:

- whether a destination is **local**
- or **requires routing through the gateway**

---

# Reserved Addresses in a Subnet

In a typical `/24` network like:

192.168.1.0/24

There are **256 total addresses**, but not all can be used by hosts.

### Network Address

192.168.1.0

This represents the **entire network itself** and cannot be assigned to a device.

---

### Broadcast Address

192.168.1.255

Packets sent to this address are **broadcast to every device on the network**.

---

### Router / Default Gateway

Often something like:

192.168.1.1

This address belongs to the **router**, which connects the local network to other networks.

Technically it *can* be any valid host address, but `.1` is commonly used.

---

# Usable Host Addresses

In a `/24` network:

Total addresses: **256**

Reserved:
- 1 Network address
- 1 Broadcast address

Remaining usable addresses:

**254 possible host addresses**

Example usable range:

192.168.1.1 → 192.168.1.254