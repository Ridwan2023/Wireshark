# Lab 06: TLS/HTTPS Handshake

## Objective
Understand how TLS establishes an encrypted connection on top of TCP, and identify the key handshake messages in Wireshark.

## Tools
- Wireshark (latest stable version)
- A network interface with active traffic (Wi-Fi or Ethernet)

## Background
HTTPS is HTTP running over TLS (Transport Layer Security). After the TCP three-way handshake completes, the client and server perform a separate TLS handshake to agree on encryption parameters and exchange keys — before any HTTP data is sent. Wireshark can see the handshake metadata, but not the encrypted application data itself.

## Steps

### 1. Start a Capture
1. Open Wireshark and select your active interface.
2. Start capturing.
3. Visit any HTTPS website in a fresh tab.
4. Stop the capture once the page loads.

### 2. Filter for TLS Traffic
| Filter | Purpose |
|--------|---------|
| `tls` | Show only TLS packets |
| `tls.handshake.type == 1` | Show Client Hello messages |
| `tls.handshake.type == 2` | Show Server Hello messages |

### 3. Inspect the Client Hello
1. Find the packet with `Info` showing "Client Hello".
2. Expand **Transport Layer Security** → **Handshake Protocol: Client Hello**.
3. Note:
   - TLS version proposed
   - Cipher suites offered (a list of encryption algorithm combinations the client supports)
   - Server Name Indication (SNI) — this reveals which domain the client is connecting to, even though later traffic is encrypted

### 4. Inspect the Server Hello
1. Find the following packet with `Info` showing "Server Hello".
2. Note:
   - TLS version and cipher suite the server selected from the client's offered list
   - Certificate message (usually follows shortly after) — this contains the server's public certificate

### 5. Observe the Encrypted Application Data
1. Scroll past the handshake messages to find packets labeled "Application Data".
2. Click one — note that the payload is encrypted and unreadable, unlike the plaintext HTTP lab.

## Observations
*(Fill in after running the capture — e.g. "The SNI field revealed the domain even though the rest of the traffic was encrypted. TLS 1.3 was negotiated.")*

## Key Takeaways
- TLS negotiates encryption parameters before any application data (like HTTP requests) is exchanged.
- The SNI field in Client Hello is sent in plaintext by default, meaning the destination domain is visible to anyone monitoring traffic — even over HTTPS.
- Once the handshake completes, all subsequent data is encrypted and appears as "Application Data" with no readable content in Wireshark.

## Next Lab
Lab 07: ICMP & Ping — examining network diagnostics and error messages.
