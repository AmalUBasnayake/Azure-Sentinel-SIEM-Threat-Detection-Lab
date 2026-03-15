# 🛡️ Azure Sentinel Security Monitoring Lab

![Azure](https://img.shields.io/badge/Azure-Security-blue?logo=microsoftazure)
![Sentinel](https://img.shields.io/badge/SIEM-Microsoft%20Sentinel-purple)
![Cybersecurity](https://img.shields.io/badge/Domain-Cybersecurity-red)
![Platform](https://img.shields.io/badge/Platform-Azure-blue)
![Logs](https://img.shields.io/badge/Logs-Windows%20Security%20Events-green)
![Status](https://img.shields.io/badge/Project-Lab-success)

---

# 📌 Overview

This project demonstrates how to build a **Security Monitoring Lab using Microsoft Sentinel in Microsoft Azure**.

The lab simulates a **real-world Security Operations Center (SOC) environment** where logs are collected from a Windows server and analyzed using Microsoft Sentinel.

The lab includes:

- Deploying a **Windows Server Virtual Machine**
- Creating a **Log Analytics Workspace**
- Enabling **Microsoft Sentinel**
- Installing **Azure Monitor Agent (AMA)**
- Configuring **Windows Security Events Connector**
- Creating **Data Collection Rules (DCR)**
- Monitoring **Security Logs with KQL queries**

---

# 🏗️ Lab Architecture

Azure VM → Azure Monitor Agent → Log Analytics Workspace → Microsoft Sentinel → Log Analytics Queries → Security Investigation

This architecture allows security analysts to monitor authentication events, suspicious logins, and other Windows security activities.

---

# ⚙️ Lab Deployment Steps

---

# 1️⃣ Create Log Analytics Workspace

![Log Analytics Workspace](images/Log-Analytics.png)

A **Log Analytics Workspace** is created to store and analyze logs collected from Azure resources.

Configuration:

Workspace Name: `Amal-Sentinel-WS`  
Region: `Central US`  
Resource Group: `AZ500-Sentinel-Lab-RG`

This workspace acts as the **central logging repository for Microsoft Sentinel**.

---

# 2️⃣ Enable Microsoft Sentinel

![Add Sentinel](images/add-Microsoft-Sentinel.png)

Microsoft Sentinel is enabled on the created Log Analytics Workspace.

Sentinel is a **cloud-native SIEM (Security Information and Event Management) and SOAR platform** used for security monitoring and threat detection.

---

# 3️⃣ Deploy Target Virtual Machine

![Target VM](images/create-VM.png)

A **Windows Server Virtual Machine** is deployed to simulate endpoint activity.

Configuration:

VM Name: `Sentinel-Target-VM`  
Operating System: Windows Server  
VM Size: Standard D2s v4  
Public IP: Enabled

This machine will generate **Windows Security Events for monitoring**.

---

# 4️⃣ Install Azure Monitor Agent

![Azure Monitor Agent](images/Sentinel-Target-VM-Extensions+applications.png)

The **Azure Monitor Agent (AMA)** is installed on the virtual machine using VM Extensions.

This agent is responsible for sending system and security logs to Azure Monitor and Microsoft Sentinel.

---

# 5️⃣ Install Windows Security Events Solution

![Windows Security Events Solution](images/Windows-Security-Events-install.png)

The **Windows Security Events Solution** is installed from the **Microsoft Sentinel Content Hub**.

This enables the collection of important Windows security logs including:

- Login attempts  
- Account activity  
- Authentication logs  
- Security audit logs  

---

# 6️⃣ Configure Windows Security Events Connector

![Data Connector](images/MSDC.png)

The **Windows Security Events via AMA** data connector is configured.

This connector allows Windows machines to securely stream **Security Event Logs directly to Microsoft Sentinel**.

---

# 7️⃣ Create Data Collection Rule

![Create Data Collection Rule](images/create-data-connection-rule.png)

A **Data Collection Rule (DCR)** defines what data should be collected from connected machines.

Configuration:

Rule Name: `Sentinel-VM-Logs`  
Target Machine: `Sentinel-Target-VM`  
Event Collection: `All Security Events`

---

# 8️⃣ Assign Data Collection Rule to VM

![Assign VM to DCR](images/set-VM-MSDC.png)

The target VM is assigned to the created Data Collection Rule.

This step allows the VM to start **sending Windows Security Events to Microsoft Sentinel**.

---

# 📊 Security Log Monitoring

![Sentinel Logs](images/logs.png)

Once logs start arriving, they can be analyzed using **Kusto Query Language (KQL)** inside Microsoft Sentinel.

Example query to detect failed login attempts:

```kql
SecurityEvent
| where EventID == 4625
| project TimeGenerated, Computer, Account, IpAddress, EventID
