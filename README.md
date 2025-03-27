# Splunk SIEM Home Lab Setup

## Overview
This project documents the setup and configuration of Splunk, a powerful Security Information and Event Management (SIEM) tool, in a home lab environment. The implementation includes:
- Splunk Enterprise installed on an Ubuntu VM
- Splunk Universal Forwarder configured on a Windows Server 2019 Domain Controller
- Centralized log management and real-time monitoring capabilities

## Why Splunk?
Splunk is widely used in cybersecurity for its ability to collect, analyze, and visualize vast amounts of data from various sources. Key benefits include:

- **Centralized Log Management**: Aggregating logs from multiple sources for easier monitoring and analysis
- **Real-Time Monitoring**: Detecting and responding to security incidents as they happen
- **Advanced Analytics**: Using machine learning and custom queries to identify trends and threats

## Installation Guide

### Ubuntu VM Setup for Splunk

#### Downloading and Creating the VM
1. Download the latest Ubuntu LTS (22.04.3 at time of writing) from [Ubuntu Downloads](https://ubuntu.com/download)
2. Create a new VM in VirtualBox:
   - Name: "Ubuntu Splunk"
   - Memory: 4096MB (4GB)
   - Hard Disk: 100GB
   - Network: Connected to "LAN 4" (SECURITY subnet)

#### Installing Ubuntu
1. Boot the VM and launch the graphical installer
2. Follow installation prompts (language, keyboard layout, etc.)
3. Opt to install third-party software for better compatibility
4. Set up user account and hostname
5. Complete installation and restart VM

#### Post-Installation Configuration
```
# Update system
sudo apt update && sudo apt full-upgrade

# Install VirtualBox Guest Additions
sudo ./VBoxLinuxAdditions.run
```
## Splunk Enterprise Installation
Downloading Splunk

1. Visit Splunk Enterprise Free Trial
2. Register an account and download the .deb package (version 9.1.2)

Installing Splunk
```
# Install dependencies
sudo apt install curl

# Install Splunk
sudo dpkg -i splunk-9.1.2-b6b9c8185839-linux-2.6-amd64.deb

# Start Splunk and accept license
sudo /opt/splunk/bin/splunk start --accept-license --answer-yes

# Enable boot start
sudo /opt/splunk/bin/splunk enable boot-start
```
## Configuring Splunk for Data Ingestion
Access Splunk web interface at http://127.0.0.1:8000

Configure data receiving:

1. Go to Settings > Forwarding and Receiving
2. Under "Receive data," click "Add new"
3. Set listening port to 9997

Windows Universal Forwarder Setup
Installation

1. Download Windows Universal Forwarder from Splunk Download page
2. Run the .msi installer
3. Accept license agreement and enter Splunk admin credentials
4. Configure with Splunk VM IP address and ports 8089/9997

Data Ingestion Configuration

In Splunk web interface:

1. Go to Settings > Add Data > Forward
2. Add DC VM as data source
3. Configure to send all local event logs
4. Create "Windows" index for logs

Verification

To verify data ingestion:

1. In Splunk, navigate to Apps > Search & Reporting
2. Run search query:
```
index="windows"
```
3. Perform actions on DC VM to generate logs if none appear initially

Snapshots

Recommended snapshots:

1. Clean, updated Ubuntu installation
2. Post-Splunk installation
3. Final configured state

Conclusion

This Splunk implementation completes a comprehensive cybersecurity home lab, providing:

1. Centralized log collection and analysis
2. Real-time security monitoring
3. Enhanced threat detection capabilities
4. Valuable hands-on experience with enterprise-grade SIEM tools
