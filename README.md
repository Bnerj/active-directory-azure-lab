# Enterprise Active Directory & Identity Management Lab (Microsoft Azure)

## 📌 Project Overview
This project demonstrates the end-to-end deployment, configuration, and administration of an on-premises-style **Windows Server 2022 Active Directory Domain Controller** hosted in **Microsoft Azure**. The lab simulates core Tier 1 and Tier 2 IT Help Desk operations, including directory structuring, role-based access control (RBAC), account lifecycle management, Group Policy enforcement, and cloud infrastructure troubleshooting.

---

## Key Skills & Outcomes Summary 
Zero-trust remote administration via Azure Bastion, Active Directory forest promotion & DNS integration, Automated user lifecycle & GPO security hardening, FinOps cost control & resource deallocation

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

## 📋 Part 1: Azure Infrastructure & Virtual Machine Deployment

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

### Step 5: Reviewing & Deploying the Virtual Machine

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

### Step 2: 

* **Action:** Choose **Role-based or feature-based installation** targeting `AD-DC-01`.

<img width="1470" height="835" alt="S7_2" src="https://github.com/user-attachments/assets/0e9a521a-ad2f-440a-b619-38fdc317468a" />

### Step 3:

* **Action:** Click **Next** three times, navigating to **Server Roles**. Select **Active Directory Domain Services**.

<img width="1470" height="956" alt="S3_ACTIVE_DIRECT" src="https://github.com/user-attachments/assets/9fe2290e-34be-4307-80ef-884a3976f174" />

### Step 4:

* **Action:** When a pop-up window appears asking to add management tools, click **Add features** then click **Next**

<img width="1235" height="783" alt="S4" src="https://github.com/user-attachments/assets/d5881408-e1a3-4e4b-bff0-f255d0e89232" />

### Step 5:

* **Action:** Click **Next** twice, check **Restart the destination server automatically** if required, click **Yes**, and click **Install**.

<img width="1227" height="748" alt="S5" src="https://github.com/user-attachments/assets/56fd6014-924b-4051-9719-f7282a891bea" />

--- 

## Part 4: Installing the AD DS Server Role / Promote Server to a Domain Controller 

### Step 1:

* **Action:** Click the **yellow triangle warning icon** in Server Manager and select **Promote this server to a domain controller**.

 <img width="1056" height="784" alt="S6" src="https://github.com/user-attachments/assets/7e746238-758c-4f40-bfcb-473120071b9a" />

### Step 2: 

* **Action:** Select **Add a new forest** and specified Root domain name: **`lab.local`**. Click **Next**. 

<img width="1058" height="789" alt="S7" src="https://github.com/user-attachments/assets/1c3adc6d-fda9-4b02-b39f-eb9c871e01bc" />

### Step 3: 

* **Action:** Configure Forest/Domain functional levels, integrated **DNS Server** and **Global Catalog (GC)** roles, and set a **Directory Services Restore Mode (DSRM)** password. clicked **Next** through the remaining screens until reaching the **Prerequisites Check**.


<img width="1056" height="784" alt="S8" src="https://github.com/user-attachments/assets/3d0271ea-ce33-4272-ba91-60d03dc1fc87" />

### Step 4:

* **Action:** Passed all Prerequisites Checks and clicked **Install**, to promote `AD-DC-01` to a Domain Controller, triggering an automatic server reboot.

<img width="1058" height="785" alt="S9" src="https://github.com/user-attachments/assets/846baac1-df52-46c4-ae4c-4675ec92d196" />

---

## 👥 Part 5: Directory Structuring & RBAC (OUs, Users, & Groups)

### Step 1: 
* **Action:** Return to **Server Manager** Dashboard and navigate to **Tools** (top right), then **Active Directory Users and Computers (ADUC)**.

<img width="1470" height="832" alt="S10" src="https://github.com/user-attachments/assets/3ff79c99-9a0c-4b91-a539-c8d979eeef6a" />

### Step 2: 
* **Action:** Right-clicked **`lab.local`**, and created three top-level Organizational Units to separate administrative boundaries:
  * **`IT`**
  * **`HR`**
  * **`Sales`**

  <img width="1057" height="782" alt="S11" src="https://github.com/user-attachments/assets/50f8722e-96af-4898-9104-509cb891b54e" />


### Step 3: 
* **Action:** Right-clicked **HR**, **IT**, **SALES**, and created two simulated employee user accounts inside each department OU (6 users total).

<img width="1058" height="784" alt="S12" src="https://github.com/user-attachments/assets/016819dc-6b3e-45c9-9a57-d0bc3c31bba4" />

### Step 4:

* **Action:** Configuring user logon names (`@lab.local`) and initial domain passwords.

<img width="1056" height="786" alt="S13" src="https://github.com/user-attachments/assets/10f31876-bc1b-4593-a608-db1940a5ac8d" />
<img width="1055" height="782" alt="S14" src="https://github.com/user-attachments/assets/c703ecc8-faff-4236-bfe1-0ac21d8c6b7c" />

### Step 5: 
* **Action:** Created a dedicated **Global Security Group** inside each respective OU to enforce Role-Based Access Control (RBAC):
  * **`GRP-IT`** (inside `IT` OU)
  * **`GRP-HR`** (inside `HR` OU)
  * **`GRP-SALES`** (inside `Sales` OU)
    
<img width="1054" height="778" alt="S16" src="https://github.com/user-attachments/assets/19c33a5e-8f35-4230-b374-6a3f61f35bfe" />

