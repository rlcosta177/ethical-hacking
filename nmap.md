## For better understanding, use a honeypot and scan it with nmap

## Nmap switches

```sh
TCP Connect Scans (-sT)
SYN "Half-open" Scans (-sS)
UDP Scans (-sU)
```

## TCP Connect Scans

### Understanding the three way handshake
- To understand TCP connect scans, its important to know how the three way handshake works.
- In short, the three-way handshake consists of three stages. 
  - First the connecting terminal sends a TCP request to the target server with the SYN flag set.
  - The server then acknowledges this packet with a TCP response containing the SYN flag, as well as the ACK flag.
  - Finally, our terminal completes the handshake by sending a TCP request with the ACK flag set.

### Posibility One
- Its important to note that, if you send a SYN flag to a closed port, the receiving machine will respond with a RST flag, that is because of the RFC9293 which states that any closed port will respond with RST to any incoming flag that isnt RST

![vUQL9SK](https://github.com/rlcosta177/ethical-hacking/assets/154469533/4fb9d990-cda9-4e5d-afba-c69b18bc571a)

### Possibility Two
- If the target machine is hidden behind a firewall, its likely that it is configured to **drop** any incoming packets. Nmap sends a TCP SYN request, and receives nothing back. This indicates that the port is being protected by a firewall and thus the port is considered to be **filtered**.

![Uploading filtered-nmap.png…]()

