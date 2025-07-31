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
**sudo apt update**
**sudo apt upgrade -y**

## 3. Set Up Metasploitable 3
Metasploitable 3 requires **Vagrant** and **VirtualBox** for setup:
Install Prerequisites:
### On Ubuntu 
**sudo apt update**
**sudo apt install virtualbox vagrant git -y**

### Clone the Metasploitable 3 repository:
**git clone https://github.com/rapid7/metasploitable3.git**
**cd metasploitable3**

### Build and start the VM:
**vagrant up**
This process will download the Ubuntu 14.04 base box, install all required vulnerable services, and boot the VM.
**Metasploitable 3 Repo:** https://github.com/rapid7/metasploitable3
**Ubuntu 14.04 Base Image:** https://releases.ubuntu.com/14.04/

## 4. Install and Configure Splunk













