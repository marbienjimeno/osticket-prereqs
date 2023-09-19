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

<p>
   Now we'll connect to the newly created Windows 10 VM using Remote Desktop. Copy VM-osTicket's Public IP address into Remote Desktop Connection and click Connect. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/29b17d1e-c908-44c7-90d7-c0f61861a67e)

<p>
   Log in using the credentials entered when setting up the VM. For User name, enter "labuser". For Password, enter "Password1234!". click OK.  
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/aed74bf3-3ccd-4ca2-a596-4a1cd9d36788)

<p>
   A warning will appear regarding the VM's certificate verification. Click Yes to connect anyway. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/5704b692-f6c1-48d2-b4ae-2c782aa3d86c)

Now that we have connected into the Windows 10 VM, we'll begin the osTicket installation process. Inside the Windows VM, open the Microsoft Edge browser. 

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/9a32acf4-de59-483b-a1a5-89bd936f612d)

Copy, paste, and go to the following URL inside the browser: [https://drive.google.com/drive/u/0/folders/1APMfNyfNzcxZC6EzdaNfdZsUwxWYChf6](https://drive.google.com/drive/u/0/folders/1APMfNyfNzcxZC6EzdaNfdZsUwxWYChf6). This Google Drive folder contains the installation files required for osTicket. 

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/44c8ecaa-8345-4fa5-9b1f-49557d114655)

**2. Install and enable IIS in Windows with CGI, all Common HTTP features, and IIS Management Console.**

<p>
   Before downloading any files, we will install and enable IIS in Windows. In the search bar, enter "control panel". Open Control Panel. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/e07f39f7-5e41-46fe-bc57-6d94a76227db)

<p>
   Inside Control Panel, go to Programs. Then click "Turn Windows features on or off". 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/5a634009-a248-4d97-8407-dc08aedaa70e)

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/c95fb88d-c740-47f8-a24e-db0ec8a0215b)

<p>
   Check and expand "Internet Information Services". 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/e17be29e-a427-46f4-bb5d-d66cad2f3ce2)

<p>
   Expand "World Wide Web Services". Expand "Application Development Features". Check the box next to "CGI".
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/3e94636e-3b85-467f-9101-cab319143519)

<p>
   Collapse "Application Development Features" and expand "Common HTTP Features". Check all the options available. Click OK. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/0854e872-4f8f-4d0b-a1d0-2cf5f7843ec9)

<p>
   After IIS has finished installing, we'll confirm the installation by going to the browser and going to the IP address "127.0.0.1". The following page should appear to confirm the IIS installation. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/670cec37-6113-4f77-8474-2ef0b4725d5f)

**3. From the installation files, download and install PHP manager for IIS and Rewrite Module.**

<p>
   Now we'll download PHP manager for IIS. From the Installation Files folder, download "PHPManagerForIIS_V1.5.0.msi".
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/d78c11a7-f12a-4497-a6e7-dc531eb39efa)

<p>
   When the file has finished downloading, open the file and follow the wizard to proceed with its installation. Click Close after PHP Manager for IIS has finished installing. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/045b3042-ef5c-41ae-b5e0-a2eac6df5157)

<p>
   Next, from the Installation Files folder, we'll download "rewrite_amd64_en-US.msi".
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/96701209-c18d-4d75-9f01-91b6d33b3450)

<p>
   Once the file has downloaded, open the file and proceed with the installation wizard. When the Rewrite module has installed, click Finish. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/bc7cb2c1-1331-429c-877c-24f64024b652)

**4. Create the directory C:\PHP. Download PHP 7.3.8 and unzip its contents into C:\PHP.**









