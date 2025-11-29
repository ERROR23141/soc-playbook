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

## Domain Name System (DNS)

- Translates domain names (e.g. 'example.com') into IP addresses.
- You type a domain → DNS resolver asks servers → gets an IP → your browser connects.

## HTTP vs HTTPS

**Hypertext Transfer Protocol (HTTP)**
- HyperText Transfer Protocol.
- Data is **cleartext** on the network (can be sniffed).

**Hypertext Transfer Protocol Secure (HTTPS)**
- HTTP + TLS encryption.
- Protects data in transit (e.g. logins, cookies).
- Uses port 443 by default.

## Quick questions for myself

- What is an IP address? A unique number assinged by a router to identify a device on a network.
- What is a port? Doorways that allow diffrent types of traffic.
- Which port does HTTPS use by default? 443
- What does DNS do? Turns domain names into IPs.
- Give one example of a TCP protocol and one UDP protocol.
  - TCP: downloading a document or email
  - UDP: watching netflix
