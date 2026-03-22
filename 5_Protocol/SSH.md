# SSH — Secure Shell


## Overview

SSH (Secure Shell) is a protocol that enables secure system
administration and file transfers over insecure networks.
It is used everywhere — in every data center and large enterprise.

> "Keys are easy to create but hard to manage"
> 
> — one of the most important real world SSH security challenges

---

## How SSH Works — Connection Process

SSH works in a client-server model:

```
1. SSH client initiates contact with SSH server
2. Server sends its public key to client
3. Client and server negotiate parameters
4. Secure encrypted channel is opened
5. User logs into the server host operating system
```

---

## Authentication Methods

### Password Authentication

- Relies on username and password combination
- Simpler but less secure
- Password is verified by the server

### Key Based Authentication

- Uses cryptographic key pairs for verification
- More secure than password authentication

```
Private Key → stays on YOUR machine, never shared
Public Key  → goes on the server

Connection process:
Server encrypts a challenge with your Public Key
Only your Private Key can decrypt it
If it matches → authenticated
Password never sent over network
```

> "Keys are easy to create but hard to manage"
> Managing and protecting private keys is critical.
> A leaked private key = full access to that server.

---

## Basic SSH Commands

| Command         | Purpose                                      |
| --------------- | -------------------------------------------- |
| `ssh user@host` | Connect to remote host                       |
| `ssh-keygen`    | Generate a key pair                          |
| `ssh-copy-id`   | Configure public key as authorized on server |
| `ssh-agent`     | Agent to hold private keys in memory         |
| `ssh-add`       | Add a key to the agent                       |
| `scp`           | Secure file transfer client                  |
| `sftp`          | File transfer client with FTP-like interface |
| `sshd`          | OpenSSH server daemon                        |

---

## SSH Tunneling — Port Forwarding

Also known as SSH port forwarding.
A method of transporting network data over an encrypted SSH connection.

Used for:

- Implementing VPNs
- Accessing intranet services across firewalls
- Securing application traffic
- Hiding attacker activity by bouncing through multiple devices

### Tunneling Types

#### Local Port Forwarding

Forward traffic FROM your machine TO a remote server.

> "Bring their service to me"

```
ssh -L 8080:target:80 user@server
Access target's port 80 through your local port 8080
```

#### Remote Port Forwarding

Forward traffic FROM remote server BACK to your machine.

> "Expose my service to them"

```
ssh -R 8080:localhost:80 user@server
Expose your local port 80 through server's port 8080
```

#### Dynamic Port Forwarding

Acts like a SOCKS proxy — routes ALL traffic through SSH.

> Used with proxychains in HTB to pivot through machines

```
ssh -D 1080 user@server
Route all traffic through SSH as SOCKS proxy
```

---

## One Line Summary

```
SSH  = encrypted secure channel between client and server
Keys = more secure than passwords but must be protected
Tunneling = powerful feature — used by admins and attackers alike
```
