# HTTP, HTTPS, TLS

---

## HTTP — HyperText Transfer Protocol

HTTP is a web based protocol built on TCP, UDP or QUIC.
TCP handles reliable data delivery — HTTP structures that data
into requests and responses so client and server can communicate.

> TCP = delivers the data
>
> HTTP = structures the data
>
> HTTPS = secures the data

---

## HTTP Message Structure

### Request

```
Start Line  → GET /index.html HTTP/1.1
              [Method] [Path] [HTTP Version]
Headers     → field: value pairs
Blank Line  → separates headers and body
Body        → (optional — some methods have no body)
```

### Response

```
Status Line → HTTP/1.1 200 OK
              [HTTP Version] [Status Code] [Status Text]
Headers     → content-type, content-length etc.
Blank Line
Body        → actual content returned
Trailers    → extra headers sent after chunked body
              only present in chunked transfer encoding
```

---

## HTTP Methods

| Method      | What it Does          | Security Relevance                                        |
| ----------- | --------------------- | --------------------------------------------------------- |
| **GET**     | Retrieve resource     | Parameters visible in URL — never put sensitive data here |
| **POST**    | Submit data           | Primary target for injection attacks — SQLi, XSS          |
| **PUT**     | Replace resource      | Misconfigured → attacker can upload malicious files       |
| **DELETE**  | Delete resource       | Misconfigured → attacker can delete server files          |
| **PATCH**   | Partial update        | Same risk as PUT but partial modification                 |
| **HEAD**    | GET without body      | Used for recon — check if resource exists                 |
| **OPTIONS** | Shows allowed methods | Recon — reveals what server allows                        |

```
GET    = read
POST   = create
PUT    = replace
PATCH  = partial update
DELETE = remove
HEAD   = read but no body
OPTIONS= what's allowed
```

> In HTB — always check OPTIONS first to see what methods are enabled.
> If PUT is enabled → possible file upload exploit.

---

## HTTP Status Codes

### Status Code Groups

```
1xx = Informational
2xx = Success
3xx = Redirection
4xx = Client Error
5xx = Server Error
```

### Common Status Codes

| Code    | Meaning                                   | Security Relevance                           |
| ------- | ----------------------------------------- | -------------------------------------------- |
| **200** | OK — request succeeded                    | Resource exists and accessible               |
| **301** | Redirect — resource moved                 | Follow redirect — reveals new location       |
| **400** | Bad Request — malformed request           | Check your exploit syntax                    |
| **401** | Unauthorized — not authenticated          | Try default credentials or brute force       |
| **403** | Forbidden — identity known, no permission | Resource EXISTS — worth probing further      |
| **404** | Not Found                                 | Keep fuzzing — may exist with different path |
| **405** | Method Not Allowed                        | Try different HTTP methods                   |
| **500** | Internal Server Error                     | Server breaking — possible injection point   |

> 403 is exciting in pentesting — it means the resource EXISTS.
> Just need to find a way around the restriction.

---

## HTTP Headers

Headers carry metadata between client and server.
They sit between the start line and body in both requests and responses.

### Authentication Headers

| Header                  | Purpose                                     |
| ----------------------- | ------------------------------------------- |
| **WWW-Authenticate**    | Server tells client what auth method to use |
| **Authorization**       | Client sends credentials to server          |
| **Proxy-Authenticate**  | Auth challenge from proxy server            |
| **Proxy-Authorization** | Client credentials for proxy server         |

### Content Headers

| Header                | Purpose                                    |
| --------------------- | ------------------------------------------ |
| **Content-Type**      | Type of data being sent (json, html, etc.) |
| **Content-Length**    | Size of the body in bytes                  |
| **Transfer-Encoding** | How data is encoded — chunked etc.         |

### Cookie Headers

| Header         | Purpose                                |
| -------------- | -------------------------------------- |
| **Cookie**     | Client sends stored cookies to server  |
| **Set-Cookie** | Server sends cookies to client browser |

### Conditional Headers

