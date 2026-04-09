# FTP — File Transfer Protocol

## Overview

FTP is a protocol used to transfer files between a client and server
over a network. A host can upload files to a server which can then
distribute them, or hosts can act as servers themselves to transfer
files directly between each other.

> FTP is a connection oriented protocol.
> FTP is inherently insecure — all data including credentials
> are sent in plaintext.

---

## How FTP Works

FTP uses two separate connections:

| Port        | Purpose                                     |
| ----------- | ------------------------------------------- |
| **Port 21** | Control connection — commands and responses |
| **Port 20** | Data connection — actual file transfer      |

---

## Commands
| command            | Purpose                                     |
| ------------------ | ------------------------------------------- |
| **ftp -?**         | Display the 'ftp' client help menu          |
| **get <filename>** | Downlaod the file we found on the FTP server|


> Note: The response cose we get for the FTP 'Login Successful' is ``230``

## Active vs Passive Mode

### Active Mode

```
Client opens random port
Client tells server "connect to me on this port"
Server initiates data connection back to client
        ↓
Firewall sees unsolicited incoming connection
Firewall blocks it → nightmare for firewalls
```

### Passive Mode

```
Server opens random port
Server tells client "connect to me on this port"
Client initiates data connection to server
        ↓
Firewall happy — client always initiates
Most modern FTP connections use passive mode
```

---

## Anonymous FTP

FTP servers can be configured to allow anonymous login:

```
Username: anonymous
Password: (blank or any email)
```

> Whether anonymous login is allowed depends on how the
> server host has configured it.
> Anonymous FTP = major misconfiguration if sensitive files are present.

---

## FTP Security

### The Problem

FTP transfers everything in plaintext:

- Credentials visible
- Files visible
- Anyone on the network can capture and read everything

### Solutions

| Protocol | What it is                                            | Port |
| -------- | ----------------------------------------------------- | ---- |
| **SFTP** | SSH File Transfer Protocol — runs completely over SSH | 22   |
| **FTPS** | FTP + TLS encryption on top — like HTTPS but for FTP  | 990  |

```
FTP   = plaintext, insecure
SFTP  = completely different protocol, runs over SSH
FTPS  = original FTP with TLS encryption added on top
```

> SFTP is the most commonly used secure alternative.
> Finding plain FTP on a server = misconfiguration.

---

## Security Relevance

### Attacks and Misconfigurations

| Topic                     | Security Relevance                                   |
| ------------------------- | ---------------------------------------------------- |
| **Anonymous FTP**         | Login without credentials — huge misconfiguration    |
| **Plaintext credentials** | Captured easily with Wireshark or TCPDump            |
| **Active mode abuse**     | Can be used for port scanning through FTP server     |
| **FTP banners**           | Server reveals version on connect — check for CVEs   |
| **Sensitive files**       | Config files, credentials, flags left on FTP servers |

### Most Common HTB Scenario

```
Nmap scan shows port 21 open
        ↓
Try anonymous login:
ftp <target IP>
Username: anonymous
Password: (press enter)
        ↓
If successful → browse all files
        ↓
Look for credentials, config files, flags
```

### Capturing FTP Credentials

```
FTP sends credentials in plaintext
        ↓
Run Wireshark or TCPDump on network
        ↓
Filter for FTP traffic
        ↓
Username and password visible in capture
```

---

## One Line Summary

```
FTP    = file transfer, plaintext, insecure
SFTP   = FTP over SSH, encrypted, secure
FTPS   = FTP + TLS, encrypted, secure
Anonymous FTP = no credentials needed = always check first in HTB
```
