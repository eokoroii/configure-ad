# Active Directory Lab: Domain Controller Setup and Domain Join

## Project Summary

In this lab, I set up a complete Active Directory environment using two virtual machines in Azure. The first VM runs Windows Server 2022 and acts as a Domain Controller (**DC-1**), serving as both the AD DS host and a DNS server. The second VM runs Windows 10 Pro (**Client-1**) and joins the domain managed by **DC-1**. This lab simulates a real-world scenario where client machines rely on a Domain Controller for authentication, DNS resolution, and overall network management.

### Key Components

- **Resource Group:** `Active-Directory-Lab`
- **Virtual Network:** `Active-Directory-VNet`
- **Domain Controller VM:** `DC-1` (Windows Server 2022 Datacenter)
- **Client VM:** `Client-1` (Windows 10 Pro)
- **Network Configuration:** Static IP configuration and custom DNS settings
- **AD DS Installation:** Installing and promoting AD DS on DC-1 to create a new forest (`mydomain.com`)
- **Domain Join:** Creating a Domain Admin user and joining Client-1 to the domain

**Goal:**  
Establish a foundational Active Directory environment by configuring a Domain Controller and joining a client machine to the domain.

---

## Steps & Screenshots

1. **Create Resource Group & Virtual Network**  
   In the Azure portal, create a Resource Group named `Active-Directory-Lab` and a Virtual Network called `Active-Directory-VNet` in the East 2 region.

   ![Resource Group Creation](https://i.imgur.com/ZWukHYp.png)  
   ![Virtual Network Creation](https://i.imgur.com/Nn3DdbK.png)  
   *Figure 1.1 – Resource Group and Virtual Network setup.*

2. **Deploy the Domain Controller VM (DC-1)**  
   Create the VM **DC-1** using the Windows Server 2022 Datacenter image with at least 2 vCPUs. Set the administrator username to `labuser` and the password to `Cyberlab123!`. Ensure that **DC-1** is connected to the `Active-Directory-VNet`.

   ![Domain Controller VM Creation](https://i.imgur.com/wdM2XcF.png)  
   *Figure 1.2 – Creating the Domain Controller VM (DC-1).*

3. **Deploy the Client VM (Client-1)**  
   Create the VM **Client-1** using the Windows 10 Pro image. Use the same credentials as for **DC-1** and connect **Client-1** to the `Active-Directory-VNet`.

   ![Client VM Creation](https://i.imgur.com/CPc9Cxs.png)  
   *Figure 1.3 – Creating the Client VM (Client-1).*

4. **Set Static IP for the Domain Controller**  
   Configure **DC-1**'s network interface to use a static private IP address. This step is critical because the client will use this IP as its DNS server.

   ![Setting Static IP](https://i.imgur.com/0GgvBRj.png)  
   *Figure 1.4 – Configuring the NIC for **DC-1** with a static IP.*

5. **Log into the Domain Controller and Disable Windows Firewall**  
   Remote into **DC-1** using its Public IP via Remote Desktop. Open Server Manager, launch the Run dialog (`Win + R`), type `wf.msc`, and disable the firewall on the Domain, Private, and Public profiles to avoid connectivity issues.
   
   ![Remote Desktop into DC-1](https://i.imgur.com/FIBMRKo.png)  
   *Figure 1.5 – Remote Desktop session on **DC-1**.*

   ![Disabling Windows Firewall](https://i.imgur.com/80dniD2.png)  
   *Figure 1.6 – Disabling Windows Firewall on **DC-1**.*

6. **Configure Client-1 to Use DC-1 as its DNS Server**  
   - In the Azure portal, navigate to **DC-1 > Networking** and copy the Domain Controller’s Private IP (e.g., `10.0.0.4`).
   - Go to **Client-1 > Networking** and select its network interface configuration.
   - Change the DNS Servers setting from *Inherit from virtual network* to **Custom**.
   - Enter **DC-1**'s Private IP as the DNS server and save the changes.
   - Restart **Client-1** to apply the new DNS settings.

   ![Configuring DNS on Client-1](https://i.imgur.com/sfLxUbV.png)  
   *Figure 1.7 – Setting Client-1 DNS to point to DC-1’s private IP.*

7. **Verify Connectivity Between Client-1 and DC-1**  
   - Remote into **Client-1** via its Public IP.
   - Open Windows PowerShell as Administrator.
   - Run `ping 10.0.0.4` (or the actual static IP of **DC-1**) to test connectivity.
   - Run `ipconfig /all` to confirm that the DNS server for **Client-1** is set to **DC-1**'s Private IP.

   ![Ping Test from Client-1](https://i.imgur.com/9Qu1Gz7.png)  
   ![IP Configuration on Client-1](https://i.imgur.com/x3JDgGO.png)  
   *Figure 1.8 – Verifying connectivity and DNS settings on Client-1.*

8. **Install Active Directory Domain Services (AD DS) on DC-1**  
   On **DC-1**, open Server Manager and click **Add Roles and Features**. Select **Active Directory Domain Services** from the list and complete the wizard. Allow the system to restart after installation.

   ![AD DS Role Selection](https://i.imgur.com/MVhTNoQ.png)  
   ![AD DS Installation Progress](https://i.imgur.com/AtcdKIX.png)  
   *Figure 1.9 – Installing AD DS on **DC-1**.*

9. **Promote DC-1 as the Domain Controller**  
   After the restart, in Server Manager click the flag icon and select **Promote this server to a domain controller**. Choose **Add a new forest** and enter `mydomain.com` as the root domain name. Provide the Directory Services Restore Mode (DSRM) password (`Password1`) and complete the wizard. The system will restart; upon logging back in, your login should display `mydomain.com\labuser`.

   ![Promoting DC-1](https://i.imgur.com/5RuUmXS.png)  
   ![Promoting DC-1](https://i.imgur.com/GvA5w3w.png)  
   *Figure 1.10 – Promoting **DC-1** and configuring the new forest.*

10. **Create a Domain Admin User in Active Directory**  
    On **DC-1**, open **Active Directory Users and Computers (ADUC)**. Create two Organizational Units (OUs) under `mydomain.com` (e.g., `_EMPLOYEES` and `_ADMINS`). Create a new user (e.g., Jane Doe with logon name `jane_admin@mydomain.com` and password `Cyberlab123!`) and add the user to the **Domain Admins** group.

    ![Creating Domain Admin User](https://i.imgur.com/2ZZG8nG.png)  
    ![Creating Domain Admin User](https://i.imgur.com/4dVOsI9.png)  
    *Figure 1.11 – Creating Jane Doe as a Domain Admin.*

11. **Join Client-1 to the Domain**  
    On **Client-1**, log in using the local `labuser` credentials. Navigate to **Settings > System > About > Advanced system settings**, and click **Change** under the Computer Name tab. Select **Domain** and enter `mydomain.com`, then provide the Domain Admin credentials (`mydomain.com\jane_admin`) when prompted. Restart **Client-1** to complete the domain join.

    ![Joining Client-1 to Domain](https://i.imgur.com/0B3AMsA.png)  
    *Figure 1.12 – Configuring **Client-1** to join the domain.*

12. **Verify Domain Join in Active Directory**  
    On **DC-1**, open **Active Directory Users and Computers**. Expand `mydomain.com` and check the **Computers** container to confirm that **Client-1** appears. Optionally, create an OU named `_CLIENTS` and move **Client-1** into it.

    ![Verifying Client-1 in ADUC](https://i.imgur.com/nAmKt7w.png)  
    *Figure 1.13 – **Client-1** is visible in Active Directory Users and Computers.*

---

## Conclusion

In this lab, I established a complete Active Directory environment by:

- Creating a Resource Group and Virtual Network in Azure.
- Deploying a Domain Controller (**DC-1**) and a client machine (**Client-1**).
- Configuring static IP and custom DNS settings to ensure proper communication.
- Installing and promoting AD DS on **DC-1** to form a new forest (`mydomain.com`).
- Creating a Domain Admin user and joining **Client-1** to the domain.
- Verifying the setup in Active Directory Users and Computers.

This comprehensive lab simulates a real-world Active Directory environment and serves as a foundational setup for advanced AD management tasks.

---
