<p align="center">
  
![image](https://github.com/user-attachments/assets/2242f440-7d95-4a4f-9bf3-b4b462d80442)

</p>

<h1>Setting up Active Directory</h1>
This project demonstrates the deployment and configuration of a Domain Controller (DC) and client environment in Microsoft Azure. The setup includes creating Azure resources, configuring static IP addresses, and validating connectivity between the DC and client. This serves as a foundational demonstration of Active Directory and network management skills.This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop

<h2>Operating Systems Used </h2>

- Windows Sserver 2022
- Windows 10 Pro

<h2>Installation Steps</h2>

<h2>1. Setup the Domain Controller (DC-1) in Azure<h2>

**1.Create a Resource Group**

  a.Navigate to the Azure Portal and create a Resource Group named Lab-RG.
  
**2.Create a Virtual Network and Subnet**

  a.Create a Virtual Network named Lab-VNet within Lab-RG.
  
  b.Add a Subnet named Lab-Subnet to the Virtual Network.
  
**3.Create the Domain Controller VM**

  a.Deploy a Virtual Machine with the following settings:
  
    -Name: DC-1
    -OS: Windows Server 2022
    -Region: Same as Lab-VNet
    -Credentials:
    -Username: labuser
    -Password: Cyberlab123!
    -Attach the VM to Lab-VNet and Lab-Subnet.
    
**4.Set Static Private IP Address**

    a.Go to DC-1's Networking settings in the Azure Portal.
    b.Set the NIC’s Private IP address to static.

![image](https://github.com/user-attachments/assets/9da6eae1-b623-49d1-8a92-5e7d66e90b84)

    
**5.Disable Windows Firewall for Testing**

    a.Log into DC-1 via Remote Desktop (RDP) using the credentials.
    b.Open PowerShell and disable the Windows Firewall:
      -Right click start Menu then press run
      -Type in wf.msc
      -Click Window Firewall Properties 
      -Turn Domain, Private,and Public Firewall State off and go to IPSec Settings to Apply

<h2>2. Setup the Client (Client-1) in Azure<h2>
  
  **1.Create the Client VM**
  
    a.Deploy a Virtual Machine with the following settings:
      -Name: Client-1
      -OS: Windows 10
      -Region: Same as Lab-VNet
      -Credentials:
      -Username: labuser
      -Password: Cyberlab123
    b.Attach the VM to Lab-VNet and Lab-Subnet.
**2.Configure DNS Settings**

  a.Navigate to Client-1's Networking settings in the Azure Portal.
  b.Set the NIC’s DNS server to the Private IP address of DC-1.
  
**3.Restart the Client VM**

  a.Restart Client-1 from the Azure Portal to apply DNS changes.

![image](https://github.com/user-attachments/assets/362fbde7-0a7f-4c2b-b431-484fd9000947)

  
**4.Test Connectivity**

  a.Log into Client-1 via Remote Desktop.
  b.Open Command Prompt or PowerShell and run
    -Type command ping dc-1 private ip address (ping 10.0.0.4)
    -Type command ipconfig /all to confirm DNS Servers IP address is dc-1 Private IP address

![image](https://github.com/user-attachments/assets/231cd34d-f69a-467f-9989-de369cc29204)

    

