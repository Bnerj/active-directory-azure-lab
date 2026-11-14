# Enterprise Active Directory & Identity Management Lab (Microsoft Azure)

## 📌 Project Overview
This project demonstrates the end-to-end deployment, configuration, and administration of an on-premises-style **Windows Server 2022 Active Directory Domain Controller** hosted in **Microsoft Azure**. The lab simulates core Tier 1 and Tier 2 IT Help Desk operations, including directory structuring, role-based access control (RBAC), account lifecycle management, Group Policy enforcement, and cloud infrastructure troubleshooting.

---

## 🛠️ Environment & Architecture Specifications
* **Cloud Provider:** Microsoft Azure (Azure for Students Subscription)
* **Resource Group:** `AD-DC-01_group`
* **Region:** North Central US (`northcentralus`)
* **Virtual Machine:** `AD-DC-01` (`Standard_B2ats_v2`, 2 vCPUs, Windows Server 2022 Datacenter: Azure Edition)
* **Virtual Network:** `vnet-northcentralus-1` with dedicated `AzureBastionSubnet` (`/26`)
* **Remote Access:** Azure Bastion (TLS-encrypted RDP over HTML5 in Chrome)
* **Active Directory Domain:** `lab.local` (NetBIOS: `LAB`)

---

## 📋 Part 1: Azure Infrastructure & Policy Troubleshooting

### Step 1: Auditing Subscription Region Restrictions
* **Action:** Navigated to **Azure Policy > Authoring > Assignments** and inspected the `Allowed resource deployment regions` (`listOfAllowedLocations`) policy parameter.
* **Outcome:** Identified that the student subscription restricted deployments strictly to five regions (`canadacentral`, `westus`, `norwayeast`, `francecentral`, `northcentralus`), resolving initial validation errors.

### Step 2: Configuring Virtual Machine Basics & Sizing
* **Action:** Created a new Virtual Machine named `AD-DC-01` inside resource group `AD-DC-01_group` in **North Central US**. Selected **Windows Server 2022 Datacenter: Azure Edition**, set size to **`Standard_B2ats_v2`**, and set Availability options to **No infrastructure redundancy required**.
* **Outcome:** Configured a cost-effective VM profile (`$0.0186/hr`) compliant with student subscription quotas.
![Step 2 - VM Basics Configuration](images/02-vm-basics.png)

### Step 3: Configuring Inbound Ports, Disks, & Auto-Shutdown
* **Action:** Enabled inbound **RDP (3389)** for administrative testing, configured managed OS storage, and enabled **Auto-shutdown** under the Management tab.
* **Outcome:** Protected cloud credits against accidental overnight runtime charges.
![Step 3 - VM Networking and Management](images/03-vm-management.png)

### Step 4: Reviewing & Deploying the Virtual Machine
* **Action:** Validated all configuration parameters on the **Review + create** screen and initiated deployment.
* **Outcome:** Provisioned `AD-DC-01` along with its Virtual Network (`vnet-northcentralus-1`), Subnet, Network Security Group, and Public IP (`AD-DC-01-ip`).
![Step 4 - VM Deployment Complete](images/04-vm-deployed.png)

### Step 5: Verifying VM Guest Agent Health
* **Action:** Monitored the VM Overview blade as the **Windows Azure Guest Agent** transitioned from `Not ready` during initial boot to `Ready`.
* **Outcome:** Confirmed the server OS completed initialization and was ready for remote management.
![Step 5 - VM Overview and Agent Status](images/05-vm-status.png)

---

## 🌐 Part 2: Remote Access via Azure Bastion (Zero-Client Setup)

### Step 6: Troubleshooting One-Click Bastion Provisioning
* **Action:** Attempted to connect via **Connect > Bastion** from a macOS Chrome browser without local RDP client software. Encountered an `ajaxExtended` / resource lock error (`Another operation on this or dependent resource is in progress`) while Azure provisioned the `AzureBastionSubnet` in the background.
* **Outcome:** Monitored the Resource Group deployment status until the Bastion gateway finished provisioning (`Succeeded`).
![Step 6 - Bastion Provisioning](images/06-bastion-deploy.png)

### Step 7: Establishing Browser-Based RDP Session
* **Action:** Entered local administrator credentials (`labadmin`) into the Azure Bastion portal blade and launched an HTML5 remote session in Chrome.
* **Outcome:** Successfully connected to the Windows Server 2022 desktop and launched **Server Manager**.
![Step 7 - Server Manager Dashboard](images/07-server-manager.png)

---

## 🏢 Part 3: Active Directory Domain Services (AD DS) Deployment

### Step 8: Launching the Add Roles and Features Wizard

* **Step 1:** Open **Server Manager** dashboard, Inside Server Manager, select **Manage** (top right), select **Add Roles and Features** and chose **Role-based or feature-based installation** targeting `AD-DC-01`.

<img width="1467" height="956" alt="S1_SERVER_DASH" src="https://github.com/user-attachments/assets/0dd2cdf5-73fd-4799-b062-9c2281a99235" />

* **Step 2:** Clicked **Next** through the initial screens, accept the required **Remote Server Administration Tools (RSAT)** features, enabled automatic restart if required. 

<img width="1470" height="920" alt="S2_NEXT" src="https://github.com/user-attachments/assets/61f8ee06-6e87-459c-9f2f-6604be344bf0" />

### Step 9: Installing the AD DS Server Role
* **Step 1:** Selected **Active Directory Domain Services**. 

