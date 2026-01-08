# Cloud SIEM & EDR Security Project using Wazuh
## Multi-Platform Security Monitoring on AWS

---

##  Project Overview

This project demonstrates the deployment of a **Cloud-based SIEM and EDR architecture** using **Wazuh** on **Amazon Web Services (AWS)**.  
The platform monitors **Linux and Windows systems** in real time, collects security logs, detects suspicious behavior, and generates alerts.

The infrastructure is deployed on **AWS Learner Lab** and follows basic **Cloud security best practices**.

---

##  Project Goals

- Deploy a centralized SIEM & EDR platform
- Monitor heterogeneous systems (Linux & Windows)
- Collect and analyze security events
- Detect authentication and privilege escalation attacks
- Apply IAM & PAM security monitoring concepts
- Understand Cloud network security (VPC, Subnet, Security Groups)
- Validate detection through real attack scenarios

---

##  Cloud Architecture

The lab architecture consists of:

- **1 VPC**
- **1 Public Subnet**
- **3 EC2 Instances**
- **Internet Gateway**
- **Security Groups**

All EC2 instances are deployed in the **same VPC and subnet**, as required by the project specification.

---

## 🖥️ EC2 Instances Description

### 🔐 Wazuh Server (Ubuntu)

- OS: Ubuntu 22.04 LTS
- Role: SIEM & EDR Server
- Components:
  - Wazuh Manager
  - Wazuh Indexer
  - Wazuh Dashboard
- Access:
  - SSH (22)
  - HTTPS Dashboard (443)

This instance centralizes all logs and security events.

---

###  Linux Client (Ubuntu)

- OS: Ubuntu 22.04
- Role: Monitored endpoint
- Wazuh Agent installed
- Monitored activities:
  - SSH logins
  - sudo usage
  - System logs
  - File Integrity Monitoring (FIM)

---

###  Windows Client (Windows Server)

- OS: Windows Server
- Role: Monitored endpoint
- Wazuh Agent installed
- Monitored events:
  - Windows Security Logs
  - Failed login attempts
  - User and group management
  - Privileged account changes

---

##  Network Security Design

Security is enforced using **AWS Security Groups**.

### Allowed Traffic

| Source | Destination | Port | Description |
|------|------------|------|------------|
| Admin IP | EC2 Instances | 22 | SSH Access |
| Admin IP | Windows EC2 | 3389 | RDP Access |
| Linux Agent | Wazuh Server | 1514 | Agent Logs |
| Windows Agent | Wazuh Server | 1514 | Agent Logs |
| Agents | Wazuh Server | 1515 | Agent Enrollment |
| Admin IP | Wazuh Server | 443 | Dashboard Access |

All other traffic is blocked.

---

##  Wazuh Installation

The Wazuh platform is installed in **All-in-One mode**.

### Installation Commands (Wazuh Server)

```bash
sudo apt update && sudo apt upgrade -y
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
sudo bash wazuh-install.sh -a
