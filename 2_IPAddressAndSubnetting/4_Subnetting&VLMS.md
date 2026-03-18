# Subnetting and VLSM Guide

## What is Subnetting

Subnetting is the process of dividing a large network into smaller networks (subnets). This improves:

* Network organization
* Security
* Efficient IP usage

An IPv4 address has **32 bits** divided into:

```
Network portion | Host portion
```

Example:

```
192.168.1.0/24
```

`/24` means the first **24 bits** are the network portion.

Subnet mask:

```
11111111.11111111.11111111.00000000
255.255.255.0
```

---

# Basic Subnetting

## Step 1 — Find the subnet mask

Example:

```
/26
```

Binary mask:

```
11111111.11111111.11111111.11000000
```

Decimal mask:

```
255.255.255.192
```

---

## Step 2 — Find block size

Formula:

```
256 - mask_octet
```

Example:

```
256 - 192 = 64
```

Subnet increments:

```
0
64
128
192
```

---

## Step 3 — Find network range

Example subnet:

```
172.21.42.128/26
```

Range:

```
Network:   172.21.42.128
First IP:  172.21.42.129
Last IP:   172.21.42.190
Broadcast: 172.21.42.191
```

---

# Host Formula

To determine how many hosts a subnet supports:

```
2^h - 2
```

Where:

* `h` = number of host bits

Example:

```
4 host bits

2^4 - 2 = 14 hosts
```

---

# What is VLSM

**VLSM (Variable Length Subnet Masking)** allows different subnet sizes in the same network.

Instead of wasting IP addresses, you allocate subnets based on the **actual number of hosts needed**.

Example host requirements:

```
Network A: 50 hosts
Network B: 26 hosts
Network C: 10 hosts
Network D: 5 hosts
```

You allocate from **largest to smallest**.

---

# VLSM Example

Base network:

```
172.21.42.0/24
```

Host needs:

```
26 hosts
10 hosts
```

---

## Subnet for 26 Hosts

Find host bits:

```
2^4 - 2 = 14  (not enough)
2^5 - 2 = 30  (works)
```

Host bits = **5**

Prefix:

```
32 - 5 = /27
```

Subnet mask:

```
255.255.255.224
```

Block size:

```
256 - 224 = 32
```

Subnet:

```
172.21.42.128/27
```

Range:

```
Network:   172.21.42.128
First IP:  172.21.42.129
Last IP:   172.21.42.158
Broadcast: 172.21.42.159
```

---

## Subnet for 10 Hosts

Find host bits:

```
2^3 - 2 = 6  (not enough)
2^4 - 2 = 14 (works)
```

Host bits = **4**

Prefix:

```
/28
```

Mask:

```
255.255.255.240
```

Block size:

```
256 - 240 = 16
```

Next available network:

```
172.21.42.160/28
```

Range:

```
Network:   172.21.42.160
First IP:  172.21.42.161
Last IP:   172.21.42.174
Broadcast: 172.21.42.175
```

---

# VLSM Rules

1. Sort networks by **largest host requirement first**.
2. Allocate **largest subnet first**.
3. Move sequentially to the next available address.
4. Do not overlap subnet ranges.

---

# Quick Cheat Sheet

| Prefix | Mask            | Hosts |
| ------ | --------------- | ----- |
| /30    | 255.255.255.252 | 2     |
| /29    | 255.255.255.248 | 6     |
| /28    | 255.255.255.240 | 14    |
| /27    | 255.255.255.224 | 30    |
| /26    | 255.255.255.192 | 62    |
| /25    | 255.255.255.128 | 126   |
| /24    | 255.255.255.0   | 254   |

---

# Fast Method Engineers Use

Instead of binary, memorize the **block sizes**:

```
128
64
32
16
8
4
2
```

Example:

```
/26 → block size 64
/27 → block size 32
/28 → block size 16
```

Then simply count the subnet ranges.
