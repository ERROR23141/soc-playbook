# Wireshark Cheatsheet

## Basic display filters

- `ip.addr == 8.8.8.8` – traffic to/from 8.8.8.8 (Google DNS)
- `tcp.port == 80` – HTTP traffic on port 80
- `udp.port == 53` – DNS traffic
- `http` – all HTTP packets
- `icmp` – ping (echo request/reply) traffic
- `tcp.port == 22` – SSH traffic
- `ip.addr == 192.168.56.102` – all traffic to/from my Ubuntu VM  
  *(change IP when needed)*

## HTTP traffic

- `tcp.port == 8080` – traffic to my test Python server
- `http` – show decoded HTTP packets
- Use **Follow → TCP Stream** on an HTTP packet to see the full request and response.

## DNS traffic

- `udp.port == 53` - DNS queries and responses.
- `dns` - only DNS protocol packets.
- `dns.qry.name == "example.com"` - DNS queries for specific domains.
- `dns.flags.response == 0` - DNS queries only.
- `dns.flags.response == 1` - DNS responses only. 
