# Active-Directory-


 
On-Premises Active Directory in Azure
This tutorial explains how to set up an on-premises Active Directory using Azure Virtual Machines.
Environments and Technologies
•	Microsoft Azure (Virtual Machines/Compute)
•	Remote Desktop
•	Active Directory Domain Services
•	PowerShell
Operating Systems
•	Windows Server 2022
•	Windows 10 (21H2)
Deployment and Configuration Steps
In this lab, we will create two Virtual Machines (VMs) within the same Virtual Network (VNET). One VM will serve as the Domain Controller, while the other will act as the Client machine. We will assign a static IP to the Domain Controller, as it provides Active Directory services to the Client machine. The Client machine will be joined to the domain, and we will configure its DNS settings to use the Domain Controller as its DNS server.
.


 
DC-1 must be assigned a static private IP address. Client-1 will connect to DC-1 to test connectivity by pinging it. Initially, the ping may not succeed. To fix this, we need to enable ICMPv4 on the firewall of DC-1. After doing this, Client-1 will be able to successfully ping DC-1.

 
 
Next, we will log back into DC-1 to install Active Directory Users & Computers. We will promote the VM to a Domain Controller and set up a new forest named "mydomain.com." After completing this, we will restart the VM and log back into DC-1 using the user account "mydomain.com\labuser." If everything was done correctly, you should be able to open Active Directory Users & Computers as shown below. 

Great! Now we can start creating Organizational Units (OUs). First, let's create an OU called **_EMPLOYEES** and another one named **_ADMINS**. To do this, right-click on the domain area, select **New** > **Organizational Unit**, and fill in the required fields.

Next, click on your new OU, right-click again, select **New** > **User**, and enter the details for the new user. Name the user **Jane Doe**, and since she will be an Admin, set her username as **Jane_admin**. Finally, add Jane to the Domain Admins security group. 


 
From now on you can use Jane_admin as the administrator account. Now we will join Client-1 to the domain (mydomain.com) from the azure portal we will change client-1's DNS settings to the DC's Private IP address. After you do that restart Client-1 from within the Azure portal. Our picture below shows verification that client-1 is on the DC-1 DNS.
 


 
We have to join Client-1 to the domain in order to do so navigate to your system settings and go to about. Off to the right select rename this pc (advanced). From there select to change the domain. Enter "mydomain.com" after that enter your credentials from mydomain.com\labuser. Your computer will restart and then client-1 will be a part of mydomain.com


 
Wonderufl Client-1 is now a part of the domain. Now we will set up remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open system properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. After completing those steps you should be able to log into Client-1 as a normal user.


 
Lastly to verify that noraml users can RDP into Client-1 we will use a script to generate thousands of users into the domain. We will input the script in powershell, after the users are created we will select one and RDP into Client-1.


 
 
 
As you can see the Powershell script created a user with the username "bab.hubo" We were able to login to Client-1 with his credentials as a normal user.
