# Address Resolution Protocol (ARP)

An **IP address** is the logical addressing scheme for nodes on the **Network Layer (L3)** of the OSI model and helps facilitate the goal of end-to-end delivery.
A **MAC address** is the physical addressing scheme for individual NIC cards on each node of a network. MAC addresses exist at the **Data Link Layer (L2)** and help facilitate the goal of **hop-to-hop delivery.**

The **Address Resolution Protocol (ARP)** bridges these two addressing schemes by mapping a known IP address (L3) to an unknown MAC address (L2).
The purpose of this mapping is so that a packet’s **L2 header** can be correctly populated to deliver the packet to the next NIC in the path between two endpoints.
