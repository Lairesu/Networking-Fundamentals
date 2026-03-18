# Proxy ARP (Address Resolution Protocol)

## Overview
Proxy ARP is a technique where a router or network device responds to an ARP request on behalf of another host.

Instead of the actual destination replying, the router provides its own MAC address and forwards the traffic to the real destination.

---

## Key Concept
A host believes the destination IP is on its local network and sends an ARP request.

The router intercepts this request and replies:

- Sender IP = Target IP (pretending to be the destination)
- Sender MAC = Router’s MAC address

The host then sends all traffic to the router, which forwards it to the actual destination.

---

## How It Works (Step-by-Step)

1. Host A wants to reach Host B
2. Host A incorrectly assumes Host B is on the same subnet
3. Host A sends an ARP Request:
   - "Who has IP X.X.X.X?"

4. Router receives the request and recognizes it can reach that IP
5. Router sends an ARP Reply:
   - Provides its own MAC address
   - Uses the target IP as the sender IP

6. Host A updates its ARP table:
   - Maps Host B’s IP to the router’s MAC

7. Host A sends packets to the router
8. Router forwards packets to Host B

---

## When Proxy ARP Is Used

- Hosts without a properly configured default gateway
- Incorrect subnet configurations
- Certain network designs requiring transparent routing

---

## Important Characteristics

- ARP request is still broadcast
- ARP reply is unicast
- Router acts as an intermediary without the host knowing
- The real destination host is not directly visible to the sender

---

## Normal ARP vs Proxy ARP

### Normal ARP
- Host resolves MAC of another host on the same network
- Destination responds directly

### Proxy ARP
- Router responds instead of the destination
- Router forwards the traffic afterward

---

## Security Considerations

- Can obscure network boundaries
- Can be abused similarly to ARP spoofing
- May complicate network troubleshooting

---

## Summary

Proxy ARP allows a router to answer ARP requests on behalf of another host by providing its own MAC address, enabling communication even when the sender has an incorrect view of the network.