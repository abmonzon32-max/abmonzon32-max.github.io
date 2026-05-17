
# Infrastructure & Network Administration Portfolio
**Byron Armando Monzón Alman** | *Telecommunications Engineer*

---

## Project: Identity Automation & Workstation Hardening with Active Directory (AD DS)

###  Project Description
This project demonstrates the deployment of a virtualized corporate network infrastructure built on VMware Workstation. It utilizes Windows Server 2022 as the primary Domain Controller (`byronlab.local`) and a Windows 11 Enterprise host acting as the corporate workstation.

The core focus of this deployment is achieving operational efficiency through **Infrastructure as Code (PowerShell Automation)** and enforcing strict security baseline standards via **Workstation Hardening** using Group Policy Objects (GPOs).

###  Tech Stack & Core Tools
* **OS:** Windows Server 2022, Windows 11 Enterprise
* **Directory Services:** Active Directory Domain Services (AD DS), Integrated DNS
* **Automation:** PowerShell Scripting
* **Security:** Group Policy Objects (GPOs), Least Privilege Enforcement
* **Virtualization:** VMware Workstation (Isolated LAN Segmentation)

---

###  Implementation Phases

#### 1. Network Infrastructure & Virtual Lab Setup
Both virtual machines were isolated within a dedicated custom LAN segment in VMware to mimic a secure corporate access layer. Static IP addressing was assigned to the Domain Controller, and DNS forwarders were structured so that the Windows 11 host could natively resolve the hierarchical AD namespace.

#### 2. Directory Deployment Automation via PowerShell
To minimize manual errors and optimize deployment times, a custom PowerShell script was executed to automatically build the root Organizational Unit (`Byron_Corp`), its functional sub-OUs (`Users`, `Teams`, `Groups`), the departmental sub-OU (`Engineering`), and provision a technical user account for **John White** (`jwhite`) with a forced password reset on initial logon:

```powershell
# Initialize Root and Sub-OU Structure
New-ADOrganizationalUnit -Name "Byron_Corp" -Path "DC=byronlab,DC=local"
$subOUs = @("Users", "Teams", "Groups")
foreach ($ou in $subOUs) {
    New-ADOrganizationalUnit -Name $ou -Path "OU=Byron_Corp,DC=byronlab,DC=local"
}
New-ADOrganizationalUnit -Name "Engineering" -Path "OU=Users,OU=Byron_Corp,DC=byronlab,DC=local"

# Secure Technical User Provisioning
$password = ConvertTo-SecureString "Password123!" -AsPlainText -Force
New-ADUser -Name "John White" -SamAccountName "jwhite" `
           -UserPrincipalName "jwhite@byronlab.local" `
           -Path "OU=Engineering,OU=Users,OU=Byron_Corp,DC=byronlab,DC=local" `
           -AccountPassword $password -Enabled $true -ChangePasswordAtLogon $true
