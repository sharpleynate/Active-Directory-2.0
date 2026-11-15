
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

---

## Part 3: Active Directory Deployment & Remote Access Configuration

### Overview

In this phase, I configured Active Directory Domain Services (AD DS), established a new Active Directory forest, and began integrating the Windows test machine into the domain environment.

I also investigated a Remote Desktop authentication issue involving a domain user account and configured the appropriate remote access permissions.

### Step 1: Installing Active Directory Domain Services

I accessed the Windows Server through the Vultr console and opened Server Manager.

Using the **Add Roles and Features Wizard**, I selected the **Active Directory Domain Services (AD DS)** server role.

AD DS provides centralized identity management, authentication, and access control for Windows domain environments.

![Active Directory Domain Services role selected in Server Manager](11_ad-ds-role-installation.png)

### Step 2: Promoting the Server to a Domain Controller

After installing the AD DS role, I launched the Active Directory Domain Services Configuration Wizard.

Because this was a new environment, I selected **Add a new forest** and configured the following root domain:

- **Root domain:** `sharp.local`
- **Deployment type:** New Active Directory forest
- **Server role:** Domain Controller

This establishes the foundation for centralized authentication and domain management.

![New Active Directory forest configuration using sharp.local](12_new-forest-sharp-local.png)

### Step 3: Verifying the Active Directory Domain

After the domain configuration, I opened **Active Directory Users and Computers (ADUC)** to verify that the new domain was available.

The console displayed `sharp.local` and its default Active Directory containers, including:

- Builtin
- Computers
- Domain Controllers
- ForeignSecurityPrincipals
- Managed Service Accounts
- Users

This confirmed that the Active Directory domain was accessible through the management console.

![Active Directory Users and Computers showing sharp.local](13_active-directory-domain.png)

### Step 4: Preparing the Windows Test Machine

Next, I accessed the Windows test machine to prepare it for integration with the Active Directory environment.

I opened **System Properties** to review its computer name and current workgroup membership.

The screenshot captures the machine before its domain membership was verified.

For domain integration, the test machine must use DNS capable of resolving the `sharp.local` Active Directory domain, typically through the Domain Controller's DNS service.

![Windows test machine computer name and workgroup settings](14_test-machine-system-properties.png)

### Step 5: Troubleshooting Remote Desktop Authentication

While attempting to connect to the Windows test machine using the domain user account `SHARP\JSmith`, I encountered the following error:

> The connection was denied because the user account is not authorized for remote login.

This indicated that the attempted account did not have the necessary Remote Desktop logon authorization on the target system.

The error was related to remote logon permissions rather than establishing that the account password was incorrect.

![Remote Desktop connection denied for domain user](15_rdp-authorization-error.png)

### Step 6: Configuring Remote Desktop User Permissions

To address the authorization issue, I opened the Windows **System Properties** Remote settings and accessed the **Remote Desktop Users** configuration.

From the domain location `sharp.local`, I selected the account **Jenny Smith (`JSmith`)** for inclusion in the permitted Remote Desktop users.

This step was intended to grant the domain account permission to establish an RDP session on the test machine.

![Selecting Jenny Smith for Remote Desktop access](16_remote-desktop-user-permissions.png)

The screenshot documents the user selection process. A successful login test is still needed to confirm that the permission change took effect.

### Part 3: Results

During this phase, I:

- Selected and installed the Active Directory Domain Services role.
- Configured a new Active Directory forest named `sharp.local`.
- Verified the domain through Active Directory Users and Computers.
- Reviewed the Windows test machine's computer and domain settings.
- Investigated a domain-user Remote Desktop authorization error.
- Configured the domain user selection for Remote Desktop access.

---

---

## Part 4: Splunk SIEM Deployment & Windows Event Log Integration

### Overview

In this phase, I deployed Splunk Enterprise on the Ubuntu server, configured Windows Universal Forwarders, collected Windows Security event logs, and verified telemetry from both Windows systems inside Splunk.

I also encountered and resolved a forwarding configuration issue on the Domain Controller. During the Universal Forwarder setup, I mistakenly used the Domain Controller's private VPC address instead of the Splunk server's private VPC address. After identifying the issue, I corrected the forwarding destination to the Splunk server and restarted the SplunkForwarder service.

### Step 1: Preparing the Ubuntu Splunk Server

Before installing Splunk, I prepared the Ubuntu server and created a simple Bash script to update and upgrade installed packages.

```bash
#!/bin/bash

sudo apt update -y && sudo apt upgrade -y
```

![Ubuntu update and upgrade script](18_ubuntu-update-script.png)

### Step 2: Downloading Splunk Enterprise

