# Active Directory Homelab with PowerShell Automation

## Overview
This project demonstrates the deployment and management of a Windows Active Directory (AD) environment in a virtualized homelab, with a focus on **identity and access management**, **role-based access control**, and **PowerShell automation**. The lab simulates real-world enterprise workflows including bulk user provisioning, Group Policy enforcement, and secure file access management.

The environment was built using VirtualBox and Windows Server, with automation scripts leveraged to streamline administrative tasks and reduce manual configuration errors.

---

## Objectives
- Deploy a functional Active Directory Domain Services (AD DS) environment
- Automate bulk user creation using PowerShell
- Implement Organizational Units (OUs) and security groups
- Enforce Group Policy Objects (GPOs) for security baselines
- Configure file shares with NTFS and share-level access control
- Validate authentication, authorization, and auditing
- Document evidence suitable for SOC, IAM, and GRC use cases

---

## Lab Environment

### Virtual Machines
| VM Name | OS | Purpose |
|------|---|--------|
| DC01 | Windows Server 2019/2022 | Domain Controller, DNS, File Server |
| CLIENT01 | Windows 10/11 | Domain-joined workstation |

### Domain Configuration
- **Domain Name:** `DC01`
- **DNS:** Integrated with Active Directory
- **Authentication:** Kerberos
- **Directory Services:** AD DS

---

## Tools & Technologies
- Windows Server Active Directory
- PowerShell (AD module)
- VirtualBox
- NTFS & SMB File Sharing
- Group Policy Management
- Event Viewer (Security Auditing)

---

## PowerShell Automation

### Bulk User Creation
A PowerShell script was used to automate the creation of 1000+ Active Directory users by parsing a CSV file and executing the script.

**Key automation features:**
- Bulk user provisioning
- Standardized naming conventionst
- Group membership assignment
- Reduced administrative overhead

**Example functionality:**
```powershell
Import-Module ActiveDirectory
Import-Csv users.csv | ForEach-Object {
    New-ADUser `
        -Name "$($_.FirstName) $($_.LastName)" `
        -SamAccountName $_.Username `
        -UserPrincipalName "$($_.Username)@corp.local" `
        -Path "OU=Users,DC=corp,DC=local" `
        -AccountPassword (ConvertTo-SecureString "P@ssw0rd!" -AsPlainText -Force) `
        -Enabled $true
}
```
## Step-by-Step Implementation

---

### Step 1: Virtualized Network & VM Configuration
Configured VirtualBox virtual machines using isolated internal networking to simulate an enterprise domain environment while maintaining controlled external access where required.

Domain Controller Network Config: <br/>
<img src="https://i.imgur.com/xgMIrZm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
CLIENT01 Workstation Config:  <br/>
<img src="https://i.imgur.com/tHZBlZi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

---

### Step 2: Active Directory Domain Deployment
Installed and configured Active Directory Domain Services and DNS on DC01. Promoted the server to a domain controller and created the `mydomain.com` domain.

Active Directory Domain Services: <br/>
<img src="https://i.imgur.com/9oFhLJq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Creation of Domain:  <br/>
<img src="https://i.imgur.com/GpXaiJL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

---

### Step 3: DNS Configuration & Validation
Verified DNS functionality and required `_msdcs` records to ensure proper Active Directory replication and authentication.

DNS functionality and '_msdcs' records:  <br/>
<img src="https://i.imgur.com/bnzOpye.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

---

### Step 4: Organizational Unit (OU) Structure
Created Organizational Units to logically separate users, groups, workstations, and servers, supporting scalable identity and access management.

OU Structure:  <br/>
<img src="https://i.imgur.com/62tPKFO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

---

### Step 5: PowerShell Automation – Bulk User Creation
Developed and executed a PowerShell script to automate bulk user provisioning in Active Directory. Users were created from a CSV file and automatically placed into appropriate OUs and security groups.

Powershell Script:  <br/>
<img src="https://i.imgur.com/d76hBIe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Bulk Automated User Creation (1000+ Users):  <br/>
<img src="https://i.imgur.com/vKepNnY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

---

### Step 6: Security Groups & Role-Based Access Control
Created global security groups to enforce role-based access control (RBAC). Group membership was used to manage permissions instead of assigning access directly to users.

Global Security Groups + RBAC:  <br/>
<img src="https://i.imgur.com/O4OeeYa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

---

### Step 7: Domain Join & Client Validation
Joined CLIENT01 to the `DC01` domain and verified successful authentication and Group Policy application.

CLIENT1 joined mydomain.com:  <br/>
<img src="https://i.imgur.com/xiSJs7X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

---

### Step 8: Group Policy Configuration
Configured Group Policy Objects to enforce domain security baselines, including account policies, audit policies, and workstation security settings.

Group Policy Config:  <br/>
<img src="https://i.imgur.com/iK8du7A.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

---

### Step 9: File Shares & NTFS Access Control
Implemented shared folders on DC01 and configured NTFS and SMB permissions using AD security groups, enforcing least privilege access.

NTFS Permissions Config:  <br/>
<img src="https://i.imgur.com/BN8FUXi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Access Test from CLIENT1:  <br/>
<img src="https://i.imgur.com/npGR940.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

---

### Step 10: File Access Auditing
Enabled auditing for file system access and validated security events in Event Viewer to support compliance and incident investigation use cases.

Validated file access logs via Event Viewer:  <br/>
<img src="https://i.imgur.com/qQSk8gG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

---

## Validation Summary
- Domain authentication and authorization verified
- PowerShell automation confirmed successful user provisioning
- RBAC enforced through security groups
- File access permissions tested across user roles
- Auditing validated via security event logs
- osTicket successfully integrated with Active Directory

---

## Security & IAM Concepts Demonstrated
- Centralized authentication (Kerberos)
- Role-Based Access Control (RBAC)
- Least privilege enforcement
- Identity lifecycle management
- Audit logging and evidence collection
- Automation in IT and security operations

---

## Future Enhancements
- PowerShell-based Group Policy deployment
- Access request approvals via osTicket
- SIEM integration (Splunk)
- SOC 2 / NIST control mapping
- MFA integration with directory services

---

## Author
**JKirk**  
Computer Science Graduate | Security+ | Splunk Core Certified User  
Aspiring Security Analyst / IAM / GRC Professional

---

## Disclaimer
This project was built for educational and portfolio purposes and simulates enterprise environments using non-production systems.


