# Lab 04: HTTP Traffic

## Objective
Understand the structure of HTTP requests and responses, and examine headers and payload data in Wireshark.

## Tools
- Wireshark (latest stable version)
- A network interface with active traffic (Wi-Fi or Ethernet)

## Background
HTTP (Hypertext Transfer Protocol) is how browsers and web servers communicate — a client sends a request (method, headers, sometimes a body) and the server sends back a response (status code, headers, and content). Since HTTP is unencrypted, Wireshark can show the full content — unlike HTTPS, where payloads are encrypted.

## Steps

### 1. Start a Capture
1. Open Wireshark and select your active interface.
2. Start capturing.
3. Visit an HTTP (not HTTPS) website — many test sites like `http://neverssl.com` exist specifically for this, since most modern sites force HTTPS.
4. Stop the capture after the page loads.

### 2. Filter for HTTP Traffic
| Filter | Purpose |
|--------|---------|
| `http` | Show only HTTP requests and responses |
| `http.request` | Show only requests |
| `http.response` | Show only responses |
| `http.request.method == "GET"` | Show only GET requests |

### 3. Inspect an HTTP Request
1. Find a packet with `Info` showing `GET / HTTP/1.1`.
2. Expand **Hypertext Transfer Protocol** in the packet details.
3. Note:
   - Request method (GET, POST, etc.)
   - Host header (which domain is being requested)
   - User-Agent header (identifies the browser/OS)

### 4. Inspect the Matching HTTP Response
1. Find the following packet with `Info` showing something like `HTTP/1.1 200 OK`.
2. Expand **Hypertext Transfer Protocol** in the response.
3. Note:
   - Status code (200 OK, 404 Not Found, 301 Redirect, etc.)
   - Content-Type header (e.g. `text/html`, `image/png`)
   - Content-Length

### 5. Follow the TCP Stream
1. Right-click any HTTP packet → **Follow → HTTP Stream**.
2. This reassembles the full request and response as readable text, showing the raw headers and body together.

## Observations
*(Fill in after running the capture — e.g. "The GET request returned a 200 OK with Content-Type text/html, and the full page HTML was visible in plaintext.")*

## Key Takeaways
- HTTP sends everything in plaintext, including headers and body — this is a key reason HTTPS is now the default for the web.
- The Host header lets one server respond to requests for multiple domains from the same IP.
- "Follow HTTP Stream" is one of the most useful Wireshark features for reconstructing a full request/response exchange.

## Next Lab
Lab 05: TCP Handshake — examining the SYN/SYN-ACK/ACK sequence and connection flags.
