# On-Premises Active Directory in Azure

![image](https://github.com/user-attachments/assets/c8be3692-3835-43a5-a828-612f39995eb5)

This tutorial explains how to set up an on-premises Active Directory using Azure Virtual Machines.

## Environments and Technologies

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

## Operating Systems

- Windows Server 2022
- Windows 10 (21H2)

## Overview

In this lab, we will create two Virtual Machines (VMs) within the same Virtual Network (VNET):
- One VM will serve as the Domain Controller (DC-1)
- The other will act as the Client machine (Client-1)

We will assign a static IP to the Domain Controller as it provides Active Directory services to the Client machine. The Client machine will be joined to the domain, and we will configure its DNS settings to use the Domain Controller as its DNS server.

<img width="468" alt="image" src="https://github.com/user-attachments/assets/4664bfd1-3ee4-419d-a495-3ea90f750241" />

## Deployment and Configuration Steps

### Step 1: Configure Network Settings

1. DC-1 must be assigned a static private IP address
2. Client-1 will connect to DC-1 to test connectivity by pinging it
3. Initially, the ping may not succeed - this is expected

<img width="461" alt="image" src="https://github.com/user-attachments/assets/e125487a-5a6a-41ff-a1cd-e593368c37cd" />

4. To fix connectivity issues, enable ICMPv4 on the firewall of DC-1
5. After enabling ICMPv4, Client-1 will be able to successfully ping DC-1

![image](https://github.com/user-attachments/assets/7d2c1e6d-102d-4722-877e-4f03b9f0bfb6)

### Step 2: Install Active Directory

1. Log back into DC-1 to install Active Directory Users & Computers
2. Promote the VM to a Domain Controller
3. Set up a new forest named "mydomain.com"
4. Restart the VM and log back into DC-1 using the user account "mydomain.com\\labuser"

If everything was done correctly, you should be able to open Active Directory Users & Computers as shown below:

![image](https://github.com/user-attachments/assets/3139695b-4077-4bed-815c-64c4d735354d)

### Step 3: Create Organizational Units (OUs) and Admin User

1. Create an OU called "_EMPLOYEES"
2. Create another OU named "_ADMINS"
3. To create OUs:
   - Right-click on the domain area
   - Select **New** > **Organizational Unit**
   - Fill in the required fields

4. Create an admin user:
   - Click on your new OU
   - Right-click and select **New** > **User**
   - Enter the details for the new user
   - Name the user **Jane Doe**
   - Set her username as **Jane_admin**
   - Add Jane to the Domain Admins security group

![image](https://github.com/user-attachments/assets/466c794d-786e-4d7f-b93d-8b0eeb65f70c)

<img width="384" alt="image" src="https://github.com/user-attachments/assets/47bb93b1-d1aa-4cd8-8b92-3c8edd420657" />

### Step 4: Join Client-1 to the Domain

1. From the Azure portal, change Client-1's DNS settings to the DC's Private IP address
2. Restart Client-1 from within the Azure portal
3. Verify that Client-1 is using DC-1 as its DNS server

![image](https://github.com/user-attachments/assets/4173369a-fa58-447f-b55b-35b3cbbd3ba4)

![image](https://github.com/user-attachments/assets/15dd424e-296f-4222-b2bc-ad1f70ad6ff2)

4. Join Client-1 to the domain:
   - Navigate to system settings
   - Go to "About"
   - Select "rename this PC (advanced)"
   - Select to change the domain
   - Enter "mydomain.com"
   - Enter your credentials (mydomain.com\\labuser)
   - Your computer will restart

<img width="468" alt="image" src="https://github.com/user-attachments/assets/c040d4d3-37f1-4e95-8d5d-7863d53f4fae" />

### Step 5: Set Up Remote Desktop for Non-Administrative Users

1. Log into Client-1 as an admin
2. Open system properties
3. Click on "Remote Desktop"
4. Allow "domain users" access to remote desktop

![image](https://github.com/user-attachments/assets/f56282a5-9b38-47ad-a6de-786c7e0e52de)

After completing these steps, you should be able to log into Client-1 as a normal user.

### Step 6: Verify Normal User Access

To verify that normal users can RDP into Client-1:

1. Use PowerShell to run a script that generates thousands of users in the domain

![image](https://github.com/user-attachments/assets/b9c01798-2647-4ce3-9761-e94d0898fa50)

2. After the users are created, select one for testing

![Generated users in Active Directory](media/image13.png)

3. Attempt to RDP into Client-1 using the credentials of the generated user

![image](https://github.com/user-attachments/assets/8bfe8b0d-a59d-4fc0-91d2-865b4262fbf9)

As shown above, the PowerShell script created a user with the username "bab.hubo" and we were able to successfully login to Client-1 with their credentials as a normal user.

## Conclusion

You have successfully:
- Set up an on-premises Active Directory in Azure
- Created and configured a Domain Controller
- Added a client machine to the domain
- Created organizational units and users
- Configured remote desktop access for domain users
- Verified functionality with test users

This lab demonstrates the fundamental components of Active Directory deployment in a cloud environment and provides a foundation for more advanced Active Directory configurations and management tasks.
