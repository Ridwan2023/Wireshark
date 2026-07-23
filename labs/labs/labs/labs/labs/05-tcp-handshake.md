# Lab 05: TCP Handshake

## Objective
Understand how TCP establishes a reliable connection using the three-way handshake, and examine sequence numbers and flags in Wireshark.

## Tools
- Wireshark (latest stable version)
- A network interface with active traffic (Wi-Fi or Ethernet)

## Background
Before any data can be exchanged, TCP establishes a connection through a three-way handshake: SYN (synchronize) → SYN-ACK (synchronize-acknowledge) → ACK (acknowledge). This ensures both sides agree on starting sequence numbers and confirms both directions of the connection are working before data flows.

## Steps

### 1. Start a Capture
1. Open Wireshark and select your active interface.
2. Start capturing.
3. Open a new connection to any website in your browser (a fresh tab/site helps trigger a new handshake rather than reusing an existing connection).
4. Stop the capture after the page starts loading.

### 2. Filter for the Handshake
| Filter | Purpose |
|--------|---------|
| `tcp.flags.syn == 1` | Show SYN and SYN-ACK packets |
| `tcp.port == 443` | Show HTTPS connection traffic |
| `tcp.stream eq 0` | Isolate one specific TCP conversation |

### 3. Identify the Three Packets
Look for this exact sequence, usually the first three packets of a new connection:

1. **SYN** — client → server. `Info` shows `[SYN]`. Client proposes an initial sequence number.
2. **SYN-ACK** — server → client. `Info` shows `[SYN, ACK]`. Server acknowledges the client's sequence number and proposes its own.
3. **ACK** — client → server. `Info` shows `[ACK]`. Client acknowledges the server's sequence number. Connection is now established.

### 4. Inspect the Flags and Sequence Numbers
1. Click the SYN packet, expand **Transmission Control Protocol**.
2. Note:
   - Sequence number (relative, starts at 0 in Wireshark's display)
   - Flags: only SYN set
   - Window size (how much data the sender can receive before needing an ACK)
3. Click the SYN-ACK packet and note the Acknowledgment number — it should be the client's sequence number + 1.

### 5. Observe Connection Teardown (optional)
1. Filter with `tcp.flags.fin == 1` to find connection close packets.
2. Note the FIN/ACK exchange — similar to the handshake but for closing, usually four packets total (FIN, ACK, FIN, ACK) since each side closes independently.

## Observations
*(Fill in after running the capture — e.g. "The handshake completed in under 5ms on the local network. Window size was 64240 bytes.")*

## Key Takeaways
- The three-way handshake (SYN, SYN-ACK, ACK) confirms both directions of a connection before any application data is sent.
- Sequence and acknowledgment numbers let both sides track exactly what's been received, enabling retransmission if packets are lost.
- Wireshark's relative sequence numbers (starting at 0) make it easier to follow a stream, but real sequence numbers are random 32-bit values for security.

## Next Lab
Lab 06: TLS/HTTPS Handshake — examining how encrypted connections are negotiated.