<img width="1470" height="956" alt="S3_ACTIVE_DIRECT" src="https://github.com/user-attachments/assets/17ba6e71-42d7-4942-9c2e-c8f788bbff7f" />

* **Step 2:** Accept the required **Remote Server Administration Tools (RSAT)** features, enabled automatic

<img width="1235" height="783" alt="S4" src="https://github.com/user-attachments/assets/e42618bd-012e-4882-bb30-321804b2febc" />

<img width="1227" height="748" alt="S5" src="https://github.com/user-attachments/assets/50689321-fe3c-48a8-b52c-bf5756b9bb6a" />

* **Step 4:** Restart if required, and clicked **Install**.

<img width="1231" height="749" alt="Screenshot 2026-11-14 at 2 36 07 PM" src="https://github.com/user-attachments/assets/4938f2d1-08aa-4ded-a3b1-f489b330b1af" />

### Step 10: Promoting `AD-DC-01` to a Domain Controller
* **Action:** Clicked the post-deployment notification flag in Server Manager and selected **Promote this server to a domain controller**.
* **Configuration:**
  * Selected **Add a new forest** and specified Root domain name: **`lab.local`**
  * Configured Forest/Domain functional levels, integrated **DNS Server** and **Global Catalog (GC)** roles, and set a **Directory Services Restore Mode (DSRM)** password.
  * Passed all Prerequisites Checks and clicked **Install**.
![Step 11 - Deployment Configuration Forest](images/11-dc-forest.png)
![Step 12 - Domain Controller Options DSRM](images/12-dc-options.png)
![Step 13 - Prerequisites Check Passed](images/13-dc-prereq.png)

### Step 11: Verifying Domain Controller Promotion
* **Action:** Reconnected via Azure Bastion after automatic server reboot and verified `AD-DC-01` was operating as the Primary Domain Controller for `LAB`.
![Step 14 - Server Manager Post-Promotion](images/14-dc-verified.png)

---

## 👥 Part 4: Directory Structuring & RBAC (OUs, Users, & Groups)

### Step 12: Creating Department Organizational Units (OUs)
* **Action:** Opened **Server Manager > Tools > Active Directory Users and Computers (ADUC)**, right-clicked **`lab.local`**, and created three top-level Organizational Units to separate administrative boundaries:
  * **`IT`**
  * **`HR`**
  * **`Sales`**
![Step 15 - ADUC Organizational Units](images/15-aduc-ous.png)

### Step 13: Provisioning Employee User Accounts
* **Action:** Created two simulated employee user accounts inside each department OU (6 users total), configuring user logon names (`@lab.local`) and initial domain passwords.
![Step 16 - Creating Domain Users](images/16-create-users.png)

### Step 14: Creating Department Security Groups
* **Action:** Created a dedicated **Global Security Group** inside each respective OU to enforce Role-Based Access Control (RBAC):
  * **`GRP-IT`** (inside `IT` OU)
  * **`GRP-HR`** (inside `HR` OU)
  * **`GRP-SALES`** (inside `Sales` OU)
![Step 17 - Creating Security Groups](images/17-security-groups.png)

### Step 15: Assigning Users to Department Security Groups
* **Action:** Opened each Security Group's **Properties > Members** tab, clicked **Add**, validated employee names via **Check Names**, and linked all users to their corresponding department groups (`GRP-IT`, `GRP-HR`, `GRP-SALES`).
![Step 18 - Adding Members to Security Groups](images/18-group-members.png)

---

## 🎫 Part 5: Simulated Tier 1 Help Desk Operations

### Step 16: Executing Password Resets
* **Action:** Simulated a forgotten-password support ticket in ADUC by right-clicking a user account, selecting **Reset Password**, and enforcing **"User must change password at next logon"**.
![Step 19 - Help Desk Password Reset](images/19-password-reset.png)

### Step 17: Simulating Employee Offboarding (Disabling Accounts)
* **Action:** Simulated an HR termination ticket by right-clicking an employee account and selecting **Disable Account** to immediately revoke domain authentication while retaining audit logs.
![Step 20 - Disabling Offboarded Account](images/20-disable-account.png)

---

## 🛡️ Part 6: Group Policy Object (GPO) Enforcement

### Step 18: Creating & Linking a Department GPO
* **Action:** Opened **Server Manager > Tools > Group Policy Management**, expanded `Forest: lab.local > Domains > lab.local`, right-clicked the **`Sales`** OU, and selected **Create a GPO in this domain, and Link it here**. Named the GPO **`Block Control Panel`**.
![Step 21 - Creating and Linking GPO](images/21-gpo-create.png)

### Step 19: Configuring Control Panel Restrictions
* **Action:** Right-clicked **`Block Control Panel`**, selected **Edit** to open the Group Policy Management Editor, and navigated to:
  `User Configuration > Policies > Administrative Templates > Control Panel`
* **Configuration:** Opened **Prohibit access to Control Panel and PC settings**, set it to **Enabled**, and clicked **Apply > OK**.
![Step 22 - GPO Editor Path](images/22-gpo-editor.png)
![Step 23 - Enabling Control Panel Restriction](images/23-gpo-enabled.png)

### Step 20: Verifying GPO Scope & Inheritance
* **Action:** Confirmed in the Group Policy Management Console that **`Block Control Panel`** was actively linked and enforced exclusively on the **`Sales`** OU without affecting `IT` or `HR` users.
![Step 24 - GPO Scope Verification](images/24-gpo-scope.png)

---

## 💰 Part 7: Cloud Cost Optimization & Resource Cleanup

### Step 21: Deleting High-Cost Bastion Gateway & Deallocating VM
