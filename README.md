<p align="center">
  
![image](https://github.com/user-attachments/assets/2242f440-7d95-4a4f-9bf3-b4b462d80442)

</p>

<h1>Setting up Active Directory</h1>
This project demonstrates the deployment and configuration of a Domain Controller (DC) and client environment in Microsoft Azure. The setup includes creating Azure resources, configuring static IP addresses, and validating connectivity between the DC and client. This serves as a foundational demonstration of Active Directory and network management skills.This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro

<h2>Installation Steps</h2>

<h2>1. Setup the Domain Controller (DC-1) in Azure</h2>

**1.Create a Resource Group**

    a. Navigate to the Azure Portal and create a Resource Group named Lab-RG.
  
**2.Create a Virtual Network and Subnet**

    a. Create a Virtual Network named Lab-VNet within Lab-RG.
  
    b. Add a Subnet named Lab-Subnet to the Virtual Network.
  
**3.Create the Domain Controller VM**

    a. Deploy a Virtual Machine with the following settings:
  
      - Name: DC-1
      - OS: Windows Server 2022
      - Region: Same as Lab-VNet
      - Credentials:
      - Username: labuser
      - Password: Cyberlab123!
      - Attach the VM to Lab-VNet and Lab-Subnet.
    
**4.Set Static Private IP Address**

    a. Go to DC-1's Networking settings in the Azure Portal.
    b. Set the NIC’s Private IP address to static.

![image](https://github.com/user-attachments/assets/9da6eae1-b623-49d1-8a92-5e7d66e90b84)

    
**5.Disable Windows Firewall for Testing**

    a. Log into DC-1 via Remote Desktop (RDP) using the credentials.
    b. Open PowerShell and disable the Windows Firewall:
      - Right click start Menu then press run
      - Type in wf.msc
      - Click Window Firewall Properties 
      - Turn Domain, Private,and Public Firewall State off and go to IPSec Settings to Apply

<h2>2. Setup the Client (Client-1) in Azure</h2>
  
**1. Create the Client VM**
  
    a. Deploy a Virtual Machine with the following settings:
      - Name: Client-1
      - OS: Windows 10
      - Region: Same as Lab-VNet
      - Credentials:
      - Username: labuser
      - Password: Cyberlab123
    b. Attach the VM to Lab-VNet and Lab-Subnet.
**2.Configure DNS Settings**

    a. Navigate to Client-1's Networking settings in the Azure Portal.
    b. Set the NIC’s DNS server to the Private IP address of DC-1.
  
**3.Restart the Client VM**

    a. Restart Client-1 from the Azure Portal to apply DNS changes.

![image](https://github.com/user-attachments/assets/362fbde7-0a7f-4c2b-b431-484fd9000947)

  
**4.Test Connectivity**

    a. Log into Client-1 via Remote Desktop.
    b. Open Command Prompt or PowerShell and run
      - Type command ping dc-1 private ip address (ping 10.0.0.4)
      - Type command ipconfig /all to confirm DNS Servers IP address is dc-1 Private IP address

![image](https://github.com/user-attachments/assets/231cd34d-f69a-467f-9989-de369cc29204)

<h2>Install and Setup Active Directory Domain Services</h2>


**1.Install Active Directory**

  1. Setup and Promote Domain Controller (DC-1)
     
    a. Log into DC-1 using Remote Desktop with:
      - Username: labuser
      - Password: Cyberlab123!
      
  3. Open Server Manager and install Active Directory Domain Services (AD DS):

    a. Click Add Roles and Features and select Active Directory Domain Services.
    
  3. Promote DC-1 to a Domain Controller:
  
    a. Choose Add a new forest and set the domain name (e.g., mydomain.com).
    b. Set a secure Directory Services Restore Mode (DSRM) password.
    c. Complete the wizard and restart the server.
    
  4. Log back into DC-1 as:
     
    - Username: mydomain.com\labuser
    - Password: Cyberlab123!
     
![image](https://github.com/user-attachments/assets/08c39821-60b0-43e6-96ed-63cba02c3f21)

**2. Configure Organizational Units (OUs) and Domain Admin Account**
   1. Open Active Directory Users and Computers (ADUC) from the Server Manager or Start Menu.
   
   2. Create Organizational Units:

    a. Right-click your domain (e.g., mydomain.com) and select New > Organizational Unit.
    b. Create an OU named _EMPLOYEES.
    c. Create another OU named _ADMINS.
      
  4. Create a Domain Admin User:
  
    a. Navigate to the _ADMINS OU.
    b. Right-click and select New > User.
      - Enter the following:
      - First Name: Jane
      - Last Name: Doe
      - User logon name: jane_admin
      - Password: Cyberlab123!
      - Uncheck User must change password at next logon.
    c. Complete the wizard to create the user.
    
  4. Add jane_admin to the Domain Admins Group:
     
    a. Open jane_admin's properties in ADUC.
    b. Navigate to the Member Of tab, click Add, and add the Domain Admins group.
    
  6. Log out of DC-1 and log back in as:
     
    - Username: mydomain.com\jane_admin
    - Password: Cyberlab123!

![image](https://github.com/user-attachments/assets/e125a877-26f5-430c-b7f2-b1593fe59e87)


**3. Join Client-1 to the Domain**

  1. Log into Client-1 using Remote Desktop with:
     
    - Username: labuser
    - Password: Cyberlab123!
    
  3. Set the system to join the domain:
  
    a. Open System Properties (Right-click This PC > Properties > Advanced System Settings > Computer Name).
    b. Click Change and enter the domain name ( mydomain.com).
    c. Enter jane_admin credentials when prompted.
    d. Restart the computer to apply changes.
    
  3. Log back into Client-1 using domain credentials:
     
    - Username: mydomain.com\jane_admin
    - Password: Cyberlab123!

**4. Organize Client-1 in ADUC**
  1. Log into DC-1 and open Active Directory Users and Computers.
  
  2. Verify that Client-1 appears under Computers.
  
  3. Create a new OU named _CLIENTS in the domain:
     
    a. Right-click the domain and select New > Organizational Unit.
   
  4. Drag Client-1 into the _CLIENTS OU.

![image](https://github.com/user-attachments/assets/e11b5080-582e-4f40-8c5b-f1d7c790d05b)

**5. Configure Remote Desktop for Domain Users on Client-1**

  1. Log into Client-1 with:
     
    - Username: mydomain.com\jane_admin
    - Password: Cyberlab123!
     
  3. Open System Properties:
     
    a.Right-click This PC > Properties > Advanced System Settings > Remote Desktop tab.
 
  5. Enable Remote Desktop:
     
    a.Select Allow remote connections to this computer.
    b.Click Select Users, then add Domain Users.
    c.Save and close.

  7. Test RDP access by logging into Client-1 with a non-administrative user account.

**6.Bulk Create Users in Active Directory**

  1. Log into DC-1 with:
     
    -Username: mydomain.com\jane_admin
    -Password: Cyberlab123!
     
  3. Open PowerShell ISE as an Administrator:
     
    a.Click Start, search for PowerShell ISE, right-click, and select Run as Administrator.

  5. Paste the following script into a new file in PowerShell ISE:
     
    a.This script creates 10000 users with the password Password1 and places them in the _EMPLOYEES OU.

  7. Run the script:
     
    a.Click Run or press F5 to execute.

  9. Verify the users in ADUC:
      
    a.Open Active Directory Users and Computers and navigate to the _EMPLOYEES OU.

![image](https://github.com/user-attachments/assets/359088fd-310c-4915-a34d-37770681e8a2)

![image](https://github.com/user-attachments/assets/e31e2cb8-fc98-4948-805b-b357cabcc8d1)



**7. Test User Login on Client-1**

  1. Attempt to log into Client-1 using one of the newly created accounts:

    -Username: mydomain.com\bam.hibu
    -Password: Password1
     
  2. Confirm that the login is successful, demonstrating proper integration of the new accounts.

![image](https://github.com/user-attachments/assets/15f7ae8f-bb0f-4d1b-9223-3e3048071741)


























