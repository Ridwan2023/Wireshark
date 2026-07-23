# Lab 07: Port Scanning & Detection

## Objective
Understand what port scanning traffic looks like at the packet level, and learn to recognize scan patterns in Wireshark.

## ⚠️ Important — Scope and Ethics
Only scan systems you own or have explicit written permission to test (e.g. your own home lab machine, a VM you control, or a purpose-built test target like `scanme.nmap.org`, which Nmap's creators provide specifically for practice). Scanning systems you don't own or lack permission for is illegal in most jurisdictions, even if unintentional.

## Tools
- Wireshark (latest stable version)
- Nmap (network scanner) — install via `sudo apt install nmap` (Linux), `brew install nmap` (Mac), or the Windows installer from nmap.org
- A test target: a VM on your own network, or `scanme.nmap.org` (Nmap's official public test target)

## Steps

### 1. Start a Capture
1. Open Wireshark and select your active interface.
2. Start capturing.

### 2. Run a Basic TCP Connect Scan
In a terminal:
```bash
nmap -sT scanme.nmap.org
```
This performs a full TCP handshake against common ports — the least stealthy but most reliable scan type.

Stop the capture once the scan finishes.

### 3. Filter and Observe the Pattern
| Filter | Purpose |
|--------|---------|
| `tcp.flags.syn==1 and tcp.flags.ack==0` | Show only SYN packets sent by the scanner |
| `ip.addr == <target IP>` | Isolate all traffic to/from the scanned host |

Notice:
- Many SYN packets sent rapidly to **different destination ports**, all from the same source
- For open ports: SYN → SYN-ACK → ACK (full handshake), then often an immediate RST (scan moves on rather than using the connection)
- For closed ports: SYN → RST-ACK (immediate rejection, no handshake completes)

### 4. Try a SYN (Stealth) Scan for Comparison
```bash
sudo nmap -sS scanme.nmap.org
```
Capture this separately and compare:
- Open ports: SYN → SYN-ACK → **RST** from scanner (never completes the handshake — this is why it's called "stealth" or "half-open")
- This generates less log noise on some systems since the connection is never fully established

### 5. Identify Scan Signatures
Key indicators that traffic is a scan rather than normal browsing:
- One source IP contacting many destination ports in a short time window
- High proportion of RST/RST-ACK responses
- Sequential or patterned port numbers (e.g. 1, 2, 3... or common port lists)
- No corresponding DNS lookup before the connection attempts (a browser looks up a name first; a scanner usually goes straight to an IP)

## Observations
*(Fill in after running your scan — e.g. "The SYN scan hit 1000 ports in under 3 seconds, with 3 ports responding SYN-ACK (open) and the rest RST-ACK (closed).")*

## Key Takeaways
- Scan traffic has a distinctive shape: one source, many destination ports, rapid timing, and a high ratio of incomplete/reset connections.
- A full connect scan (`-sT`) completes handshakes and looks closer to normal traffic; a SYN scan (`-sS`) never finishes them, which is faster and stealthier but still detectable.
- This pattern-recognition skill is the foundation of intrusion detection systems (IDS) that flag scanning behavior automatically.

## Next Lab
Lab 08: UDP Traffic & Streaming — examining connectionless protocol behavior.
