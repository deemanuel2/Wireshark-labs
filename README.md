# Wireshark-labs
Hands-on Wireshark labs where I investigate network captures as a SOC analyst, tracking attackers, hidden data, exposed passwords, and a scam call.
| Lab | Topic | Key skill |
|-----|-------|-----------|
| 03 | Identify the attacker's IP | Spotting a SYN port scan and open/closed ports |
| 04 | Follow TCP Stream | Reading a plain text chat on TCP port 5000 |
| 05 | Follow TCP Stream, part 2 | Finding a hidden note in an HTTP page |
| 06 | HTTP plain text credentials | Finding login POSTs in unencrypted traffic |
| 07 | FTP brute force catch | Spotting failed logins (530) and the success (230) |
| 08 | VoIP call replay | Decoding SIP and RTP and replaying a call |

---

# Lab 3: Identify the Attacker's IP

## Scenario
A staff member reported unusual traffic to the company's web server. I
captured the traffic, analysed it in Wireshark, and found out who the
attacker is and what kind of attack it is.

## Method
1. Checked the capture size: 309 packets
2. Filtered pure SYN packets: tcp.flags.syn == 1 && tcp.flags.ack == 0
3. Used Statistics > Conversations (TCP tab) to see one source sending to
   many destination ports
4. Counted the SYN packets from the attacker: 40
5. Filtered the server's replies to separate open and closed ports:
   - Open: ip.src == 192.168.10.20 && tcp.flags.syn == 1 && tcp.flags.ack == 1
   - Closed: ip.src == 192.168.10.20 && tcp.flags.reset == 1

## Findings
- Packets captured: 309
- Attacker IP: 192.168.10.66
- Target IP: 192.168.10.20
- Attacker's source port: 52311 (the same in every packet)
- Ports probed: 40 different destination ports
- Open ports: 22, 80, 443 (answered SYN, ACK)
- Closed ports: 37 (answered RST, ACK)
- Attack type: port scan

## Lesson learned
One source sending SYN packets to many different destination ports on a
single target is the signature of a port scan. Attackers use it to find open
services before choosing what to attack. Defenses: firewall rules that drop
unneeded ports, rate limiting, and an IDS/IPS that alerts on scan patterns.

## Screenshots
(Conversations tab, SYN filter result showing Displayed: 40, SYN-ACK replies,
RST replies, 10/10 score)
