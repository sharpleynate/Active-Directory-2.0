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
