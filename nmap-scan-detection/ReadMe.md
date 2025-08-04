# Attack Steps: Port Scanning with Nmap (Kali to Metasploitable 3)

## Check Kali Machine's IP Address
1) Open the terminal on your Kali Linux Machine
2) Run the following command to view the current IP address
``` bash
ifconfig
```
3) From the output, I noted the Kali machine's IP as **192.168.177.129**

## Identify Metasploitable 3 Machine's IP Address
1) Next, I switched to the **Metasplotable 3** VM and ran the same command
``` bash
ifconfig
```
2) The IP address for the Metasplotable 3 machine was **192.168.177.130**

## Verify Network Connectivity Between the Machines 
1) Before launching any attacks, I confirmed that both the machines could communicate over the network.
2) From my Kali machine, I ran the command
``` bash
ping 192.168.177.130
```
3) Then, from the Metasploitable3 machine, I tested the reverse connection
``` bash
ping 192.168.177.129
```
4) Both machines successfully responded to each other's ping requests, confirming that they were on the same network and could communicate properly.

## Perform Nmap Port Scan
1) With connectivity confirmed, I initiated a port scan from my Kali machine to the Metasploitable3 target using **nmap**
2) I used the following command to perform a service/version detection scan
``` bash
nmap -sV 192.168.177.130
```
3) This scan helped me identify open ports on the victim machine and provided information about the services running on those ports.

## Analyze Scan Results
1) The scan returned a list of open and closed ports along with the versions of services running on each open port.
<img width="911" height="362" alt="image" src="https://github.com/user-attachments/assets/1126599b-0d15-481c-8f18-cb1afbfc1e9e" />

2) This marked the completion of the reconnaissance phase.
3) I successfully gathered intelligence about the target system’s surface area for potential exploitation.

## Logs Forwarding to Splunk & Packet Capture in Wreshark
1) Metasploitable3 was configured to forward logs to Splunk via Syslog, allowing detection of the Nmap scan as a simulated SOC setup. 
2) Additionally, Wireshark was used to capture all network traffic during the event, providing visibility into ICMP pings and the full port scan.

Detailed analysis of Splunk logs and Wireshark captures is provided in their respective sections:

1) Splunk Analysis

https://github.com/kabir737851/SOC-Home-Lab/blob/main/nmap-scan-detection/Splunk%20/Splunk%20Analysis.md


2) Wireshark Analysis

https://github.com/kabir737851/SOC-Home-Lab/blob/main/nmap-scan-detection/Wireshark/Wireshark%20Analysis.md

   


