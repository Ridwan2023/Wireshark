# Navigate to your repo (clone first if needed)
cd Wireshark

# Make sure the labs folder exists
mkdir -p labs

# Write the file content directly
cat > labs/08-icmp-traceroute.md << 'EOF'
# Lab 08: ICMP & Traceroute Analysis

## Objective
Understand how ICMP (Internet Control Message Protocol) is used for network diagnostics, and analyze how `ping` and `traceroute` work at the packet level using Wireshark.

## Tools
- Wireshark
- Terminal (ping, traceroute/tracert)

## Background
ICMP doesn't carry application data — it carries control and error messages between network devices. The two most common uses are:
- **Echo Request/Reply** (`ping`) — tests reachability
- **Time Exceeded** messages — used by `traceroute` to map the path packets take

## Steps

### Part 1: Capturing a Ping
1. Start a Wireshark capture on your active interface.
2. Run `ping google.com` (or any reachable host) from a terminal.
3. Stop the capture after a few replies come back.
4. Filter with: `icmp`

**What to observe:**
- Echo Request (Type 8) followed by Echo Reply (Type 0)
- The `Identifier` and `Sequence Number` fields — used to match requests to replies
- Time-to-Live (TTL) field in the IP header

### Part 2: Capturing a Traceroute
1. Start a new capture.
2. Run `traceroute google.com` (Linux/Mac) or `tracert google.com` (Windows).
3. Stop the capture once it completes.
4. Filter with: `icmp || (udp && ip.ttl < 5)`

**What to observe:**
- On Linux/Mac, traceroute typically uses UDP packets with increasing TTL values
- On Windows, `tracert` uses ICMP Echo Requests with increasing TTL
- Each hop that can't forward the packet (TTL expired) sends back an ICMP **Time Exceeded** (Type 11) message
- The source IP of each Time Exceeded message reveals one hop on the path

### Part 3: Mapping the Path
1. In Wireshark, filter: `icmp.type == 11`
2. List the source IP of each Time Exceeded packet in order — this reconstructs the route your traffic took.
3. Compare this list to the terminal output of your traceroute command.

## Key Filters
| Filter | Purpose |
|---|---|
| `icmp` | Show all ICMP traffic |
| `icmp.type == 8` | Echo Requests only |
| `icmp.type == 0` | Echo Replies only |
| `icmp.type == 11` | Time Exceeded (traceroute hops) |
| `ip.ttl < 5` | Packets with low TTL (early hops) |

## Analysis Questions
1. What is the purpose of the Identifier and Sequence Number fields in ICMP Echo packets?
2. Why does traceroute increment the TTL value with each probe?
3. What's the difference between how Linux and Windows implement traceroute at the protocol level?
4. Why might some hops show up as `* * *` (no reply) in a real traceroute?

## Security Note
ICMP is often rate-limited or blocked by firewalls, which is why some traceroute hops appear unresponsive. Excessive ICMP traffic can also indicate reconnaissance activity (e.g., ping sweeps) — a topic covered further in the network scanning/detection labs.
EOF

# Stage, commit, and push
git add labs/08-icmp-traceroute.md
git commit -m "Add Lab 08: ICMP & Traceroute Analysis"
git push origin main
