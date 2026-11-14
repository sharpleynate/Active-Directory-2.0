# Active-Directory-2.0
Simulated enterprise SOC environment using Active Directory, Splunk SIEM, and Shuffle SOAR for threat detection and automated incident response.

<img width="1774" height="887" alt="37466940-7507-473c-925d-506738b7344a" src="https://github.com/user-attachments/assets/255c5c35-5cc3-45e3-931b-631869d91ac6" />

# Active Directory 2.0 — SOC Automation & Incident Response

## Part 1: Project Overview

In this project, I'm building an Active Directory environment to simulate real-world cyberattacks and automate security responses.

### 🖥️ Lab Infrastructure

The environment consists of **three virtual machines hosted on Vultr**:

1. **Domain Controller (Windows Server):** Manages domain users and accounts.
2. **Splunk Server (Ubuntu):** Collects security logs and detects suspicious activity.
3. **Windows Test Machine:** Simulates a vulnerable endpoint that attackers can target.

### 🔄 How It Works

1. An attacker attempts to authenticate to the Windows test machine.
2. Splunk collects authentication logs and generates a security alert.
3. Shuffle SOAR triggers an automated incident response workflow.
4. A SOC analyst receives a notification and reviews the suspicious activity.
5. The analyst decides whether the potentially compromised Active Directory account should be disabled.

### 🎯 Project Goal

Build a hands-on SOC environment that combines:

- **Active Directory** — Identity and access management
- **Splunk SIEM** — Security monitoring and threat detection
- **Shuffle SOAR** — Security orchestration and automated response
- **Slack** — Security alert notifications

### 📌 Part 1: Current Focus

In this first part, I'm focusing on understanding the project architecture and setting up the infrastructure required for the lab.

Part 2: Cloud Infrastructure & Network Configuration
In this part, I focused on configuring the cloud infrastructure, securing remote access, and establishing network connectivity between my virtual machines.
Step 1: Deploying the Vultr Instances
I deployed three virtual machines in Vultr's Los Angeles region: a Windows Server for Active Directory, a Windows test machine, and an Ubuntu server for Splunk. Keeping the instances in the same region allowed me to configure private networking between them.

Step 2: Configuring Firewall Rules
I created a Vultr firewall group and restricted inbound SSH (port 22) and RDP (port 3389) access to my public IP address. All other inbound traffic was blocked by default. I also configured the firewall group on my Windows instances.



Step 3: Connecting to the Windows Servers
I used Remote Desktop Protocol (RDP) to connect to the Windows virtual machines and begin configuring their network settings.

Step 4: Configuring Private Networking
I enabled Vultr's Virtual Private Cloud (VPC) networking so the servers could communicate over private IP addresses rather than relying on their public interfaces. I reviewed the VPC network configuration and configured the secondary network adapter on the Windows machines with static IP settings matching the private subnet.



Step 5: Troubleshooting and Verifying Connectivity
My initial ping tests between the Ubuntu Splunk server and the Windows machines failed. After correcting the private network configuration, I repeated the tests and successfully received replies from the Windows servers over their private IP addresses.


Part 2 Result
By the end of this phase, I had deployed the three cloud servers, configured restricted remote access, established private networking, and verified connectivity. The infrastructure is ready for the next phase: installing Active Directory Domain Services and promoting the Windows Server to a domain controller.
