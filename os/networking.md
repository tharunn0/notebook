# Computer Networking: Architecture, Protocols & Mechanics

A comprehensive guide detailing fundamental networking concepts, layered architectural models, transport protocols, and modern web communications at systems depth.

---

## Index

1. [What is a Computer Network, and how does it facilitate distributed communication?](#what-is-a-computer-network-and-how-does-it-facilitate-distributed-communication)
2. [What is the OSI Model, and what are the responsibilities of its seven layers?](#what-is-the-osi-model-and-what-are-the-responsibilities-of-its-seven-layers)
3. [What are Network Protocols, what types exist, and how do TCP and UDP introduce Transport Layer communication?](#what-are-network-protocols-what-types-exist-and-how-do-tcp-and-udp-introduce-transport-layer-communication)
4. [How does TCP work under the hood? (Deep Dive into Mechanics, State Machines, Flow & Congestion Control)](#how-does-tcp-work-under-the-hood-deep-dive-into-mechanics-state-machines-flow--congestion-control)
5. [How does UDP work under the hood, and what are its core mechanics and use cases?](#how-does-udp-work-under-the-hood-and-what-are-its-core-mechanics-and-use-cases)
6. [How do Web Protocols function? (HTTP Evolution, HTTPS, and TLS Mechanics)](#how-do-web-protocols-function-http-evolution-https-and-tls-mechanics)

---

## What is a Computer Network, and how does it facilitate distributed communication?

A computer network is an interconnected infrastructure of autonomous computing nodes, switches, routers, and transmission media that exchange digital data using standardized communication protocols.

It serves as the communication backbone for distributed systems, allowing isolated processes across distinct hardware to coordinate state, stream telemetry, and execute remote procedure calls (RPCs).

### Packet Switching & Forwarding Mechanics
- **Packet Switching**: Data payloads are broken into discrete, independently addressed datagrams or frames.
- **Hardware Forwarding**: Layer 2 switches and Layer 3 routers forward packets at line-rate using Application-Specific Integrated Circuits (ASICs) and Ternary Content-Addressable Memory (TCAM).
- **Routing Protocols**:
  - *Inter-Domain*: Border Gateway Protocol (BGP) computes global paths based on autonomous system (AS) path vectors and routing policies.
  - *Intra-Domain*: Open Shortest Path First (OSPF) synchronizes link-state topology databases using Dijkstra’s algorithm.
- **Host OS Network Stack**: In Linux, the kernel manages packets via socket buffers (`sk_buff`), reading from Network Interface Card (NIC) DMA ring buffers with hardware interrupt coalescing and poll-mode drivers (NAPI).

### Data Center Topology & Virtualization
- **Leaf-Spine (Clos) Fabrics**: Modern data centers replace hierarchical topologies with leaf-spine fabrics to maximize east-west bisection bandwidth via Equal-Cost Multi-Path (ECMP) routing.
- **Network Virtualization**: Software-Defined Networking (SDN) decouples the control plane from the data plane, enabling multi-tenant overlay networks using VXLAN or Geneve in platforms like Kubernetes.

---

## What is the OSI Model, and what are the responsibilities of its seven layers?

The Open Systems Interconnection (OSI) reference model is a 7-layer architectural framework standardizing communication protocols, encapsulation boundaries, and hardware-software interaction.

### Encapsulation & The 7 Layers
Data moves down the stack during transmission, with each layer wrapping the payload in a layer-specific Protocol Data Unit (PDU) header, reversing the process upon reception:

1. **Layer 1 - Physical**: Transmits raw, unstructured bitstreams over copper, optical fiber, or RF media. Defines voltage levels, pinouts, and transceivers (e.g., SFP+, 100GBASE-LR4).
2. **Layer 2 - Data Link**: Delivers **frames** between nodes on the same physical link. Manages 48-bit MAC addressing, media access arbitration, CRC frame check sequences, and VLAN tagging (IEEE 802.1Q).
3. **Layer 3 - Network**: Routes **packets** across interconnected networks. Handles logical IP addressing (IPv4/IPv6), path determination (BGP, OSPF), address resolution (ARP/NDP), and packet fragmentation.
4. **Layer 4 - Transport**: Manages process-to-process communication using 16-bit port numbers. Provides segmentation, connection state, flow control, and reliability (TCP **segments** or UDP **datagrams**).
5. **Layer 5 - Session**: Manages dialogues, checkpoints, and stream recovery between applications (e.g., RPC sessions, NetBIOS).
6. **Layer 6 - Presentation**: Handles data formatting, character set translation (UTF-8/ASCII), compression, and serialization (JSON, Protocol Buffers).
7. **Layer 7 - Application**: Exposes high-level protocols directly to software applications (e.g., HTTP/HTTPS, gRPC, DNS, SSH).

### Engineering Applications
- **Troubleshooting**: Triage outages systematically from L1 (cable/link carrier) $\to$ L2 (ARP/VLANs) $\to$ L3 (IP routing/ping) $\to$ L4 (port listening/SYN handshake) $\to$ L7 (HTTP status codes).
- **Load Balancing**:
  - *Layer 4 (NLB, IPVS)*: Forwards raw TCP/UDP packets at wire speed based on IP/port tuples without decrypting or inspecting payloads.
  - *Layer 7 (Envoy, Nginx, ALB)*: Terminates TCP/TLS connections to inspect HTTP headers, URIs, and cookies for intelligent path routing.

---

## What are Network Protocols, what types exist, and how do TCP and UDP introduce Transport Layer communication?

A network protocol defines the rules, message formats, state machines, and error-handling semantics that enable distinct computing nodes to communicate predictably.

### Protocol Classifications
- **Connection-Oriented vs. Connectionless**: Connection-oriented protocols establish synchronized state via handshakes before data transfer (TCP); connectionless protocols transmit isolated datagrams immediately without setup (UDP).
- **Reliable vs. Best-Effort**: Reliable protocols use acknowledgments, checksums, and retransmissions to guarantee loss-free, in-order delivery; best-effort protocols silently discard dropped or corrupted packets.
- **Stateful vs. Stateless**: Stateful protocols maintain session state across requests; stateless protocols treat each transaction as independent.
- **Routing Paradigms**: Unicast (one-to-one), multicast (one-to-many), and anycast (one-to-nearest via BGP).

### Transport Layer Multiplexing (TCP vs. UDP)
The transport layer uses 16-bit port numbers (0–65535) to direct network flows to specific application sockets:
- **TCP**: Full-duplex, reliable, connection-oriented byte stream with ordering, flow control, and congestion avoidance. Mandatory for transactional data, database replication, and file transfer.
- **UDP**: Lightweight, connectionless, unreliable datagram service with minimal 8-byte headers. Preferred for real-time media, gaming, DNS, and telemetry, where latency is prioritized over retransmissions.

---

## How does TCP work under the hood? (Deep Dive into Mechanics, State Machines, Flow & Congestion Control)

TCP (RFC 793) provides reliable, ordered, error-checked delivery of a continuous stream of bytes over packet-switched IP networks.

### TCP Header Layout
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Data |           |U|A|P|R|S|F|                               |
| Offset| Reserved  |R|C|S|S|Y|I|            Window             |
|       |           |G|K|H|T|N|N|                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |         Urgent Pointer        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### State Machine & Handshakes
- **Three-Way Handshake (Establishment)**:
  1. Client sends `SYN` with Initial Sequence Number ($\text{ISN}_C$).
  2. Server returns `SYN-ACK` with its own $\text{ISN}_S$ and acknowledges client (`ACK` = $\text{ISN}_C + 1$).
  3. Client sends `ACK` ($\text{ISN}_S + 1$), transitioning to `ESTABLISHED`. Negotiates MSS, SACK, and Window Scale.
- **Four-Way Handshake (Teardown)**:
  1. Initiator sends `FIN` $\to$ Peer responds with `ACK` (initiator enters `FIN_WAIT_2`, peer enters `CLOSE_WAIT`).
  2. Peer completes outbound writes and sends its own `FIN` $\to$ Initiator returns `ACK` and enters `TIME_WAIT`.
  3. **`TIME_WAIT` ($2 \times \text{MSL}$)**: Stays open for ~60s to ensure delayed packets drain from routers and verify peer received the final ACK.

### Flow & Congestion Control
- **Flow Control (Sliding Window)**: The receiver advertises its available buffer space (`rwnd`). The sender never transmits more unacknowledged bytes than `rwnd`. If `rwnd = 0`, sender halts and sends periodic zero-window probes.
- **Congestion Control**: Sender maintains an internal Congestion Window (`cwnd`), limiting in-flight data to $\min(\text{cwnd}, \text{rwnd})$.
  1. *Slow Start*: Exponentially doubles `cwnd` every RTT until reaching `ssthresh`.
  2. *Congestion Avoidance*: Linearly increases `cwnd` by 1 MSS per RTT (Additive Increase).
  3. *Fast Retransmit & Recovery*: On receiving 3 duplicate ACKs, infers packet loss immediately without waiting for a retransmission timeout (RTO), scales `cwnd` by half (Multiplicative Decrease), and retransmits.
  4. *Modern Algorithms*: **CUBIC** (Linux default; optimizes for high-bandwidth/latency pipes) and **BBR** (models bottleneck bandwidth and min RTT to prevent bufferbloat).

### Production Tuning & Pitfalls
- **`TIME_WAIT` Port Exhaustion**: Mitigated with `net.ipv4.tcp_tw_reuse = 1`.
- **SYN Floods**: Protected via SYN Cookies (`net.ipv4.tcp_syncookies = 1`), hashing connection state into sequence numbers to avoid allocating kernel memory until the final ACK.
- **Transport Head-of-Line (HOL) Blocking**: Dropping one packet pauses delivery of all subsequent arrived packets in the socket buffer until the missing segment arrives.

---

## How does UDP work under the hood, and what are its core mechanics and use cases?

UDP (RFC 768) is a minimalist, connectionless transport protocol providing best-effort datagram delivery with zero handshake or connection state.

### UDP Header Layout
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Length             |           Checksum            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```
The fixed 8-byte header contains only Source Port, Destination Port, Total Length, and a 16-bit Checksum over a pseudo-header and payload.

### Core Mechanics & Edge Cases
- **Preserves Message Boundaries**: Calling `sendto()` with a 500-byte buffer produces a discrete datagram delivered atomically to `recvfrom()`, unlike TCP's byte stream.
- **Socket Buffer Drops**: Because UDP lacks flow control, if incoming traffic outpaces application reads, the socket receive buffer (`SO_RCVBUF`) overflows, silently discarding incoming datagrams without notification.
- **Path MTU & IP Fragmentation**: If a UDP datagram exceeds the link Path MTU (typically 1500 bytes for Ethernet), IP fragments the packet. If any fragment is dropped, the entire datagram is discarded. Robust UDP protocols cap packet sizes below safe limits ($\le 1280$ bytes for IPv6, 512 bytes for DNS).

### Production Use Cases
- Short request-response query protocols (DNS, NTP, DHCP).
- Real-time media streaming (WebRTC, VoIP/RTP) where dropping late video frames is preferable to freezing playback.
- Substrate for modern user-space transport protocols like **QUIC** (HTTP/3), which brings zero-RTT handshakes and multiplexing without HOL blocking on top of UDP.

---

## How do Web Protocols function? (HTTP Evolution, HTTPS, and TLS Mechanics)

Web protocols define the application and transport security standards of the Internet, evolving from plaintext text streams into multiplexed, encrypted transports.

### The Evolution of HTTP
- **HTTP/1.1**: Text-based protocol over persistent TCP connections (`Connection: keep-alive`). Suffers from **Application-Level HOL Blocking**: requests on a single connection must be answered in sequential order. Also incurs heavy bandwidth overhead due to uncompressed ASCII headers.
- **HTTP/2**: Introduces a **binary framing layer** that multiplexes multiple independent request-response streams over a single TCP connection, eliminating application-level HOL blocking. Features **HPACK** header compression and server push. *Drawback*: Still suffers from **Transport-Level HOL Blocking** if an IP packet is dropped on the shared TCP connection.
- **HTTP/3**: Replaces TCP with **QUIC over UDP**. Each HTTP stream runs on an independent QUIC stream, eliminating transport HOL blocking. Features **QPACK** compression, unified 0-RTT/1-RTT connection handshakes, and **Connection Migration** via 64-bit Connection IDs (allowing sessions to survive IP switches between Wi-Fi and mobile data).

```
+-------------------------------------------------------------------+
|                           HTTP/1.1                                |  Plaintext Text
|                             TCP                                   |  Transport Stream
+-------------------------------------------------------------------+
|                     HTTP/2 (Binary Framing)                       |  Multiplexed Streams
|                             TLS                                   |  Encryption Layer
|                             TCP                                   |  HOL Blocking Risk
+-------------------------------------------------------------------+
|                            HTTP/3                                 |  QPACK Compressed
|                        QUIC (TLS 1.3)                             |  Per-Stream Transport
|                             UDP                                   |  Datagram Substrate
+-------------------------------------------------------------------+
```

### HTTPS & TLS Mechanics (TLS 1.2 vs. TLS 1.3)
HTTPS layers HTTP over Transport Layer Security (TLS), providing confidentiality, integrity, and server authentication via X.509 certificates:
- **TLS 1.2 (2-RTT Handshake)**: Requires two network round-trips to negotiate cipher suites, exchange certificates, and establish a symmetric session key.
- **TLS 1.3 (1-RTT & 0-RTT Handshake)**: Combines cipher proposal and Diffie-Hellman key shares into the initial `ClientHello`, establishing encryption in a single round-trip (1-RTT). Supports 0-RTT session resumption via Pre-Shared Keys (PSK).
- **Forward Secrecy**: TLS 1.3 deprecates static RSA key exchange, mandating Ephemeral Diffie-Hellman (ECDHE) so past traffic cannot be decrypted if the server's private key is later compromised.
- **ALPN**: Uses Application-Layer Protocol Negotiation inside `ClientHello` to negotiate HTTP/2 or HTTP/3 without extra upgrade round-trips.
- **Production Edge Design**: Terminate TLS at edge CDN PoPs using **OCSP Stapling** to eliminate certificate revocation round-trips, and enforce Mutual TLS (mTLS) via service mesh sidecars (Envoy) for Zero Trust microservice security.
