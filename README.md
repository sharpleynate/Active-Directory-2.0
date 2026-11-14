# Active-Directory-2.0
Simulated enterprise SOC environment using Active Directory, Splunk SIEM, and Shuffle SOAR for threat detection and automated incident response.

<img width="1774" height="887" alt="37466940-7507-473c-925d-506738b7344a" src="https://github.com/user-attachments/assets/255c5c35-5cc3-45e3-931b-631869d91ac6" />
### Active Directory 2.0 — Part 1: Project Overview

In this project, I'm building an Active Directory environment to simulate real-world cyberattacks and automate security responses.

**The setup consists of three virtual machines hosted on Vultr:**

1. **Domain Controller (Windows Server):** Manages domain users and accounts.
2. **Splunk Server (Ubuntu):** Collects logs and detects suspicious activity.
3. **Windows Test Machine:** Simulates a vulnerable machine that attackers can target.

**How it works:**

An attacker attempts to log into the Windows machine. Splunk collects the activity and generates an alert. Shuffle then automates the response by notifying a SOC analyst, who decides whether the compromised account should be disabled.

**The goal:** Build a hands-on SOC environment that combines Active Directory, SIEM monitoring, and SOAR automation.

In this first part, I'm focusing on understanding the architecture and setting up the infrastructure.
