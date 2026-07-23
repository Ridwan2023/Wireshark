# Lab 03: DNS Analysis

## Objective
Understand how DNS resolves domain names to IP addresses, and examine DNS query/response structure in Wireshark.

## Tools
- Wireshark (latest stable version)
- A network interface with active traffic (Wi-Fi or Ethernet)

## Background
Before a browser can connect to a website, it needs the site's IP address. DNS (Domain Name System) handles this translation. A client sends a DNS query to a resolver (usually your ISP or a public DNS server like 8.8.8.8), which replies with the matching IP address(es).

## Steps

### 1. Start a Capture
1. Open Wireshark and select your active interface.
2. Start capturing.
3. Flush your local DNS cache so a fresh lookup happens:
   - Windows: `ipconfig /flushdns`
   - Linux: `sudo systemd-resolve --flush-caches` (or restart `nscd`/`dnsmasq` depending on distro)
   - Mac: `sudo dscacheutil -flushcache`
4. Visit a new website in your browser (pick one you haven't visited recently).
5. Stop the capture after a few seconds.

### 2. Filter for DNS Traffic
| Filter | Purpose |
|--------|---------|
| `dns` | Show only DNS queries and responses |
| `dns.qry.name == "example.com"` | Show DNS traffic for a specific domain |
| `dns.flags.response == 0` | Show only queries (not responses) |
| `dns.flags.response == 1` | Show only responses |

### 3. Inspect a DNS Query
1. Find a packet with `Info` showing "Standard query ... A ...".
2. Expand **Domain Name System (query)** in the packet details.
3. Note:
   - Transaction ID
   - Query name (the domain being looked up)
   - Query type (e.g. `A` for IPv4, `AAAA` for IPv6)

### 4. Inspect the Matching DNS Response
1. Find the following packet with `Info` showing "Standard query response ...".
2. Expand **Domain Name System (response)**.
3. Note:
   - Same Transaction ID as the query (this is how client matches response to request)
   - Answer section — the resolved IP address(es)
   - TTL (Time To Live) — how long the answer can be cached

### 5. Compare Transport Protocol
1. Check the protocol column for these packets — DNS normally runs over **UDP port 53**.
2. If you see DNS over TCP, note it — this usually happens for large responses or zone transfers.

## Observations
*(Fill in after running the capture — e.g. "The query for example.com used UDP, resolved in under 50ms, with a TTL of 300 seconds.")*

## Key Takeaways
- DNS primarily uses UDP for speed, falling back to TCP only when responses are too large for a single UDP packet.
- The Transaction ID links each query to its response, since UDP doesn't guarantee response order.
- TTL values tell resolvers (and browsers) how long to cache an answer before looking it up again.

## Next Lab
Lab 04: HTTP Traffic — examining request/response headers and payloads.
