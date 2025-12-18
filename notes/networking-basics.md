# Networking Basics

## IP addresses

- An IP address identifies a device on a network.
- Example private IP: '192.168.1.10'
  (private IPs are only visible to devices on a local network and are assigned by a router)
- Example public IP: '8.8.8.8' (Google DNS)
  (public IPs are visible to everyone on the internet and are assigned by the Internet Service Provider)
  
## Ports

- A virtual endpoint for communication that allows a computer to differentiate between various types of network traffic.
  - Ports are like "doors" on a host that services use.
  - One IP can have many ports open at the same time.

Common ports to remember:
- 22 - Secure Shell (SSH) 
- 80 - HTTP (web, unencrypted)
- 443 - HTTPS (web, encrypted)
- 53 - Domain Name System (DNS)
- 3389 - Remote Desktop Protocol (RDP)
- 25 - Simple Mail Transfer Protocol (SMTP)

## Protocols: TCP vs UDP

**Transmission Control Protocol (TCP)**
- TCP guarantees that data arrives in order and without errors by using acknowledgments and retransmitting lost packets.
  - Connection oriented, reliable.
  - Used for: web (HTTP/HTTPS), SSH, email (SMTP/IMAP/POP3).

**User Datagram Protocol (UDP)**
- UDP uses speed over reliability which makes it good for applications where data loss isnt critical.
  - Connectionless, "fire and forget".
  - Used for: DNS, streaming, some VPNs, games.

## HTTP vs HTTPS

**Hypertext Transfer Protocol (HTTP)**
- Cleartext web traffic (usually **TCP/80**)
- Can be sniffed.
- Can be fully read in Wireshark (`http` filter + FOLLOW TCP Stream)

**Hypertext Transfer Protocol Secure (HTTPS)**
- HTTP over **TLS/SSL** (usually **TCP/443**).
- Encrypted, protects data in transit (e.g. logins, cookies).

## Quick questions for myself

- What is an IP address? A unique number assinged by a router to identify a device on a network.
- What is a port? Doorways that allow diffrent types of traffic.
- Which port does HTTPS use by default? 443
- What does DNS do? Turns domain names into IPs.
- Give one example of a TCP protocol and one UDP protocol.
  - TCP: downloading a document or email
  - UDP: watching netflix

## LAN vs WAN

**LAN (Local Area Network):**
- Small / local scope (home, office or lab)
- Usually owned and managed by one person or organization.
- Example: my VirtualBox lab network `192.168.56.0/24`.

**WAN (Wide Area Network):**
- Large / long distance network (internet, ISP networks)
- Connects multiple LANs together over bigger distances.
- Example: my home network connects to the ISP then connects to the wider internet.

## Switch vs Router 

**Switch:**
- Connects devices inside the same LAN.
- Works mainly with MAC addresses.
- Forwards frames only to the correct port on the local network.

**Router:**
- Connects diffrent networks/LANs together.
- Works with IP addresses.
- Decides where packets go between networks.

## What is a subnet?
- A subnet is a slice of a larger IP network.
- It defines which IPs are considered local to each other.
- Example: `192.168.56.0/24`

## OSI model

The OSI model is a way to break networking into **layers** so it’s easier to understand and troubleshoot from top (what users see) to bottom (wires).

### Layer 7 – Application

- **What it is:** Where applications talk over the network.
- **Examples:** HTTP, HTTPS, DNS, SMTP (email), FTP, SSH, RDP.
- **What I care about as a SOC beginner:**
  - URLs, domains, HTTP methods (GET/POST), user agents.
  - DNS queries (`example.com → IP`).
  - Application logs (web server logs, mail logs, etc.).
- **In Wireshark:** Filters like `http`, `dns`, `ssh`.

---

### Layer 6 – Presentation

- **What it is:** Translates/encodes data so apps can understand it.
- **Examples:** Encryption (TLS), compression, data formats (JSON, XML).
- **Why it matters:**
  - TLS (HTTPS): encrypts HTTP so people can’t sniff passwords in transit.

---

### Layer 5 – Session

- **What it is:** Manages sessions (who is talking to who, and for how long).
- **Examples:** Creating, maintaining, and closing connections/sessions.
- **Why it matters:**
  - Long-lived connections, timeouts, broken sessions.

---

### Layer 4 – Transport

- **What it is:** End-to-end transport of data between hosts.
- **Main protocols:** **TCP** and **UDP**
  - **TCP:** connection-oriented, reliable, ordered delivery.  
    Used by: HTTP/HTTPS, SSH, SMTP, FTP, etc.
  - **UDP:** connectionless, “fire and forget”.  
    Used by: DNS, streaming, some VPNs/games.
- **Ports live here:**
  - Examples: 22 (SSH), 80 (HTTP), 443 (HTTPS), 8080 (alt HTTP), 3389 (RDP).
