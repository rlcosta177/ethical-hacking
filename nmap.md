## For better understanding, use a honeypot and scan it with nmap

## Nmap switches

```sh
TCP Connect Scans (-sT)
SYN "Half-open" Scans (-sS)
UDP Scans (-sU)
```
---

## TCP Connect Scans

### Understanding the three way handshake
- To understand TCP Connect Scans (-sT), its important to know how the three way handshake works.
- In short, the three-way handshake consists of three stages. 
  - First the connecting terminal sends a TCP request to the target server with the SYN flag set.
  - The server then acknowledges this packet with a TCP response containing the SYN flag, as well as the ACK flag.
  - Finally, our terminal completes the handshake by sending a TCP request with the ACK flag set.

### Posibility One
- The target machine responds with open/closed port status.

### Posibility Two
- Its important to note that, if you send a SYN flag to a closed port, the receiving machine will respond with a RST flag, that is because of the RFC9293 which states that any closed port will respond with RST to any incoming flag that isnt RST

![vUQL9SK](https://github.com/rlcosta177/ethical-hacking/assets/154469533/4fb9d990-cda9-4e5d-afba-c69b18bc571a)

### Possibility Three
- If the target machine is hidden behind a firewall, its likely that it is configured to **drop** any incoming packets. Nmap sends a TCP SYN request, and receives nothing back. This indicates that the port is being protected by a firewall and thus the port is considered to be **filtered**.

![filtered-nmap](https://github.com/rlcosta177/ethical-hacking/assets/154469533/3772794e-5759-4f14-a46a-468000a114f0)

---

## SYN Scans

SYN scans (-sS) are essencially stealthy because they send a SYN flag followed by an RST flag, which prevents the target from repeatedly trying to complete the request

![cPzF0kU](https://github.com/rlcosta177/ethical-hacking/assets/154469533/73af9915-6c32-40c6-9bbf-2c0088534e91)

---

## UDP Scans

Unlike TCP, UDP connections are stateless. This means that, rather than initiating a connection with a back-and-forth "handshake", UDP connections rely on sending packets to a target port and essentially hoping that they make it. This makes UDP superb for connections which rely on speed over quality (e.g. video sharing), but the lack of acknowledgement makes UDP significantly more difficult (and much slower) to scan. The switch for an Nmap UDP scan is (-sU)

When a packet is sent to an open UDP port, there should be no response. When this happens, Nmap refers to the port as being `open|filtered`. In other words, it suspects that the port is open, but it could be firewalled. 

![23123](https://github.com/rlcosta177/ethical-hacking/assets/154469533/a0b755a9-21a8-438b-bc78-19e25d950312)
















































