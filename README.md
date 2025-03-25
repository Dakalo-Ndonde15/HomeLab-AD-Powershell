# Home Lab: Active Directory AD & PowerShell 

## Description
This tutorial provides a step-by-step guide on setting up a basic home lab with an Active Directory (AD) using Hyper-V Manager and creating 1000 users in AD with PowerShell.

## Languages and Utilities Used
- PowerShell
- Command Prompt
- Hyper-V Manager

## Environments Used
- Windows 11 Pro/Enterprise (22H2)

## Files You Need to Download
- [Win Server 2016 Download](https://info.microsoft.com/ww-landing-windows-server-2016.html)
- [Windows 10 ISO Download](https://www.microsoft.com/en-us/software-download/windows10)
- [AD_PS-master Download file](https://github.com/Dakalo-Ndonde15/HomeLab-AD-Powershell/blob/main/AD_PS-master.zip)

## Steps Included
1. Virtual Machines Setup
    - Create 2 Virtual Switches ("NAT" and "SUBNETA")
    - Create 2 Virtual Machines ("DC1" and "Client1")
2. Install & Configure AD, RAS/NAT, DHCP
3. Create 1000 Users in AD using PowerShell
4. Join "Client1" to the Domain
5. Log in to "Client1" with a Created User

## Walk-through Steps

### Step 1: Virtual Machines Setup
#### Create 2 Virtual Switches ("NAT" and "SUBNETA")
Open Hyper-V Manager. Right-click on the local machine and select **Virtual Switch Manager**. Alternatively, select **Virtual Switch Manager** from the Action pane.

Next, click on **New Virtual Network Switch** and create two virtual switches:
- **NAT** (External)
- **SUBNETA** (Private)
  
![Virtual Switch Manager Open](https://imgur.com/6bGexoi.png)

#### Create 2 Virtual Machines ("DC1" and "Client1")
1. Configure the Virtual Machine settings to match the summary shown below.
2. Attach **Windows Server 2016 ISO** to the **DC1** virtual machine.
   
![Virtual Machine Settings](https://imgur.com/ZdZtpFk.png)

#### Configure DC1 Network Adapters
1. Right-click on **DC1**, select **Settings**.
2. Add a second network adapter:
    - **NAT** for external access
    - **SUBNETA** for internal network
      
![Virtual Machine Settings](https://imgur.com/BpG2ZDi.png)

#### Install Windows Server 2016 on DC1
1. Start **DC1** and install **Windows Server 2016**.

#### Rename Computer in DC1
1. Open **System Properties** in **DC1**.
2. Click **Change** next to the computer name.
3. Rename the computer to **DC1**.
4. Restart the machine to apply the changes.

#### Rename Network Adapters in DC1
1. Open **Network Settings** in **DC1**.
2. Rename **NAT** and **SUBNETA** for easy identification.
   
![Virtual Machine Settings](https://imgur.com/6RUafEc.png)

#### Set Static IP Address for SUBNETA (Private Network) in DC1

1. Open **Network Settings** on **DC1**.
2. Locate the **SUBNETA (Private Network)** adapter, right-click on it, and select **Properties**.
3. In the list of items, select **Internet Protocol Version 4 (TCP/IPv4)**, then click **Properties**.
4. Select **Use the following IP address** and enter the following values:
   - **IP address:** `172.16.0.1`
   - **Subnet mask:** `255.255.255.0`
   - **Preferred DNS server:** `127.0.0.1`
5. Click **OK** to save the settings and close the dialog.
   
![Virtual Machine Settings](https://imgur.com/tAnqObi.png)

#### Create Client1 Virtual Machine
1. Repeat the steps to create a new Virtual Machine named **Client1**.
2. Connect **Client1** to **SUBNETA**.
3. Attach **Windows 10 ISO** and install **Windows 10 Pro**.

![Virtual Machine Settings](https://imgur.com/rYM9hiw.png)

#### Install Windows 10 on Client1
1. Complete the Windows 10 installation on **Client1**.

#### Rename Computer in Client1
1. Open **System Properties** in **Client1**.
2. Click **Change** next to the computer name.
3. Rename the computer to **Client1**.
4. Restart the machine to apply the changes.

## Video Walkthrough
[Watch the Video](https://www.youtube.com/watch?v=dQw4w9WgXcQ)

---
### Step 2: Install & Configure AD, RAS/NAT, DHCP

#### Install Active Directory Domain Services (AD DS)
1. Open **Server Manager** on **DC1**.
2. Click **Add roles and features**.
3. Select **Role-based or feature-based installation** and click **Next**.
4. Select **DC1** and click **Next**.
5. Select **Active Directory Domain Services** and click **Add Features** when prompted.
6. Click **Next** through the wizard, then click **Install**.
7. After installation, click **Promote this server to a domain controller** in the notification area.

#### Configure AD Domain Controller
1. In the **Deployment Configuration** window, select **Add a new forest**.
2. Enter a **Root domain name** (e.g., `homelab.local`), and click **Next**.
3. Set the **Forest functional level** and **Domain functional level** to **Windows Server 2016**.
4. Enter and confirm a **Directory Services Restore Mode (DSRM) password**.
5. Click **Next** through the rest of the wizard and click **Install**.
6. The server will restart automatically after promotion.

#### Install and Configure RAS/NAT
1. Open **Server Manager**.
2. Click **Add roles and features**.
3. Select **Remote Access** and click **Next**.
4. Under **Role Services**, select **Routing** and click **Add Features** when prompted.
5. Complete the installation and open **Routing and Remote Access** from **Tools** in **Server Manager**.
6. Right-click on **DC1** (in the **Routing and Remote Access** console) and select **Configure and Enable Routing and Remote Access**.
7. Select **Custom configuration**, check **NAT**, and click **Next**.
8. Click **Finish**, then **Start service**.
9. Expand **IPv4**, right-click **NAT**, and select **New Interface**.
10. Select **NAT (External Network)** and click **OK**.
11. Select **Public interface connected to the Internet** and check **Enable NAT on this interface**.
12. Click **OK**.
13. Repeat **New Interface** for **SUBNETA (Private Network)**, select **Private interface connected to the private network**, and click **OK**.

#### Install and Configure DHCP
1. Open **Server Manager**.
2. Click **Add roles and features**.
3. Select **DHCP Server** and click **Next** through the wizard. Click **Install**.
4. After installation, open **DHCP** from **Tools** in **Server Manager**.
5. Expand **DC1**, right-click **IPv4**, and select **New Scope**.
6. Click **Next** and enter a **Scope Name** (e.g., `SUBNETA`), then click **Next**.
7. Enter the **IP address range** (e.g., **172.16.0.100** to **172.16.0.200**) and **Subnet mask** (**255.255.255.0**), then click **Next**.
8. Add any **Exclusions** if necessary or click **Next**.
9. Set the **Lease Duration** as desired, and click **Next**.
10. Select **Yes, I want to configure these

## Next Steps
- Step 3 – Create 1000 Users in AD with PowerShell
- Step 4 – Join **Client1** to the Domain
- Step 5 – Login to **Client1** as a Created User