I downloaded the Splunk Enterprise Debian package directly from Splunk using `wget`.

The package downloaded was Splunk Enterprise 10.6.1.1 for Linux AMD64.

![Downloading Splunk Enterprise](17_splunk-enterprise-download.png)

After installing Splunk, the application files were available under:

```text
/opt/splunk/bin
```

This directory contains the Splunk command-line utilities used to start, stop, and manage the Splunk instance.

### Step 3: Configuring Splunk Web Access

Splunk Web uses TCP port **8000**.

I first worked on adding an inbound TCP rule for port 8000 through the Vultr firewall configuration.

![Vultr firewall rule configuration for port 8000](20_vultr-web-port-rule.png)

I also allowed port 8000 through the Ubuntu host firewall using UFW.

```bash
ufw allow 8000
```

The screenshot below shows the Splunk installation directory and the successful addition of the UFW rule.

![Splunk directory and UFW port 8000 rule](19_splunk-installation-files.png)

This allowed the Splunk web interface to be accessed remotely.

### Step 4: Configuring Splunk Preferences

After accessing Splunk Enterprise through the web interface, I reviewed the administrator preferences.

I configured the interface time zone to **GMT (Greenwich Mean Time)** to keep timestamps consistent during event analysis.

![Splunk administrator timezone preferences](21_splunk-web-firewall.png)

### Step 5: Installing the Splunk Add-on for Microsoft Windows

I installed the **Splunk Add-on for Microsoft Windows**.

This add-on provides additional support for parsing and working with Windows event data inside Splunk.

![Splunk Add-on for Microsoft Windows installed](22_splunk-web-preferences.png)

### Step 6: Enabling the Splunk Receiving Port

To receive data from the Windows Universal Forwarders, I configured the Splunk server to listen on TCP port **9997**.

![Splunk receiving configuration on port 9997](23_windows-addon-installed.png)

I then allowed port 9997 through the Ubuntu firewall.

```bash
ufw allow 9997
```

![Ubuntu UFW rule allowing port 9997](29_initial-zero-events.png)

Port 9997 is used by the Universal Forwarders to send event data to the Splunk server.

### Step 7: Installing the Splunk Universal Forwarder

I installed the **Splunk Universal Forwarder** on the Windows systems.

The Universal Forwarder collects selected Windows event logs and sends them to the Splunk server for indexing and analysis.

![Splunk Universal Forwarder installation](24_splunk-receiving-port.png)

After installation, the local configuration files were stored under:

```text
C:\Program Files\SplunkUniversalForwarder\etc\system\local\
```

![Splunk Universal Forwarder local configuration directory](25_universal-forwarder-installation.png)

### Step 8: Configuring Windows Security Event Collection

I configured `inputs.conf` to collect events from the Windows Security log and send them into my custom Splunk index named:

```text
sharp-ad
```

The Security log input included:

```ini
[WinEventLog://Security]
index = sharp-ad
disabled = false
```

![Splunk Universal Forwarder inputs.conf configuration](26_forwarder-configuration-directory.png)

This allowed authentication and other Windows Security events to be collected by the forwarder.

### Step 9: Reviewing the SplunkForwarder Service

I reviewed the Windows **SplunkForwarder** service configuration to confirm which account was being used to run the service.

The screenshot shows the forwarder configured under:

```text
NT SERVICE\SplunkForwarder
```

![SplunkForwarder Windows service Log On configuration](27_forwarder-security-input.png)

### Step 10: Troubleshooting Initial Event Ingestion

After configuring the forwarder, I searched the custom Splunk index:

```spl
index=sharp-ad
```

Initially, Splunk returned **0 events**.

![Initial sharp-ad search returning zero events](28_forwarder-service-account.png)

This indicated that the forwarding pipeline was not yet working correctly.

### Step 11: Verifying Initial Event Ingestion

After continuing the configuration and troubleshooting process, events began appearing inside the `sharp-ad` index.

The first successful search showed over **1,000 Windows Security events** being indexed.

![First successful Windows event ingestion](30_first-ingested-events.png)

This confirmed that the Windows Universal Forwarder was successfully communicating with the Splunk server.

### Step 12: Troubleshooting the Domain Controller Forwarder

When installing the Universal Forwarder on the Domain Controller, I ran into a configuration issue.

During setup, I mistakenly entered the Domain Controller's VPC address:

```text
10.5.96.4
```

instead of the Splunk server's VPC address:

```text
10.5.96.5
```

The screenshot below captures the Universal Forwarder configuration during this troubleshooting process.

![Domain Controller Universal Forwarder configuration](31_splunk-9997-firewall.png)

