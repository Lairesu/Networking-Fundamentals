# Traditional Address Resolution Protocol (ARP)

ARP is the process by which known L3 address is mapped to an unknown L2 address.

The purpose for creating such a mapping is so packet's L2 header can be properly populated to deliver a packet to the next NIC in the path between two end points.

The "next NIC" in the path will become the target of the ARP request

For example:

```bash
 ARP Target Example

| Scenario                        | ARP Target                    |
|---------------------------------|-------------------------------|
| Host → Host (same network)       | Destination Host IP           |
| Host → Host (different network)  | Default Gateway IP            |
| Router → Host                    | Host IP                       |
| Router → Next Router             | Next Router’s Interface IP    |
```

If a host is speaking to another host on the same IP network, the target of ARP request will be the other host's IP address. Whereas, if the host is speaking to another host on a different IP network, the target for the ARP request will be the Default Gateway's IP address.

Similarly, If a Router is delivering a packet to the destination host, the Router's ARP target will be the Host's IP address. If Router is delivering a packet to the next Router in the path to the host, the ARP target will be the other Router's Interface IP address.

## The ARP Process

ARP is a two-step process: Request and Response.

1. ARP Request (Broadcast)

- Initiator sends an ARP request to all nodes using the **broadcast MAC address** (`FF:FF:FF:FF:FF:FF`) because the target MAC is unknown.
- Every node receives it and checks if it is the intended target.
- Non-target nodes silently discard the request.

Example:

```zsh
PC1 wants to reach 192.168.1.10
ARP Request: "Who has 192.168.1.10? Tell 192.168.1.5"
Broadcast to: FF:FF:FF:FF:FF:FF
```

2. ARP Response (Unicast)

- The target host knows the sender and sends its MAC address back directly (unicast) to the initiator.
- The sender caches this IP → MAC mapping in its ARP table for future use.

Example:

```bash
PC2 has IP 192.168.1.10, MAC AA:BB:CC:DD:EE:FF
ARP Response (Unicast): "192.168.1.10 is at AA:BB:CC:DD:EE:FF"
PC1 stores this mapping in ARP table
```

> Key Points
>
> ARP is always used to find the next hop, never the final destination directly.
>
> The ARP table stores IP → MAC mappings.
>
> Commands to check ARP table:
>
> Windows: arp -a
>
> Linux/macOS: arp -n

