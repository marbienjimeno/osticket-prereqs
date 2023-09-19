<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Prerequisites and Installation</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>High-Level Steps</h2>

1. Create an Azure Virtual Machine with Windows 10 and 4 virtual CPUs.
2. Install and enable IIS in Windows with CGI, all Common HTTP features, and IIS Management Console.
3. From the installation files, download and install PHP manager for IIS and Rewrite Module.
4. Create the directory C:\PHP. Download PHP 7.3.8 and unzip its contents into C:\PHP.
5. From the installation files, download and install VC_redist.x86.exe and MySQL 5.5.62.
6. Open IIS as an Admin, register PHP from within IIS, and reload IIS.
7. Open osTicket on the browser and enable additional PHP extensions.
8. Rename "ost-sampleconfig.php" to "ost-config.php". Assign the necessary permissions in ost-config.php.
9. Continue osTicket setup in the browser. Download and install HeidiSQL from the installation files and create an "osTicket" database. Finish the osTicket setup and install.
10. Perform final cleanup.
   
<h2>Installation Steps</h2>

**1. Create an Azure Virtual Machine with Windows 10 and 4 virtual CPUs.**
<p>
We'll start by creating a new Resource group. We'll give it the name "RG-osTicket". For Region, select the region that best applies. In this lab, we'll select "(US) West US 3". Click Review and create. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/27a982cb-a64e-4cb3-a7fc-54e64d3f23e1)

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/cb803f19-eb6f-45bf-9b0f-b66231d45e66)

<p>
   When the validation process has passed, click Create. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/74540534-613e-4c5f-b500-5b1ac3f2bd68)

<p>
   After the RG-osticket resource group has been created, create a new Azure virtual machine. For Resource group, select the previously created "RG-osTicket". For its name, we'll enter "VM-osTicket". For Region, select the same region used when creating the resource group. We'll select "(US) West US 3". For Availability options, select "No infrastructure redundancy required". For Image, select the Windows 10 Pro option. For Size, select "Standard_D4s_v3 - 4 vcpus, 16 GiB memory ($140.16/month)". For Username, we'll use "labuser". For Password, we'll use "Password1234!". Make sure to record the username and password for future reference. Under Licensing, check the box stating "I confirm I have eligible Windows 10/11 license with multi-tenant hosting rights". Click Next: Disks >.
</p>  

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/7cfa368a-c3e3-4bff-96e5-103c63af9c79)

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/2850135f-c5f0-4014-a1cb-47d7b9269adf)

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/edcf5284-3b61-4788-9105-08271851c674)

<p>
We'll use the default settings for Disks. Click Next: Networking >
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/a1a6f2d7-fa0b-4729-b86e-65121f4e0d1b)

<p>
   For Virtual network, we see that a new "VM-osTicket-vnet" has been created. For Subnet, our virtual network has been assigned 10.0.0.0/24 by default. For Public IP, a new "VM-osTicket-ip" has been created. Click Review + create.  
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/2923e97d-3fee-4752-aed6-eb4e0d12291e)

<p>
   After the validation process has passed, click Create. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/58e679da-f36b-4c63-9775-0f88aaf96dcc)

<p>
   After the deployment process has completed, click Go to resource. We have successfully created a new Windows 10 VM. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/2974be59-469d-47cb-a982-3e385b6e6844)


