# SOC Analyst Home Lab Documentation

This documentation outlines the SOC Home Lab I built to develop and showcase **hands-on cybersecurity skills** relevant to real-world Security Operations Centre (SOC) analyst roles. The lab is designed to simulate common security tasks, including penetration testing, log analysis, incident response, and exploit assessment, using **VMware Workstation, Kali Linux, and Metasploitable 3.**

## Why Build a SOC Analyst Lab?

The purpose of this lab is to:
•	Simulate realistic attack and defense scenarios.
•	Practice using popular SOC tools like **Splunk** and **Wireshark**.
•	**Analyse logs** and **events** in a controlled environment.
•	**Improve detection** and **response skills** using real-world techniques.
This lab is fully virtualized, cost-effective, and can be set up using open-source and free tools on a **single machine**.

## Virtual Lab Overview
### Components used in the lab:
•	**Kali Linux:** Attacker machine with built-in penetration testing tools.
•	**Metasploitable 3 (Ubuntu 14.04):** Vulnerable victim machine configured using Vagrant.
•	**Syslog:** Forwarding logs from the victim machine to SIEM.
•	**Splunk:** SIEM platform for log analysis and incident detection.
•	**Wireshark:** Network packet capture and analysis.
•	**VMware Workstation:** Virtualization platform to run multiple VMs safely.

## Setting Up the Lab
**1. Install VMware Workstation**
Download and install **VMware Workstation Pro** on your host machine:
https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion

## 2. Set Up Kali Linux
1.	Download **Kali Linux ISO:** https://www.kali.org/get-kali/#kali-platforms
2.	Create a new VM in VMware Workstation and mount the ISO.
3.	Follow the GUI installer to complete setup.

After the installation, update the machine using the following command:

```bash
sudo apt update
sudo apt upgrade -y
```

## 3. Set Up Metasploitable 3
Metasploitable 3 requires **Vagrant** and **VirtualBox** for setup:
Install Prerequisites:
### On Ubuntu 
```bash
sudo apt update
sudo apt install virtualbox vagrant git -y
```
### Clone the Metasploitable 3 repository:
```bash
git clone https://github.com/rapid7/metasploitable3.git
cd metasploitable3
```
### Build and start the VM:
```bash
vagrant up
```
This process will download the Ubuntu 14.04 base box, install all required vulnerable services, and boot the VM.
**Metasploitable 3 Repo:** https://github.com/rapid7/metasploitable3
**Ubuntu 14.04 Base Image:** https://releases.ubuntu.com/14.04/

## 4. Install and Configure Splunk
1.	**Download Splunk (Free version):** https://www.splunk.com/en_us/download.html
2.	**Download Splunk (Free version):** https://www.splunk.com/en_us/download.html
3.	**Install on your preferred VM or host. Example (Ubuntu):**
    ```bash
    sudo apt install ./splunk-<version>-linux-2.6-amd64.deb
    ```
4.	**Start Splunk and set admin password:**
    ```bash
    sudo /opt/splunk/bin/splunk start --accept-license
    ```
6.	**Enable Splunk to start at boot:**
    ```bash
    sudo /opt/splunk/bin/splunk enable boot-start
    ```
8.	**Configure UDP port 517 as the data input in the Splunk Web UI:** **Settings → Data Inputs → UDP → Add New → Port: 517**
   
## 5. Configure Syslog on Metasploitable 3
1.	Ensure rsyslog is installed:
   ```bash
   sudo apt-get update
   sudo apt-get install rsyslog -y
   ```
3.	Edit the syslog config file:
    ```bash
    sudo nano /etc/rsyslog.conf
    ```
4.	Add the following line to forward logs to Splunk:
    ```bash
    *.*   @<splunk_server_ip>:517
    ```
5.	Restart the syslog service:
    ```bash
    sudo service rsyslog restart
    ```
## 6. Install Wireshark
**Wireshark can be installed on your host or attacker VM:**
   ```bash
   sudo apt update
   sudo apt install wireshark -y
   ```
**Download Wireshark:** https://www.wireshark.org/





   	













