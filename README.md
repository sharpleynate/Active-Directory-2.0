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

The planned security workflow consists of the following steps:

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

In this first part, I'm focusing on understanding the project architecture and planning the infrastructure required for the lab.

---

## Part 2: Cloud Infrastructure & Network Configuration

In this part, I focused on deploying the cloud infrastructure, securing remote access, and establishing network connectivity between my virtual machines.

### Step 1: Deploying the Vultr Instances

I deployed three virtual machines in Vultr's Los Angeles region:

- **Windows Server 2025:** Planned Active Directory Domain Controller
- **Windows Server 2022 Core:** Windows Test Machine
- **Ubuntu Server 22.04:** Splunk SIEM Server

Keeping all three servers in the same region allowed me to configure private networking between them.

![Vultr Instances](images/01_vultr-instances.png)

### Step 2: Configuring Firewall Rules

I created a Vultr firewall group to secure the environment and restrict unauthorized remote access.

- Allowed **SSH (Port 22)** from my public IP address.
- Allowed **RDP (Port 3389)** from my public IP address.
- Blocked all other inbound traffic by default.
- Applied the firewall group to my Windows instances.

![Firewall Configuration](images/02_firewall-rules.png)

![Domain Controller Firewall](images/04_domain-controller-firewall.png)

![Test Machine Firewall](images/06_test-machine-firewall.png)

### Step 3: Connecting to the Windows Servers

I used Remote Desktop Protocol (RDP) to remotely access my Windows servers and begin configuring their network settings.

![Windows Remote Desktop](images/03_windows-rdp.png)

### Step 4: Configuring Private Networking

I configured Vultr's **Virtual Private Cloud (VPC)** networking to allow the virtual machines to communicate through private IP addresses.

I also manually configured static IP addresses on the secondary network adapters of my Windows servers to match the VPC subnet.

![VPC Network Configuration](images/05_vpc-network.png)

![Windows Static IP Configuration](images/08_windows-static-ip.png)

![Splunk Network Configuration](images/07_splunk-network.png)

### Step 5: Troubleshooting Network Connectivity

During testing, I initially encountered connectivity issues when attempting to ping the Windows servers from my Ubuntu Splunk server.

After troubleshooting and correcting the private network configuration, I successfully established communication between the servers.

![Initial Connectivity Troubleshooting](images/09_connectivity-troubleshooting.png)

![Successful Connectivity Test](images/10_connectivity-success.png)

### ✅ Part 2: Results

By the end of this phase, I successfully:

- Deployed three virtual machines on Vultr.
- Configured firewall rules to restrict remote access.
- Established private VPC networking.
- Configured static IP addresses.
- Verified network connectivity between the servers.
