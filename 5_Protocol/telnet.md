# Telnet — Teletype Network

## Overview

Telnet is a client/server application protocol that provides access to virtual terminals of remote systems on local area networks or the internet. It is one of the oldest internet protocols — used as the standard TCP/IP protocol for virtual terminal service.

> Telnet was replaced by SSH for the same reason HTTP was replaced by HTTPS — no encryption.

---

## Port

| Protocol | Port   |
| -------- | ------ |
| Telnet   | TCP 23 |

---

## How Telnet Works

### Connection Process

```
Telnet client initiates request to Telnet server
        ↓
Connection established
        ↓
Client sends commands to server
        ↓
Server processes commands and responds
```

### Character Flow

```
User types on local computer
        ↓
Local OS accepts characters
        ↓
Telnet client converts to NVT (Network Virtual Terminal) characters
        ↓
NVT characters travel through internet via local TCP/IP protocol stack
        ↓
Remote Telnet server converts NVT to format understood by remote computer
        ↓
Remote OS receives characters from pseudo terminal driver
        ↓
Passes to appropriate application
```

---

## Why Telnet is Insecure

Telnet has no encryption whatsoever:

- Credentials sent in plaintext
- Commands sent in plaintext
- Responses sent in plaintext
- Anyone on network can read everything

```
Wireshark on same network:
→ capture Telnet traffic
→ see username and password
→ see every command typed
→ complete session visible
```

---

## Telnet vs SSH

|                    | Telnet                   | SSH             |
| ------------------ | ------------------------ | --------------- |
| **Port**           | 23                       | 22              |
| **Encryption**     | None                     | Full encryption |
| **Authentication** | Plaintext                | Key or password |
| **Security**       | Insecure                 | Secure          |
| **Still used**     | Rarely, misconfiguration | Everywhere      |

> SSH was created specifically to replace Telnet. Same functionality, completely secure.

---

## Cybersecurity Relevance

### Attack Process

```
Nmap shows port 23 open
        ↓
telnet <target IP>
        ↓
Try default credentials:
→ admin/admin
→ root/root
→ blank username/password
        ↓
If no auth required:
→ direct root access
→ game over
```

### Capturing Telnet Credentials

```
Wireshark filter: telnet
        ↓
Follow TCP stream
        ↓
See complete session in plaintext:
→ username
→ password
→ every command typed
→ every response received
```

### Common Default Credentials

| Username | Password |
| -------- | -------- |
| root     | (blank)  |
| admin    | admin    |
| admin    | password |
| root     | root     |

---

## HTB Meow — What Happened

```
Nmap found port 23 open (Telnet)
        ↓
telnet <target IP>
        ↓
No authentication required
        ↓
Direct root access
        ↓
Found flag in /root/flag.txt
```

> No authentication on Telnet = complete system access.
> Most critical misconfiguration possible.

---

## One Line Summary

```
Telnet = SSH without encryption
         same remote access
         everything in plaintext
         finding it open = easy target
         always try blank credentials first
```
