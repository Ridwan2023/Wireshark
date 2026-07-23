# Lab 02: ARP & Ethernet

## Objective
Understand how devices resolve IP addresses to MAC addresses using ARP, and examine the structure of Ethernet frames in Wireshark.

## Tools
- Wireshark (latest stable version)
- A network interface with active traffic (Wi-Fi or Ethernet)

## Background
Before two devices on the same local network can communicate, the sending device needs to know the destination's MAC address. ARP (Address Resolution Protocol) handles this by broadcasting a request ("Who has this IP?") and receiving a reply with the MAC address. Ethernet frames then carry that MAC addressing at Layer 2.

## Steps

### 1. Start a Capture
1. Open Wireshark and select your active interface.
2. Start capturing.
3. Trigger some ARP traffic — the easiest way is to clear your ARP cache and then ping a device on your local network:
   - Windows: `arp -d *` then `ping <local IP, e.g. your router>`
   - Linux/Mac: `sudo ip -s -s neigh flush all` then `ping <local IP>`
4. Stop the capture after a few seconds.

### 2. Filter for ARP Traffic
Use this filter in the filter bar:

| Filter | Purpose |
|--------|---------|
| `arp` | Show only ARP requests and replies |
| `eth.addr == <MAC address>` | Show frames to/from a specific MAC |

### 3. Inspect an ARP Request
1. Find a packet with `Info` showing "Who has ... Tell ...".
2. Expand the **Address Resolution Protocol** section in the packet details.
3. Note:
   - Sender MAC and IP
   - Target IP (target MAC will be `00:00:00:00:00:00` since it's unknown)
   - Opcode: `request (1)`

### 4. Inspect the Matching ARP Reply
1. Find the following packet with `Info` showing "... is at ...".
2. Note:
   - Sender MAC and IP (this is the device that owns the resolved IP)
   - Opcode: `reply (2)`

### 5. Examine the Ethernet Frame
1. Click any packet and expand the **Ethernet II** section.
2. Note:
   - Destination MAC address
   - Source MAC address
   - Type field (e.g. `0x0806` for ARP, `0x0800` for IPv4)

## Observations
*(Fill in after running the capture — e.g. "The ARP request was broadcast to ff:ff:ff:ff:ff:ff, and the reply came directly from the target's MAC address.")*

## Key Takeaways
- ARP requests are broadcast to the entire local network (destination MAC `ff:ff:ff:ff:ff:ff`); replies are unicast directly to the requester.
- Every Ethernet frame carries a Type field indicating which protocol it's carrying inside (ARP, IPv4, IPv6, etc.).
- ARP only operates within the local network segment — it doesn't cross routers.

## Next Lab
Lab 03: DNS Analysis — examining domain name resolution queries and responses.
