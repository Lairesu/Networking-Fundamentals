# TCPDump — Command Line Packet Analyzer

## Overview

TCPDump is the command line version of Wireshark.
Same job, no GUI — works in terminal only.

> If Wireshark is a GUI packet analyzer
> TCPDump is its terminal equivalent

---

## Why It Matters

```
Compromised servers in HTB:
→ no GUI available
→ TCPDump works in terminal
→ lightweight, always on Linux
→ standard tool on every Linux system
```

---

## Essential Commands

| Command                                | Purpose                              |
| -------------------------------------- | ------------------------------------ |
| `tcpdump -D`                           | List available interfaces            |
| `sudo tcpdump -i wlo1`                 | Capture on interface                 |
| `sudo tcpdump -i wlo1 -n`              | Show raw IPs, no hostname resolution |
| `sudo tcpdump -i wlo1 -A port 80`      | Show packet contents ASCII           |
| `sudo tcpdump -i wlo1 port 53`         | Filter DNS traffic                   |
| `sudo tcpdump -i wlo1 port 80`         | Filter HTTP traffic                  |
| `sudo tcpdump -i wlo1 host 8.8.8.8`    | Filter by host                       |
| `sudo tcpdump -i wlo1 -w capture.pcap` | Save to file                         |
| `tcpdump -r capture.pcap`              | Read saved file                      |

---

## Important Flags

| Flag | Purpose                           |
| ---- | --------------------------------- |
| `-i` | Specify interface                 |
| `-n` | No name resolution — show raw IPs |
| `-A` | Show packet content in ASCII      |
| `-w` | Save capture to .pcap file        |
| `-r` | Read from .pcap file              |
| `-q` | Quiet mode, less output           |

---

## TCPDump vs Wireshark

|                      | TCPDump              | Wireshark         |
| -------------------- | -------------------- | ----------------- |
| Interface            | Terminal             | GUI               |
| Best for             | Capturing on servers | Analyzing traffic |
| Needs GUI            | No                   | Yes               |
| Available on servers | Always               | Rarely            |
| Reading packets      | -A flag              | Click and expand  |

---

## Best Combo Workflow

```
Capture with TCPDump on compromised server:
sudo tcpdump -i eth0 -w capture.pcap
↓
Transfer .pcap file to your machine
↓
Open in Wireshark for full analysis
↓
Read credentials, find attack patterns
```

---

## HTB Scenario

```
Compromise Linux server
↓
No GUI available
↓
sudo tcpdump -i eth0 -n -A port 80
↓
Capture HTTP credentials in plaintext
↓
Or save to file:
sudo tcpdump -i eth0 -w capture.pcap
↓
Analyze in Wireshark on your machine
```

---

## One Line Summary

```
TCPDump = Wireshark for terminal
capture on servers with no GUI
save to .pcap for Wireshark analysis
```
