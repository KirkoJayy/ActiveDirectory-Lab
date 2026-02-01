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