| Header            | Purpose                                           |
| ----------------- | ------------------------------------------------- |
| **ETag**          | Unique string identifying version of resource     |
| **If-Match**      | Apply method only if resource matches ETag        |
| **If-None-Match** | Apply method only if resource does not match ETag |

### Header Groups

```
Request Headers        → sent by client
Response Headers       → sent by server
Representation Headers → describe body format
Payload Headers        → describe payload data
```

### Security Relevance of Headers

| Header              | Security Risk                                    |
| ------------------- | ------------------------------------------------ |
| **Authorization**   | Contains auth tokens — target for interception   |
| **Cookie**          | Session hijacking if stolen                      |
| **Set-Cookie**      | Check for missing Secure/HttpOnly/SameSite flags |
| **Content-Type**    | Manipulating can bypass file upload filters      |
| **X-Forwarded-For** | Can be spoofed to bypass IP restrictions         |

---

## Cookies

A cookie is a small piece of data a server sends to a user's browser.
The browser stores it and sends it back with future requests.

### Cookie Security Flags

| Flag         | Purpose                              |
| ------------ | ------------------------------------ |
| **Secure**   | Cookie only sent over HTTPS          |
| **HttpOnly** | Cookie not accessible by JavaScript  |
| **SameSite** | Controls cross site request behavior |

> Missing these flags = vulnerability.
>
> No HttpOnly flag → cookie accessible via JavaScript → XSS can steal it.
>
> No Secure flag → cookie sent over HTTP → MitM can intercept it.

---

## HTTPS

HTTPS is the implementation of TLS encryption on top of HTTP.
Any website using HTTPS is using TLS encryption.

```
HTTP  = plaintext — anyone can read it
HTTPS = encrypted — only client and server can read it
```

---

## TLS — Transport Layer Security

TLS is the successor to SSL, developed from SSL 3.0.

### Version History

```
SSL 2.0 → 1995 — deprecated, insecure
SSL 3.0 → 1996 — deprecated, insecure
TLS 1.0 → 1999 — deprecated
TLS 1.1 → 2006 — deprecated
TLS 1.2 → 2008 — still widely used
TLS 1.3 → 2018 — current standard
```

> Finding SSL or TLS 1.0 on a server in HTB = vulnerability to exploit.

### Where TLS Sits

```
Application Layer (Layer 7) — HTTP
        ↕ TLS encrypts here
Transport Layer (Layer 4)   — TCP
```

### What TLS Does

- Encrypts communication between web applications and servers
- Also encrypts email, messaging, VoIP
- Protects data in transit from interception

### TLS Handshake

Two separate handshakes occur when visiting an HTTPS website:

**Step 1 — TCP Handshake first:**

```
SYN → SYN-ACK → ACK
establishes the connection
```

**Step 2 — TLS Handshake after:**

```
1. Client Hello  → supported TLS versions + cipher suites
2. Server Hello  → agreed TLS version + cipher suite
3. Certificate   → server proves its identity
4. Key Exchange  → both sides generate session keys
5. Finished      → ready to communicate encrypted
6. Encrypted     → all communication now encrypted
```

### Cipher Suite

An agreed set of algorithms for the connection:

```
Key exchange algorithm
Encryption algorithm
Message authentication algorithm

Example: TLS_AES_128_GCM_SHA256
```

### TLS Security Relevance

| Topic                        | Security Risk                                 |
| ---------------------------- | --------------------------------------------- |
| **Old SSL/TLS versions**     | Exploitable — POODLE, BEAST attacks           |
| **Weak cipher suites**       | Downgrade attacks — force weak encryption     |
| **Expired certificates**     | Misconfiguration — common in HTB              |
| **Self signed certificates** | MitM attacks — fake certificates              |
| **SSL Stripping**            | Downgrade HTTPS to HTTP — remove TLS entirely |

---

## One Line Summary

```
TCP   = delivers the data
HTTP  = structures the data
TLS   = encrypts the data
HTTPS = HTTP + TLS together
```
