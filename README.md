# Enterprise Active Directory & Identity Management Lab (Microsoft Azure)

## 📌 Project Overview
This project demonstrates the end-to-end deployment, configuration, and administration of an on-premises-style **Windows Server 2022 Active Directory Domain Controller** hosted in **Microsoft Azure**. The lab simulates core Tier 1 and Tier 2 IT Help Desk operations, including directory structuring, role-based access control (RBAC), account lifecycle management, Group Policy enforcement, and cloud infrastructure troubleshooting.

---

## 🛠️ Technologies & Tools Used
* **Cloud Infrastructure:** Microsoft Azure (Virtual Machines, Virtual Networks, Azure Bastion, Resource Groups)
* **Operating System:** Windows Server 2022 Datacenter (Azure Edition)
* **Identity & Access Management:** Active Directory Domain Services (AD DS), Active Directory Users and Computers (ADUC)
* **Configuration Management:** Group Policy Management Console (GPMC)
* **Remote Administration:** Azure Bastion (Browser-based RDP over TLS)

---

## 🏗️ Architecture & Environment Setup
* **Domain Name:** `lab.local` (NetBIOS: `LAB`)
* **Server Hostname:** `AD-DC-01`
* **VM Size & OS:** `Standard_B2ats_v2` running Windows Server 2022 Datacenter
* **Network Topology:** Isolated Azure Virtual Network (`vnet-northcentralus-1`) with dedicated `AzureBastionSubnet` for secure, zero-client remote desktop administration.

---

## 🚀 Step-by-Step Implementation

### Phase 1: Cloud Provisioning & Remote Access
1. Provisioned a Windows Server 2022 Virtual Machine in Microsoft Azure (`North Central US`).
2. Configured inbound network security rules and deployed **Azure Bastion** to establish secure RDP access directly via web browser without exposing local client dependencies.
3. Verified VM Guest Agent health and initialized Server Manager.

### Phase 2: Domain Controller Promotion
1. Installed the **Active Directory Domain Services (AD DS)** server role along with RSAT management tools via Server Manager.
2. Promoted `AD-DC-01` to a Primary Domain Controller by creating a new Active Directory forest (`lab.local`), configuring DNS server integration, and setting a Directory Services Restore Mode (DSRM) recovery password.

### Phase 3: Directory Structure & RBAC (OUs, Users, & Groups)
1. Designed a department-based **Organizational Unit (OU)** hierarchy inside Active Directory Users and Computers (ADUC) to separate administrative boundaries:
   * `IT`
   * `HR`
   * `Sales`
2. Provisioned employee user accounts across each department OU following enterprise naming conventions.
3. Created department-specific **Security Groups** (`GRP-IT`, `GRP-HR`, `GRP-SALES`) and assigned corresponding users to enforce the **Principle of Least Privilege**.

![Active Directory Users and Computers Structure](images/aduc-structure.png)

### Phase 4: Simulated Help Desk Ticket Operations
Executed standard Tier 1 support workflows to simulate daily help desk ticket resolution:
* **Password Resets:** Performed administrative password resets with *"User must change password at next logon"* enforced.
* **Account Lockout Recovery:** Simulated failed login lockouts and restored user access.
* **Employee Offboarding:** Disabled user accounts and removed group memberships to revoke access while preserving audit history.

### Phase 5: Group Policy Object (GPO) Enforcement
1. Opened **Group Policy Management** and created a custom GPO titled **`Block Control Panel`**.
2. Configured the policy under `User Configuration > Policies > Administrative Templates > Control Panel > Prohibit access to Control Panel and PC settings` (**Enabled**).
3. Linked the GPO directly to the **`Sales`** OU to restrict workstation system changes for non-administrative staff without impacting `IT` administrators.

![Group Policy Management Console](images/gpo-configuration.png)

---

## 🧩 Troubleshooting & Lessons Learned
During deployment, I encountered and resolved several real-world cloud infrastructure challenges:
* **Azure Policy Region Restrictions:** Initial VM deployments failed validation due to subscription-level location policies. Audited Azure Policy assignments (`listOfAllowedLocations`) and redeployed into an authorized region (`North Central US`).
* **Azure Bastion Provisioning & Subnet Locks:** Encountered an `ajaxExtended` / resource lock error during one-click Bastion setup. Resolved the issue by verifying backend provisioning states and ensuring the required `/26` `AzureBastionSubnet` was properly allocated.
* **Cloud Cost Optimization (FinOps):** Identified that the default Bastion SKU incurs continuous hourly charges even when the VM is stopped. Implemented resource lifecycle hygiene by deallocating the VM (`Stopped (deallocated)`) and removing the Bastion gateway after lab sessions to protect cloud credits.

---

## 🎯 Key Skills Demonstrated
* Active Directory Domain Services (AD DS) installation & DNS configuration
* User onboarding, offboarding, password management, and RBAC security grouping
* Group Policy Object (GPO) creation, linking, and scope management
* Cloud VM provisioning, virtual networking, and cost-control governance