After identifying the mistake, I corrected the forward server from the Domain Controller using PowerShell.

I removed the incorrect destination:

```powershell
& "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" remove forward-server 10.5.96.4:9997
```

I then added the correct Splunk server destination:

```powershell
& "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" add forward-server 10.5.96.5:9997
```

I restarted the SplunkForwarder service so the new configuration would take effect:

```powershell
Restart-Service SplunkForwarder
```

The Domain Controller's Universal Forwarder configuration directory contained the local configuration files, including `inputs.conf` and `outputs.conf`.

![Domain Controller Universal Forwarder configuration directory](32_domain-controller-forwarder-setup.png)

I then restarted the SplunkForwarder service.

![Restarting the SplunkForwarder service](33_domain-controller-inputs-conf.png)

This troubleshooting step was important because the forwarder must send events to the **Splunk server**, not to the machine generating the events.

### Step 13: Verifying Both Windows Hosts in Splunk

After correcting the Domain Controller's forwarding destination, I searched the `sharp-ad` index again.

Splunk now showed **1,208 events**.

More importantly, the `host` field contained two Windows systems:

- `vultr-guest`
- `sharp-ADDC01`

![Splunk showing telemetry from both Windows hosts](34_both-hosts-verified.png)

This confirmed that both the Windows test machine and Domain Controller were successfully forwarding Windows event logs into Splunk.

### Step 14: Researching Successful Windows Logons

With telemetry successfully reaching Splunk, I began researching Windows authentication events.

Windows Security Event ID **4624** represents a successful account logon.

![Windows Security Event ID 4624 reference](35_event-4624-reference.png)

I also reviewed the different Windows logon types to understand which authentication events would be relevant to Remote Desktop activity.

![Windows logon type reference](36_windows-logon-types.png)

Some important logon types include:

- **2 — Interactive:** Local console login
- **3 — Network:** Authentication over the network
- **7 — Unlock:** Unlocking an existing Windows session
- **10 — RemoteInteractive:** Remote Desktop or Terminal Services login

For RDP detection, **Logon Type 10** is particularly important.

### Step 15: Searching Successful Authentication Events

I first searched for successful Windows authentication events using Event ID 4624:

```spl
index=sharp-ad EventCode=4624
```

The query returned **259 successful logon events**.

![Splunk search for EventCode 4624](37_successful-logon-events.png)

### Step 16: Filtering Relevant Logon Types

I then narrowed the results to logon types **7** and **10**:

```spl
index=sharp-ad EventCode=4624 (Logon_Type=7 OR Logon_Type=10)
```

The filtered query returned **9 events**.

![Splunk filtering Logon Types 7 and 10](38_logon-type-7-event-details.png)

This reduced the number of events that needed to be reviewed and helped isolate activity related to workstation unlocks and remote interactive sessions.

### Step 17: Inspecting a Successful Logon Event

I opened one of the resulting events to review the raw Windows Security event information.

The event showed:

```text
Account Name: jsmith
Account Domain: SHARP
Logon Type: 7
```

![Windows Security event showing jsmith and Logon Type 7](39_filtered-logon-type-events.png)

Logon Type 7 represents a workstation unlock.

This helped confirm that not every successful Event ID 4624 result represented a new Remote Desktop connection.

For detecting RDP activity specifically, I will focus primarily on **Logon Type 10**.

### Part 4: Results

During this phase, I:

- Deployed Splunk Enterprise on the Ubuntu server.
- Configured Splunk Web access on TCP port 8000.
- Installed the Splunk Add-on for Microsoft Windows.
- Enabled Splunk receiving on TCP port 9997.
- Installed Splunk Universal Forwarders on the Windows systems.
- Configured Windows Security event collection.
- Created and used the `sharp-ad` index.
- Troubleshot an incorrect forwarding destination on the Domain Controller.
- Changed the forwarder target from `10.5.96.4:9997` to `10.5.96.5:9997`.
- Restarted the SplunkForwarder service.
- Verified event ingestion from both Windows systems.
- Investigated successful Windows Event ID 4624 activity.
- Filtered Windows logon events by Logon Types 7 and 10.
- Identified the difference between workstation unlock events and Remote Desktop logons.

### Next Steps

The next phase will focus on building detection logic for successful Remote Desktop authentication.

Planned work includes:

1. Filtering Event ID 4624 specifically for **Logon Type 10**.
2. Identifying the source network address associated with Remote Desktop sessions.
3. Distinguishing expected administrative connections from suspicious external connections.
4. Building a Splunk alert for successful unauthorized RDP authentication.
5. Preparing the detection for integration with Shuffle SOAR and Slack.