- **In Wireshark/logs:**
  - Filters like `tcp.port == 22`, `udp.port == 53`.
  - Firewalls and SIEM rules often reference **L4 port + protocol**.

---

### Layer 3 – Network

- **What it is:** Logical addressing and routing between networks.
- **Main protocol:** IP (IPv4/IPv6).
- **Key concepts:**
  - **IP addresses** (e.g. `192.168.56.101`).
  - **Subnets** (e.g. `192.168.56.0/24`).
  - **Routers** move packets between networks/LANs.
- **In Wireshark/logs:**
  - Filters like `ip.addr == 192.168.56.102`.
  - Firewall/SIEM rules often filter by **source/destination IP**.
  - Important for tracking where traffic is coming from/going to.

---

### Layer 2 – Data Link

- **What it is:** Local network communication inside a single LAN.
- **Key concepts:**
  - **MAC addresses** (e.g. `00:1c:42:2b:60:5a`).
  - Frames, ARP, switches.
- **Devices:** Switches, NICs (network cards).
- **In Wireshark:**
  - You can see MAC addresses, ARP traffic, and frame details.
- **Why it matters:**
  - Useful for local network troubleshooting, ARP spoofing detection, and some Wireshark labs.
  - Less common in SIEM logs (most logs focus on IP/port/application).

---

### Layer 1 – Physical

- **What it is:** The actual bits on the wire or in the air.
- **Examples:** Cables, fiber, radio signals, hubs, physical ports.
- **Why it matters:**
  - Link down, bad cable, Wi-Fi signal issues, etc.
- **In virtual labs (like mine):**
  - VirtualBox simulates this, but I usually don’t see L1 directly.

---

## How data moves through the network (encapsulation/decapsulation)

When my **Kali** browser talks to my **Ubuntu** web server on port 8080:

1. **Layer 7 – Application**
   - Browser builds an **HTTP GET** request:
     - `GET / HTTP/1.1`
     - Headers like `Host`, `User-Agent`, etc.

2. **Layer 4 – Transport**
   - HTTP data is wrapped in a **TCP segment**:
     - Source port: random high port (50000)
     - Destination port: `8080`
   - TCP handles things like the handshake (SYN, SYN/ACK, ACK).

3. **Layer 3 – Network**
   - TCP segment gets wrapped in an **IP packet**:
     - Source IP: Kali (`192.168.56.101`)
     - Destination IP: Ubuntu (`192.168.56.102`)

4. **Layer 2 – Data Link**
   - IP packet gets wrapped in an **Ethernet frame**:
     - Source MAC: Kali’s MAC
     - Destination MAC: Ubuntu’s MAC (or router’s MAC if off-LAN)

5. **Layer 1 – Physical**
   - The frame is turned into bits and sent over the (virtual) cable.

On the **Ubuntu** side this is reversed:

- L1/L2: frame is received MAC checked.
- L3: IP says “this is for 192.168.56.102”.
- L4: TCP reassembles the HTTP stream.
- L7: web server reads the HTTP request and sends an HTTP response back the same way.

---

## Router basics 

- Routers connect **diffrent networks** and forwards packets between them.
- Hosts send traffic to a **default gateway** (the router) if the destination is outside their subnet.
- Routers use:
  - A **routing table** to decide which interface/next hop to send a packet to.
  - An **ARP table** to map IP → MAC on each connected network.
- At each hop:
  - Layer 2 (MAC addresses) change
  - Layer 3 (IP addresses) usually stay the same.

---

## Core protocols

### ARP (Address Resolution Protocol)

- Used on local networks to map **IP address → Mac address**.
- Example: "Who has 192.168.1.10? Tell 192.168.1.5."
- In Wireshark:
  - Filter: `arp`
  - Youll see **ARP Request** and  **ARP Reply**.

### DNS (Domain Name System)

- Translates **domain names → IP addresses**.
- Example: `github.com` → `140.82.x.x`
- Typical port: **53/UDP** (sometimes TCP).
- Wireshark filters:
  - `dns`
  - `udp.port == 53`

### DHCP (Dynamic Host Configuration Protocol)

- Automatically gives clients:
  - IP addresses
  - Subnet mask
  - Default gateway
  - DNS server
- Used when a device first joins the network (DISCOVER → OFFER → REQUEST → ACK)

## HTTP vs HTTPS

**Hypertext Transfer Protocol (HTTP)**
- Cleartext web traffic (usually **TCP/80**)
- Can be sniffed.
- Can be fully read in Wireshark (`http` filter + FOLLOW TCP Stream)

**Hypertext Transfer Protocol Secure (HTTPS)**
- HTTP over **TLS/SSL** (usually **TCP/443**).
- Encrypted, protects data in transit (e.g. logins, cookies).


---

## "4 things" a host needs for internet access.

To reach the internet a host needs:

- An **IP address**
- A **subnet mask**
- A **default gateway** (router IP)
- A **DNS server**
(Usually all of these come from **DHCP**.)
