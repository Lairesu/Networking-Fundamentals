# DNS – Domain Name System

## Overview

DNS is the phonebook of the internet. Humans access websites through
domain names like google.com but browsers interact through IP addresses.
DNS translates domain names into IP addresses so browsers can load
internet resources.

> Every device on the internet has an IP address — just like a real
> world address used to find a specific location. DNS removes the need
> for humans to memorize these addresses.

---

## How DNS Works

DNS converts human-readable hostnames into machine-friendly IP addresses.
When a user wants to load a webpage, a translation occurs between what
the user types into their browser and the IP address needed to locate
that webpage.

A **DNS Query** is the formal request sent behind the scenes by your
browser asking:

> "What is the IP address for this domain name?"

---

## The 4 DNS Servers Involved in Loading a Webpage

### 1. DNS Recursor (The Librarian)

- First point of contact for the client's DNS query
- Receives queries from client machines through applications like
  web browsers
- Responsible for making additional requests to satisfy the
  client's DNS query
- Does all the leg work on behalf of the client

> Think of it as a librarian who is asked to find a particular book
> in a library — they go and do the searching for you.

---

### 2. Root Nameserver (The Index)

- First step in translating human-readable hostnames into IP addresses
- Does not know the final answer but knows where to look next
- Points the recursor toward the correct TLD Nameserver

> Think of it as the index of a library that points to different
> racks of books — a reference to more specific locations.

---

### 3. TLD Nameserver — Top Level Domain (The Rack of Books)

- Hosts the last portion of a hostname — the domain extension
- Different TLD servers handle different extensions:

| TLD  | Example       |
| ---- | ------------- |
| .com | google.com    |
| .org | wikipedia.org |
| .net | speedtest.net |
| .dk  | denmark.dk    |
| .np  | nepal.np      |
| .jp  | japan.jp      |

> Think of it as a specific rack of books — each rack holds
> books for a specific category (.com, .org, .net etc.)

---

### 4. Authoritative Nameserver (The Dictionary)

- Final stop in the DNS query journey
- Has the actual IP address for the requested domain
- If the record exists, it returns the IP address back to
  the DNS Recursor
- The Recursor then returns it to the client

> Think of it as the dictionary on the rack — where the specific
> name is finally translated into its definition (IP address)

---

## The Full DNS Resolution Flow

```
You type google.com in browser
            ↓
DNS Recursor — "I'll find that for you"
            ↓
Root Nameserver — "Try the .com TLD server"
            ↓
TLD Nameserver — "Try Google's Authoritative server"
            ↓
Authoritative Nameserver — "google.com = 142.250.80.46"
            ↓
DNS Recursor returns IP to your browser
            ↓
Browser connects to 142.250.80.46
```

---

## Fantasy Analogy

- You = traveler who knows the name of a treasure location
- IP Address = coordinates to teleport there
- DNS Recursor = librarian who searches on your behalf
- Root Nameserver = robot with index of all library sections
- TLD Nameserver = robot pointing to the right shelf by
  last part of name (treasure.desert → desert shelf)
- Authoritative Nameserver = dictionary on that shelf with
  the exact coordinates
- Browser loading = teleporting to the treasure

---

### Authoritative DNS server vs Recursive DNS resolver

Recursive Resolver = Searches for the answer
Authoritative Server = Has the answer

### Steps in a DNS lookup:

user/host types **mangadex.com**

```bash
1. You type mangadex.com
         ↓
2. DNS Recursive Resolver receives query
         ↓
3. Resolver asks Root Nameserver
         ↓
4. Root says "ask the .com TLD server"
         ↓
5. TLD says "ask mangadex's Authoritative Nameserver"
         ↓
6. Authoritative Nameserver returns IP
         ↓
7. Resolver returns IP to your browser
         ↓
8. Browser makes HTTP request to that IP
         ↓
9. Webpage loads
```

The steps are skipped from the DNS lookup process when the information is cached which makes it quicker to load the website.

## DNS Query Types

In an ideal situation cached record data will be available, allowing
DNS name server to return a non-recursive query — reducing the full
resolution process entirely.

---

### 1. Recursive Query

The client demands a full answer from the resolver.

> "Find the complete answer and don't come back until you have it"

- Resolver does ALL the work on behalf of the client
- Returns either the requested record or an error message

---

### 2. Iterative Query

Each server points to the next — resolver does the hopping itself.

> "I'll tell you who to ask next, but YOU go ask them yourself"

```
Resolver → Root Nameserver → "ask TLD"
Resolver → TLD Nameserver → "ask Authoritative"
Resolver → Authoritative Nameserver → IP returned
```

