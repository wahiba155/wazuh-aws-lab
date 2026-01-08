# Cloud-Based SIEM & EDR Security Architecture using Wazuh
## Multi-OS Security Monitoring on AWS (Linux & Windows)

---

##  Project Overview

This project presents the design and implementation of a **Cloud-based SIEM & EDR security architecture** using **Wazuh**, deployed on **Amazon Web Services (AWS Learner Lab)**.

The platform provides centralized security monitoring for **Linux and Windows endpoints**, enabling real-time detection, analysis, and visualization of security events.  
The architecture simulates a **modern Security Operations Center (SOC)** in a realistic cloud environment.



---

##  Project Objectives

The main goals of this project are to:

- Deploy a centralized **SIEM & EDR platform**
- Monitor heterogeneous environments (Linux & Windows)
- Collect, index, and analyze security logs
- Detect authentication failures and privilege escalation
- Apply **IAM & PAM monitoring concepts**
- Implement **AWS network security best practices**
- Validate detections using realistic attack scenarios

---

##  Global Cloud Architecture

The laboratory infrastructure is deployed entirely on **AWS** and consists of:

- **1 Virtual Private Cloud (VPC)**
- **1 Public Subnet**
- **3 EC2 Instances**
- **1 Internet Gateway**
- **3 Security Groups**

All instances are deployed in the **same VPC and subnet** to ensure seamless internal communication while maintaining strict network security using Security Groups.

---

##  EC2 Instances Description

###  Wazuh Server (SIEM & EDR)

- **Operating System**: Ubuntu 22.04 LTS
- **Instance Type**: t3.large
- **Role**: Central SIEM & EDR Server
- **Components**:
  - Wazuh Manager
  - Wazuh Indexer
  - Wazuh Dashboard
- **Access**:
  - SSH (TCP 22)
  - HTTPS Dashboard (TCP 443)

This instance acts as the **central security brain**, receiving, correlating, and visualizing all security events.

---

###  Linux Client

- **Operating System**: Ubuntu 22.04 LTS
- **Instance Type**: t3.micro
- **Role**: Monitored Linux Endpoint
- **Installed Component**:
  - Wazuh Agent

**Monitored activities**:
- SSH authentication attempts
- sudo privilege escalation
- System logs
- File Integrity Monitoring (FIM)

---

###  Windows Client

- **Operating System**: Windows Server
- **Instance Type**: t3.medium
- **Role**: Monitored Windows Endpoint
- **Installed Components**:
  - Wazuh Agent
  - (Optional) Sysmon for enhanced EDR telemetry

**Monitored events**:
- Windows Security Events
- Failed login attempts (Event ID 4625)
- User and group management
- Administrative privilege changes

---

##  Network Security Design

Network security is enforced using **AWS Security Groups**, following the **principle of least privilege**.

### 🔸 Wazuh Server Security Group

**Inbound rules**:
- SSH (22) → Admin public IP
- HTTPS (443) → Admin public IP (Dashboard access)
- TCP 1514 → Linux & Windows Security Groups (Agent communication)
- TCP 1515 → Linux & Windows Security Groups (Agent enrollment)

All other inbound traffic is denied.

---

### 🔸 Linux Client Security Group

**Inbound rules**:
- SSH (22) → Admin public IP

---

### 🔸 Windows Client Security Group

**Inbound rules**:
- RDP (3389) → Admin public IP

---

##  Network Communication Flows

| Source | Destination | Port | Purpose |
|--------|-------------|------|---------|
| Linux Agent | Wazuh Server | 1514 | Log transmission |
| Windows Agent | Wazuh Server | 1514 | Log transmission |
| Linux & Windows | Wazuh Server | 1515 | Agent enrollment |
| Admin | Wazuh Server | 443 | Dashboard access |
| Admin | Linux Client | 22 | SSH |
| Admin | Windows Client | 3389 | RDP |

---

##  Wazuh Installation (All-in-One)

Wazuh is installed in **All-in-One mode**, hosting all components on the same server.

### Installation Commands

```bash
sudo apt update && sudo apt upgrade -y
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

At the end of the installation, the script provides:

- Dashboard URL (HTTPS)
- Admin username
- Admin password

---

##  Agent Enrollment

###  Linux Agent

- Enrolled via Wazuh Dashboard
- Commands generated automatically
- Agent starts sending logs immediately

###  Windows Agent

- Installed using PowerShell
- MSI package from Wazuh repository
- Runs as a Windows service

Both agents appear as **Active** in the Dashboard after installation.

---

##  SIEM & EDR Demonstration Scenarios

### 🔹 Linux Scenarios (SIEM)

- Multiple failed SSH login attempts (brute force simulation)
- Privilege escalation using sudo
- Modification of sensitive files (FIM)

### 🔹 Windows Scenarios (EDR)

- Failed RDP authentication attempts (Event ID 4625)
- Creation of a local user account
- Adding a user to the Administrators group

These scenarios generate real-time alerts visible in the Wazuh Dashboard.

---

##  Monitoring & Threat Hunting

Using the Wazuh Dashboard:

- Filter events by agent
- Analyze authentication failures
- Detect suspicious privilege escalation
- Perform basic threat hunting

---


##  Author

**Wahiba Moussaoui**  
Engineering Student – GLSID  
ENSET Mohammedia  
Academic Year: 2025 / 2026

**Supervisor**: Prof. Azeddine KHIAT

---



##  Acknowledgments

- ENSET Mohammedia
- Prof. Azeddine KHIAT
- Wazuh Community
- AWS Learner Lab Program