### Step 6:

* **Action:** Opened each Security Group's **Properties > Members** tab, clicked **Add**, validated employee names via **Check Names**, and linked all users to their corresponding department groups (`GRP-IT`, `GRP-HR`, `GRP-SALES`).
  
<img width="908" height="785" alt="S17_2" src="https://github.com/user-attachments/assets/40d95ab6-d2e0-4a21-8f78-a0626070c7fc" />

<img width="911" height="782" alt="S17_3" src="https://github.com/user-attachments/assets/8b562759-d517-40f1-9b36-ec586a785422" />

<img width="911" height="783" alt="S17_4" src="https://github.com/user-attachments/assets/28d5ebc9-d4a3-4b87-bbc8-56714787b045" />

---

## 🎫 Part 6: Simulated Tier 1 Help Desk Operations

### Step 1: Executing Password Resets
* **Action:** Simulated a forgotten-password support ticket in ADUC by right-clicking a user account, selecting **Reset Password**, and enforcing **"User must change password at next logon"**.

<img width="813" height="782" alt="S18" src="https://github.com/user-attachments/assets/f9be8145-5c88-4db3-b8d3-e73fda89441b" />

<img width="811" height="782" alt="S19" src="https://github.com/user-attachments/assets/c718e8a5-264d-41ef-a682-9173e657bb48" />


### Step 2: Simulating Employee Offboarding (Disabling Accounts)
* **Action:** Simulated an HR termination ticket by right-clicking an employee account and selecting **Disable Account** to immediately revoke domain authentication while retaining audit logs.
  
<img width="912" height="783" alt="S20_2" src="https://github.com/user-attachments/assets/587f166c-87f2-4b4c-8967-2e298c75d94a" />

<img width="911" height="786" alt="S20_3" src="https://github.com/user-attachments/assets/5eb5adcf-d243-4b89-a238-91197cf941f5" />

---

## 🛡️ Part 7: Group Policy Object (GPO) Enforcement

### Step 1: Creating & Linking a Department GPO

* **Action:** Opened **Server Manager > Tools (top right) > Group Policy Management**.

<img width="812" height="781" alt="S20" src="https://github.com/user-attachments/assets/63abae85-ffb7-4194-a29f-957ae824443f" />

### Step 2: 

* **Action:** Expanded `Forest: lab.local > Domains > lab.local`, right-clicked the **`Sales`** OU, and selected **Create a GPO in this domain, and Link it here**.

<img width="808" height="783" alt="S22" src="https://github.com/user-attachments/assets/6adf4b9a-ba27-461b-8284-4aafdd3d35f6" />

### Step 3: Name the GPO **`Block Control Panel`**.

<img width="915" height="745" alt="S23_2" src="https://github.com/user-attachments/assets/ee67a41d-61b5-4d50-88b6-e4ebf46f5f46" />

### Step 4: 

* **Action:** Right-clicked **`Block Control Panel`**, selected **Edit** to open the Group Policy Management Editor.

<img width="813" height="783" alt="S23" src="https://github.com/user-attachments/assets/ddfe90cf-9472-42d7-bc6a-310dee3513fd" />

### Step 5: 

* **Action:** Navigated to `User Configuration > Policies > Administrative Templates > Control Panel`

<img width="814" height="787" alt="S26" src="https://github.com/user-attachments/assets/ba060ffc-a6ff-477f-a311-fafdae63ba75" />

<img width="810" height="788" alt="S27" src="https://github.com/user-attachments/assets/70c365a1-3ec7-4de7-915b-654843b229df" />

### Step 6: 

* **Action:** Opened **Prohibit access to Control Panel and PC settings**

<img width="812" height="785" alt="S28" src="https://github.com/user-attachments/assets/7458e2f0-9a5a-4e76-b16e-acb76e2afc2c" />

### Step 7: 

* **Action:** Set it to **Enabled**, and clicked **Apply > OK**.

<img width="1073" height="784" alt="S29" src="https://github.com/user-attachments/assets/9bb6a433-1325-4435-b468-65983c2b2826" />

### Step 8: 
* **Action:** Confirmed in the Group Policy Management Console that **`Block Control Panel`** was actively linked and enforced exclusively on the **`Sales`** OU without affecting `IT` or `HR` users.

<img width="1470" height="831" alt="S30" src="https://github.com/user-attachments/assets/e39b667e-7b44-4a2c-bad9-59b10d742ada" />

---

## 💰 Part 8: Cloud Cost Optimization & Resource Cleanup

### Step 1: 

* **Action:** Navigate to the Azure Portal Dashboard, Select `AD-DC-01_group` in the `resource` section 

 <img width="1468" height="832" alt="S31" src="https://github.com/user-attachments/assets/710280fe-1698-406e-9b67-6ffe8013175a" />

 ### Step 2:

 * **Action:** Select `Connect` drop-down-menu, click `Bastion` and click on `vnet-northcentralus-1-bastion`.

<img width="1463" height="832" alt="S32" src="https://github.com/user-attachments/assets/e44f4ffa-f86c-4be0-ab94-86b58fa6bbb0" />

### Step 3: 

 * **Action:** Deleted the **Azure Bastion** resource from `AD-DC-01_group` and stopped `AD-DC-01` (**Stopped (deallocated)**) to protect Azure for Students credits.

<img width="1470" height="794" alt="S33" src="https://github.com/user-attachments/assets/654cdbaf-610b-4c4d-b024-907eda61c618" />