---

### 3. Non-Recursive Query

Cached answer — no searching needed.

> "I already have it, no need to search"

- Server returns cached DNS records immediately
- Reduces bandwidth consumption
- Speeds up the resolution process significantly

---

### One Line Summary

```bash
Recursive     = "find the full answer for me"
Iterative     = "point me to who knows next"
Non-Recursive = "I already have it cached"
```

---

### Security Note

DNS Cache Poisoning exploits non-recursive queries — if an attacker
poisons the cache, every cached response returns a fake IP without
ever verifying again.

## What is DNS Caching?

The purpose of caching is to temporarily store data in a location that
results in improvements in performance and reliability for data requests.

DNS caching involves storing data closer to the requesting client so that
the DNS query can be resolved earlier and additional queries further down
the DNS lookup chain can be avoided — improving load times and reducing
bandwidth/CPU consumption.

DNS records can be cached in a variety of locations, each storing records
for a set amount of time determined by **TTL (Time To Live).**

> Short TTL = more DNS lookups but fresher data
> Long TTL = fewer lookups but risk of stale/poisoned data

---

## Where Does DNS Caching Occur?

### 1. Browser DNS Cache

Modern web browsers cache DNS records by default for a set amount of time.
The closer the DNS caching occurs to the browser, the fewer processing
steps needed to resolve a request.

> Check Chrome's DNS cache at: `chrome://net-internals/#dns`

---

### 2. Operating System Level DNS Cache (Stub Resolver)

The second and last local stop before a DNS query leaves the machine.

The OS process that handles this is called a **Stub Resolver** (DNS Client):

- Checks its own local cache first
- If record found → returns it immediately
- If not found → forwards query to ISP's recursive resolver

> Called "stub" because it does minimal work — just checks local
> cache and forwards. It does not perform the full recursive lookup itself.

---

### 3. ISP Recursive Resolver Cache

When the ISP recursive resolver receives the query:

- Checks its own local persistence layer first
- If found → returns cached answer immediately
- If not found → begins full DNS lookup

---

## Full Caching Chain

```bash
Browser Cache                → checked first
        ↓ not found
OS Stub Resolver Cache       → checked second
        ↓ not found
ISP Recursive Resolver Cache → checked third
        ↓ not found
Full DNS Lookup Begins       → Root → TLD → Authoritative
```

---

## Security: DNS Cache Poisoning

DNS caching is a primary attack surface for DNS-based attacks.

| Level Poisoned     | Impact                               |
| ------------------ | ------------------------------------ |
| Browser Cache      | Affects only your browser            |
| OS Cache           | Affects all browsers on your machine |
| ISP Resolver Cache | Affects everyone using that ISP      |

**How it works:**
An attacker injects a fake IP address into a DNS cache. Every cached
response then returns the fake IP without verifying — redirecting
victims to malicious servers without their knowledge.

**The higher up the chain the poisoning occurs — the more victims.**

**Prevention:**

- DNSSEC — cryptographically signs DNS records to verify authenticity
- DNS over HTTPS (DoH) — encrypts DNS queries
- Short TTL values on critical records
- Network monitoring for unusual DNS responses

## DNS Record Types

### Record Types

| Record    | Full Name         | What it Does                                          |
| --------- | ----------------- | ----------------------------------------------------- |
| **A**     | Address Record    | Maps domain to IPv4 address                           |
| **AAAA**  | Address Record    | Maps domain to IPv6 address                           |
| **CNAME** | Canonical Name    | Maps domain to another domain name (alias)            |
| **MX**    | Mail Exchange     | Points to mail server for the domain                  |
| **TXT**   | Text Record       | Stores text info — used for verification and security |
| **NS**    | Nameserver Record | Points to authoritative nameserver for the domain     |

---

### Cybersecurity Relevance

| Record    | Why It Matters in HTB/Pentesting                        |
| --------- | ------------------------------------------------------- |
| **A**     | Finding real IP behind a domain                         |
| **AAAA**  | Same but IPv6 — often forgotten and misconfigured       |
| **CNAME** | Subdomain enumeration — reveals hidden services         |
| **MX**    | Email server recon — target for phishing setup          |
| **TXT**   | Can leak sensitive info — API keys, verification tokens |
| **NS**    | Finding nameservers — target for DNS zone transfers     |

> **DNS Zone Transfer** — a famous attack where you query the NS record
> and dump ALL DNS records of a target at once. Common in HTB machines.

---

_Module 5 – Network Protocols | DNS_
_Part of: Solidifying Networking for Cybersecurity_
