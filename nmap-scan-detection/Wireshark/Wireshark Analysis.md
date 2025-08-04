# Wireshark Analysis of Nmap Scan

To confirm and analyze the Nmap scanning behavior at the packet level, I used Wireshark to inspect the traffic between the attacker (Kali) and the victim (Metasploitable3).

1) Started Wireshark on the** Kali machine** before running the **Nmap scan.**
2) Selected the appropriate network interface and began the capture.
3) Used the following display filter to view only initial connection attempts from the attacker:
``` bash
ip.src == 192.168.177.129 && ip.dst == 192.168.177.130 && tcp.flags.syn == 1 && tcp.flags.ack == 0
``` 
## Query Explanation
```bash
ip.src == 192.168.177.129
```
Shows packets sent by the attacker.
``` bash
ip.dst == 192.168.177.130
```
Shows packets going to the target (victim).
``` bash
tcp.flags.syn == 1
```
Matches TCP packets that are trying to initiate a connection (SYN flag set).
``` bash
tcp.flags.ack == 0
```
Excludes any replies (ensures this is the first packet of the 3-way handshake).

## Result: 
This filter revealed multiple SYN packets, It is a clear evidence of Nmap scanning.

4) To determine whether a port is open or closed, I used this response filter:
``` bash
ip.src == 192.168.177.130 && ip.dst == 192.168.177.129
```
This filter shows how the victim responded to the attacker's SYN requests.



## Analyze TCP Flags (SYN-ACK = Open) for deeper inspection:
1) Selected a response packet.
2) Checked the TCP Flags field.
3) Found Flags: 0x012 (SYN and ACK set).

### Explanation:
1) SYN + ACK = Target port is open (responding as expected to connection attempts).
2) To simplify visibility, **right-clicked the TCP flag** field and added it as a **column** for easier tracking of responses.

## Confirm Open Ports with SYN-ACK Filter
To isolate only open ports based on SYN-ACK responses, applied this filter:
``` bash
tcp.flags.syn == 1 && tcp.flags.ack == 1
```
These packets confirmed that the target system was responding to the SYN probes — meaning specific ports were open and accepting connections.


## Conclusion
### Using Wireshark filters, I successfully:
1) Verified the SYN scan behavior initiated by the attacker.
2) Detected which ports were open based on SYN-ACK replies.
3) Cross-validated Nmap results with actual network traffic at the packet level.
