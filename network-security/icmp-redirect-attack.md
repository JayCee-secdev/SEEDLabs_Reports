# Lab Report: ICMP Redirect Attack
## **🎯 Objective**
To understand how the ICMP redirect message can be abused by an attacker to intercept and redirect network traffic from a victim to a malicious gateway, effectively performing a Man-in-the-Middle (MITM) attack.

## **🛠️ Environment & Tools**
- System: Docker Container Environment (SEED VM)
- Key Tools: Scapy (Python), Wireshark, ip route, ping
- Key IPs:
    - Victim: 10.9.0.5
    - Real Router: 10.9.0.1
    - Malicious Router (Attacker): 10.9.0.105

## **💡 Key Learning & Tips**
### The ICMP Redirect Concept: 
ICMP redirect messages are normally used by routers to tell a host, "There is a better route to your destination." In this lab, the attacker sends a forged redirect message to the victim, claiming that the attacker's IP is the best gateway for a specific destination.
### The "Racing" Requirement: 
For the attack to be successful, timing is key. You often need to have a continuous ping running on the victim machine while launching the attack script to ensure the victim's routing cache updates.
### Filter by MAC, Not Just IP: 
A major breakthrough in this lab was realizing that filtering by IP address can cause a loop where the malicious router re-captures its own modified packets. Filtering by the Victim's MAC address is much more robust because MAC addresses are hardware-specific and don't change during the packet modification process.
Scapy's Power: Using Scapy allows you to construct packets layer by layer. Setting type=5 and code=1 specifically identifies the packet as an ICMP Redirect for a host.

## 🔍 Troubleshooting Log
| Issue Encountered | Root Cause | Solution/Observation |
| --- | --- | --- |
| Routing cache not updating | The attack was sent while the network was idle | Run a ping on the victim to trigger the routing mechanism before sending the spoofed ICMP packet |
| Packet Looping | The attacker was capturing packets it had already forwarded | Refine the Scapy sniffer filter to exclude the attacker's own MAC address or focus strictly on the victim's MAC |
| Modified data not appearing | Forwarding was happening, but the "Man-in-the-Middle" payload wasn't being injected | Ensure the Python script correctly intercepts, modifies (e.g., changing "Juan" to "John"), and re-transmits the packet |

## 🏁 Summary of Defense
- The ICMP Redirect attack is a classic example of why network protocols shouldn't blindly trust unauthenticated control messages.
- Disable ICMP Redirects: In modern secure environments, hosts are often configured to ignore ICMP redirect messages entirely (net.ipv4.conf.all.accept_redirects = 0).
- Static Routing: For critical systems, manually defining routes prevents any dynamic protocol from altering the path of the data.
- Firewalls: Modern firewalls can be configured to drop ICMP redirect packets that do not originate from a known, trusted gateway.
