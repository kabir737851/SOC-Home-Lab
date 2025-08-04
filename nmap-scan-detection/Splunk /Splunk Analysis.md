
# Splunk Log Analysis After Nmap Attack

1) Logged into Splunk and went to the Search & Reporting section.
2) Set the time range to **Today** to make sure only the latest logs from the attack are shown.
3) Ran the following Splunk query to check if logs were received
``` bash
index=main (192.168.177.129 OR 192.168.177.130)
| search NOT "dhclient" NOT "DHCP"
| eval details=replace(_raw,".*metasploitable3-ub1404 ","")
| table _time src_ip host sourcetype source pid hostname host details
| sort - count
```
## Query Explanation
A) Looks at logs from two IP addresses: **192.168.177.129** and **192.168.177.130.**
B) It filters out common DHCP-related logs (**dhclient, DHCP**) to reduce noise.
C) Then it creates a new field called details by cutting out everything before the text **metasploitable3-ub1404** from each log line.
D) Which shows a neat table with time, IP, host, source, and the cleaned-up log message.

## Created Splunk Dashboard  
https://github.com/kabir737851/SOC-Home-Lab/blob/main/nmap-scan-detection/evidence/Nmap%20Splunk%20Dashboard.pdf

After verifying that logs were received correctly, I created a custom Splunk Dashboard named **nmap_scan_detection**
1) This helps the SOC Analyst monitore suspicious traffic patterns in real time

# SOC Analyst Case Report for Nmap Scan Detection

Case ID: INC-2025-0802-001
Date/Time: 02 August 2025, 8:15 PM
Reported By: SOC Analyst (L1) – Kabir Bagalkot

## Summary: 
A security alert was triggered because the IP address 192.168.177.129 tried to connect to the 1 unique host (192.168.177.130) 143 times. This is an unusually high number of attempts and suggests someone might be scanning the network to find open ports or weakness.

## Detection Details: 
1) Detection Type: Nmap Scan Detection
2) Source IP: 192.168.177.129 (Attacker – Kali Linux)
3) Destination IP: 192.168.177.130 (Victim – Metasploitable 3)
4) Count: 143 connection attempts
5) Unique Ports Scanned – 143
6) Unique Hosts Scanned – 1
7) Log Source: Syslog (forwarded to Splunk)
8) Detection Query:
```bash
index=main (192.168.177.129 OR 192.168.177.130)
| search NOT "dhclient" NOT "DHCP"
| stats dc(host) as unique_hosts count by src_ip
| search unique_hosts > 1 OR count > 20
```
## Query Explanation: 
1) This query searches the main index for logs involving the IP’s **192.168.177.129** or **192.168.177.130** excluding the event containing the dhclient and DHCP. 
2) It then calculates the number of unique_hosts and the total connection attempts count by src_ip. 
3) Finally, it filters to show only those where unique_hosts is greater than 20, which can indicate scanning activity.

## True Positive: 
This is confirmed malicious activity because the source IP made a large number of scan attempts (143) in a short time.

## Analysis: 
After checking the Splunk logs and Wireshark packet captures, we confirmed that 192.168.177.129 made 143 connections attempts to different open ports on 192.168.177.130. 

1) src_ip:  192.168.177.129 
2) host: 192.168.177.130
3) unique_host: 1
4) count: 143
           
This behavior matches typical scanning activity (like Nmap scans) used by attackers to find weakness.

## Escalation: 
Yes, this needs to be escalated to the SOC level 2 (Incident Response) team. 

### Reason for Escalation:
1) The attacker attempted connections to 143 ports within a short timeframe, which indicates targeted reconnaissance activity.
2) Further forensic analysis is required to determine the attacker’s intent, the full scope of scanning activity and whether any exploitation attempts occurred.
3) L2 SOC analysts will conduct deeper investigations, including correlating data with firewall, IDS/IPS, and endpoint logs, and will take containment actions as required.

## Proposed Action:
1) Block or isolate the source IP (192.168.177.129) at the firewall to prevent further reconnaissance attempts.
2) Review network-wide logs for any additional scanning or suspicious activity originating from this IP address.
3) Perform a vulnerability assessment on the victim system (192.168.177.130) to identify any exposed or exploitable ports.
4) Update SIEM use cases and correlation rules to ensure similar scanning patterns are detected and alerted on promptly.
   
## Status: 

This incident has been escalated to the SOC L2 team for in-depth investigation and containment. Awaiting their findings and recommendations.









   
