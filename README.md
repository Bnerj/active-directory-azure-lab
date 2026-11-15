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

### Step 1: 
* **Action:** Created a new Virtual Machine in Azure Portal. 

<img width="1470" height="833" alt="S1" src="https://github.com/user-attachments/assets/983e63ef-27dd-4196-a752-96e7932ecb02" />

### Step 2: 
* **Action:** Name the VM `AD-DC-01` inside resource group `AD-DC-01_group` in **North Central US**. Selected **Windows Server 2022 Datacenter: Azure Edition**, set size to **`Standard_B2ats_v2`**, and set availability options to **No infrastructure redundancy required**.
<img width="1470" height="834" alt="S2" src="https://github.com/user-attachments/assets/c31996fa-d3d0-4427-9d65-a48035b5611b" />

### Step 3: 
* **Action:** Enabled inbound **RDP (3389)** for administrative testing.

<img width="1470" height="791" alt="S30" src="https://github.com/user-attachments/assets/7f409714-c948-4259-a0d3-ec6bdeb1cbc8" />

### Step 4: 
* **Action:** Configure managed OS storage, and enabled **Auto-shutdown** under the Management tab

<img width="1470" height="792" alt="S4_2" src="https://github.com/user-attachments/assets/147ea9bd-1971-489e-b889-323cee48874a" />

### Step 4: Reviewing & Deploying the Virtual Machine
* **Action:** Validated all configuration parameters on the **Review + create** screen and initiated deployment.

---

## 🌐 Part 2: Remote Access via Azure Bastion (Zero-Client Setup)

### Step 1: 
* **Action:** Attempted to connect via **Connect > Bastion** from a macOS Chrome browser without local RDP client software.

<img width="1467" height="827" alt="S5_2" src="https://github.com/user-attachments/assets/791ae214-62ff-4b54-9c80-c4785111ac6c" />

### Step 2: 
* **Action:** Entered local administrator credentials (`labadmin`) into the Azure Bastion portal blade and launched an HTML5 remote session in Chrome.

 <img width="1470" height="831" alt="S6_2" src="https://github.com/user-attachments/assets/00afbd43-2472-415c-ac0c-457822fd0ba6" />

---

## 🏢 Part 3: Active Directory Domain Services (AD DS) Deployment

### Step 1: 

* **Action:** Open **Server Manager** dashboard, Inside Server Manager, select **Manage** (top right), select **Add Roles and Features**

<img width="1467" height="832" alt="S1_SERVER_DASH" src="https://github.com/user-attachments/assets/0475bbd2-44e6-4cb3-9b6a-bef6b1ca3106" />

### Step 2 
* **Action:** Choose **Role-based or feature-based installation** targeting `AD-DC-01`.

<img width="1470" height="835" alt="S7_2" src="https://github.com/user-attachments/assets/0e9a521a-ad2f-440a-b619-38fdc317468a" />

### Step 3

* **Action:** Click **Next** three times, navigating to **Server Roles**. Select **Active Directory Domain Services**.

<img width="1470" height="956" alt="S3_ACTIVE_DIRECT" src="https://github.com/user-attachments/assets/9fe2290e-34be-4307-80ef-884a3976f174" />

### Step 4 
* **Action:** When a pop-up window appears asking to add management tools, click **Add features** then click **Next**

<img width="1235" height="783" alt="S4" src="https://github.com/user-attachments/assets/d5881408-e1a3-4e4b-bff0-f255d0e89232" />

### Step 5 

* **Action:** Click **Next** twice, check **Restart the destination server automatically** if required, click **Yes**, and click **Install**.

<img width="1227" height="748" alt="S5" src="https://github.com/user-attachments/assets/56fd6014-924b-4051-9719-f7282a891bea" />

--- 

## Part 4 : Installing the AD DS Server Role


* **Action:** Selected **Active Directory Domain Services**. 

<img width="1470" height="956" alt="S3_ACTIVE_DIRECT" src="https://github.com/user-attachments/assets/17ba6e71-42d7-4942-9c2e-c8f788bbff7f" />

* **Step 2:** Accept the required **Remote Server Administration Tools (RSAT)** features, enabled automatic

<img width="1235" height="783" alt="S4" src="https://github.com/user-attachments/assets/e42618bd-012e-4882-bb30-321804b2febc" />

<img width="1227" height="748" alt="S5" src="https://github.com/user-attachments/assets/50689321-fe3c-48a8-b52c-bf5756b9bb6a" />

* **Step 4:** 

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
