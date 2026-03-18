# Subnet Mask

A **subnet mask** helps us determine how big a network is, how many IP addresses it contains, and which part of an IP address represents the **network** vs the **host**.

## Example

IP address:

`192.168.32.5`

Subnet mask:

`255.255.255.0`

---

### Converting IPv4 to Binary

IP address in binary:

`11000000.10101000.00100000.0000101`

subnet Mask in binary:

`11111111.11111111.11111111.0000`

The `1`s in the subnet mask represent the **network portion**, while the `0`s represent the **host portion**.

With the subnet mask `255.255.255.0`, the first **24 bits** define the network, leaving **8 bits for hosts**.

---

### Number of Available IP Addresses

With **8 host bits**, we get:
2^8 = 256 addresses

However, two addresses cannot be used:

- **Network address**
- **Broadcast address**

So the usable addresses are:

256 - 2 = 254 usable IP addresses

---

### When We Need More IP Addresses

Sometimes a network needs more than **254 devices**. For example, if we need about **500 IP addresses**.

Originally we had:

2^8 = 256 addresses

If we borrow one bit from the network portion (changing one `1` to `0` in the subnet mask), we increase the host space to **9 bits**:

2^9 = 512 addresses

This gives enough IP addresses for our network.

---

### Subnetting

Adjusting the subnet mask to change the number of available hosts or networks is called **subnetting**.

## 192.168.1.0 /24

Subnet mask: `255.255.255.0`  
Total usable addresses: **254**

---

### Splitting a Home Network

Sometimes we want to break a single network into **smaller networks** using **subnetting**.  
For example:

1. Wireless
2. IoT devices
3. DMZ
4. User devices

More networks require **more network bits**.  
To achieve this, we **borrow bits from the host portion** of the address.

---

### Creating 4 Networks

Our original network:

`192.168.1.0 /24`

This means:

- **24 bits for the network**
- **8 bits for hosts**

If we want **4 networks**, we need **2 additional network bits** because:

2^2 = 4 networks

So we borrow **2 bits from the host portion**.

Binary subnet mask becomes:

`11111111.11111111.11111111.11000000`

In decimal:

`255.255.255.192`

New prefix:

`/26`

---

### Finding the Increment

The block size (increment) is determined by the last subnet mask value:

256 - 192 = 64

So each subnet increases by **64**.

---

### Subnet Ranges

```
192.168.1.0 - 192.168.1.63
192.168.1.64 - 192.168.1.127
192.168.1.128 - 192.168.1.191
192.168.1.192 - 192.168.1.255
```

Each subnet contains **64 total addresses**:

- **1 network address**
- **1 broadcast address**
- **62 usable host addresses**

---

### Result

We successfully created **4 smaller networks** from the original `192.168.1.0 /24` network using **subnetting**.

> Instead of doing the binary every time, you can jump straight to the increment trick:
>
> `256` - subnet mask value

Example:

256 - 192 = 64

So you instantly know the networks start at:

`0, 64, 128, 192`

## Subnetting Based on Number of Hosts vs Number of Networks Needed

Subnetting can be approached in two ways:

- Based on the **number of networks (subnets)** required
- Based on the **number of hosts per network** required

To calculate this quickly, we use the **Nosferatu chart** (binary weight chart):

[128, 64, 32, 16, 8, 4, 2, 1]

---

### Subnetting Based on Number of Networks

1. Use the **Nosferatu chart** to determine how many **bits need to be borrowed** from the host portion to create the required number of networks.
2. **Borrow bits from the host side (starting from the left).**
3. Determine the **increment (block size)** using:
   256 - subnet_mask_value
4. Use the increment to **calculate the subnet ranges**.

---

### Subnetting Based on Number of Hosts

1. Use the **Nosferatu chart** to determine how many **host bits are required** to support the needed number of hosts.
2. **Reserve the required host bits (from the right side).**
3. Determine the **increment (block size)** from the subnet mask.
4. Use the increment to **calculate the subnet ranges**.
