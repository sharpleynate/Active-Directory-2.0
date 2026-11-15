
# Active Directory 2.0 — SOC Automation & Incident Response

A hands-on cybersecurity project focused on building a simulated enterprise SOC environment using Active Directory, Splunk SIEM, Shuffle SOAR, and Slack for threat detection and automated incident response.

![SOC Automation Architecture](https://github.com/user-attachments/assets/27d698bd-e8c9-496e-b7ec-db51e0abfcf7)

---

## Part 1: Project Overview & Architecture

### Project Overview

In this project, I'm building an Active Directory environment to simulate real-world cyberattacks, monitor suspicious authentication activity, and automate security responses.

The goal is to understand how different security technologies work together in a SOC environment, from collecting security logs to detecting threats and responding to potential account compromises.

### 🖥️ Lab Architecture

The environment consists of three virtual machines hosted on Vultr:

| Virtual Machine | Operating System | Purpose |
|---|---|---|
| Domain Controller (`sharp-ADDC01`) | Windows Server 2025 Standard | Planned Active Directory users, accounts, and authentication management |
| Splunk Server (`sharp-Splunk`) | Ubuntu Server 22.04 | Planned security log collection and analysis |
| Test Machine | Windows Server 2022 Core | Simulated endpoint for testing |

### 🔄 Planned Security Workflow

The project is designed to follow this incident response process:

1. **Attack Simulation:** An attacker attempts to authenticate to the Windows test machine.
2. **Log Collection:** Authentication activity is forwarded to Splunk for monitoring.
3. **Threat Detection:** Splunk identifies suspicious activity and generates a security alert.
4. **SOAR Automation:** The alert triggers a Shuffle SOAR playbook and sends a notification to Slack.
5. **Analyst Review:** Shuffle notifies a SOC analyst, who determines whether the affected account should be disabled.
6. **Incident Response:** If approved, the domain account is disabled and a confirmation is sent to Slack. Otherwise, no automated remediation occurs.

### 🎯 Project Objectives

Through this project, I aim to develop hands-on experience with:

- **Active Directory:** Identity and access management
- **Splunk SIEM:** Security monitoring and threat detection
- **Shuffle SOAR:** Security orchestration and automated incident response
- **Slack:** Security alert notifications
- **Windows Security Logs:** Authentication monitoring and investigation
- **Cloud Networking:** Virtual machine deployment, firewall configuration, and private networking

### Part 1 Summary

This phase focused on understanding the overall architecture, identifying the required technologies, and planning how the different systems will communicate.

The next phase involves deploying and configuring the infrastructure needed to support the environment.

---

## Part 2: Cloud Infrastructure & Network Configuration

### Overview

In this phase, I deployed virtual machines on Vultr, configured firewall rules for remote access, established private networking, and troubleshot connectivity issues between the servers.

The objective was to prepare a functioning cloud environment before configuring Active Directory and integrating the security monitoring tools.

### Step 1: Deploying the Virtual Machines

I deployed three virtual machines in Vultr's **Los Angeles region**:

- **Windows Server 2025 Standard:** Future Active Directory Domain Controller (`sharp-ADDC01`)
- **Windows Server 2022 Core:** Windows Test Machine
- **Ubuntu Server 22.04:** Future Splunk SIEM Server (`sharp-Splunk`)

The instance dashboard shows all three machines and their operating systems. It also captures their running or stopped status at that point in the deployment.

![Vultr instance list showing three deployed servers](01_vultr-instances.png)

### Step 2: Configuring Firewall Security

I created a Vultr firewall group named `sharp-AD-Project-2.0` to restrict administrative access. The configured inbound rules shown in the screenshot allow SSH (port 22) and RDP (port 3389) from a specified `/32` public IP, with a default deny rule for other inbound traffic.

![Vultr firewall group inbound rules](02_firewall-rules.png)

I then checked the firewall group selection on the intended Domain Controller instance.

![Domain Controller firewall group selection](03_windows-rdp.png)

I also reviewed the firewall group selection on the Windows test machine.

![Test Machine firewall group selection](06_test-machine-firewall.png)

**Note:** The firewall rule screenshot shows **zero attached instances** at that moment, while the instance settings screenshots show firewall groups selected. These screenshots document configuration steps at different times; they do not, by themselves, prove the rules were active on both machines simultaneously.

### Step 3: Establishing Remote Access

I accessed the Windows server through Remote Desktop Protocol (RDP) to perform configuration directly inside the operating system.

The following screenshot shows the established Windows desktop session.

![Windows Remote Desktop session](04_domain-controller-firewall.png)

### Step 4: Configuring Private VPC Networking

I configured a Vultr VPC connection for internal communication between the servers, reducing the need to use public IP addresses for server-to-server traffic.

The Windows instance's VPC settings show its assigned private address and network details.

![Windows server VPC network assignment](05_vpc-network.png)

I also checked the Ubuntu server's public IPv4 configuration while reviewing its network connectivity. **This screenshot shows its public network settings, not its VPC address.**

![Ubuntu Splunk server public IPv4 configuration](07_splunk-network.png)

On the Windows test machine, I opened the secondary adapter's IPv4 properties and manually entered a private address and subnet mask for the VPC network.

![Windows secondary adapter static IPv4 settings](08_windows-static-ip.png)

### Step 5: Troubleshooting Network Connectivity

I tested connectivity from the Ubuntu server using `ping`. The initial attempts to reach the Windows test machine's private address returned **Destination Host Unreachable** messages. A separate public-address test also showed packet loss.

After adjusting the network configuration, the same terminal session showed successful ICMP replies from the previously unreachable private address.

![Initial failed tests followed by successful connectivity](09_connectivity-troubleshooting.png)

I then ran another ping test to the other Windows server's private address and received successful ICMP replies.

![Successful private IP ping from Ubuntu](10_connectivity-success.png)

These tests confirmed basic ICMP reachability to the two private targets shown. Additional service-level checks will be needed when Active Directory and Splunk are configured.

### ✅ Part 2: Results

By the end of this phase, I had:

- Deployed three Vultr instances in the Los Angeles region.
- Created firewall rules limiting SSH and RDP access to a specified source IP.
- Accessed a Windows server through RDP.
- Attached and configured private VPC networking on the Windows environment.
- Configured a static private IPv4 address on a Windows network adapter.
- Troubleshot failed ICMP connectivity tests.
- Verified successful ICMP replies to two Windows private IP addresses from Ubuntu.

### Next Steps

1. Install and configure Active Directory Domain Services.
2. Promote the Windows Server to a Domain Controller.
3. Configure the domain and DNS.
4. Join the Windows test machine to the domain.
5. Prepare Windows security event collection for Splunk.
