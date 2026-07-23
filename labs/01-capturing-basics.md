# Lab 01: Capturing Basics

## Objective
Learn how to start a packet capture in Wireshark, apply basic filters, and identify key traffic types on the network.

## Tools
- Wireshark (latest stable version)
- A network interface with active traffic (Wi-Fi or Ethernet)

## Steps

### 1. Start a Capture
1. Open Wireshark.
2. Select your active network interface (e.g. Wi-Fi, Ethernet).
3. Click the blue shark fin icon to start capturing.
4. Let it run for 1–2 minutes while browsing a couple of websites.
5. Click the red square icon to stop the capture.

### 2. Apply Basic Display Filters
Try each of these in the filter bar and observe the results:

| Filter | Purpose |
|--------|---------|
| `http` | Show only HTTP traffic |
| `dns` | Show only DNS queries/responses |
| `tcp.port == 443` | Show HTTPS traffic |
| `ip.addr == <your IP>` | Show traffic to/from your machine |

### 3. Inspect a Packet
1. Click on any packet in the list.
2. Expand the panels below (Frame, Ethernet, IP, TCP/UDP, and protocol layer).
3. Note the following for one packet:
   - Source and destination IP
   - Source and destination port
   - Protocol
   - Packet length

## Observations
*(Fill in after running the capture — e.g. "Most traffic was TCP on port 443, indicating HTTPS. DNS queries resolved before each new site load.")*

## Key Takeaways
- Wireshark captures raw traffic at the packet level, showing every protocol layer.
- Display filters narrow results without affecting the underlying capture.
- Most modern web traffic is encrypted (HTTPS/TCP 443), so payload data is not visible — only headers and metadata.

## Next Lab
Lab 02: ARP & Ethernet — examining address resolution and frame structure.
