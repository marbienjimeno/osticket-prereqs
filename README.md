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

<p>
   Open File Explorer and navigate to the C: folder. Inside C:, create a new folder and name it "PHP".
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/f1fc4c32-d7d9-43a5-9116-fac0e2bd174f)

<p>
   Close File Explorer and go to the Installation Files folder. Dowload the file "php-7.3.8-nts-Win32-VC15-x86.zip". The download may take a few moments to begin. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/c92309bf-a479-4252-b66b-1182e2707d89)

<p>
   When the file has finished downloading, go to File Explorer and navigate to the file. Right-click the file and click Extract All....
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/2da0c4fa-1ea7-45e1-97e1-8aa74cd17e87)

<p>
   Extract the contects of the downloaded file into C:\PHP.
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/a2935616-cfc0-4ef1-95cf-bee41ff8e9b6)

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/3ccbe05f-caf3-4cf9-928e-50e331d5cd5b)

**5. From the installation files, download and install VC_redist.x86.exe and MySQL 5.5.62.**
<p>
   Close File Explorer. From the Installation Files folder, download "VC_redist.x86.exe".
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/f6aedb77-2129-410d-ba37-1f05f28e8d0b)

<p>
   After the file had downloaded, click the file. Follow the installation wizard and click Close when the installation has finished.
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/95ef258a-1f2a-4750-bb54-456a9c45bdce)

<p>
   From the Installation Files folder, download "mysql-5.5.62-win32.msi".
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/de767eaf-862b-43ab-999e-0cb368db2cbc)

<p>
   When the file has downloaded, Open the file. Inside the installation wizard, for the Setup Type, choose Typical. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/06331eae-efd8-4141-9622-0a7c7220ac93)

<p>
   Click to check "Launch the MysQL Instance Configuration Wizard" and click Finish. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/41f9c437-daf4-4537-ba79-902f95ad5799)

<p>
   Inside the MysQL Instance Configuration Wizard, select "Standard Configuration". 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/069aabe6-58d6-4a10-9361-bc006d56a145)

<p>
   For the root password of the root account, we'll enter "Password1". Make sure to remember this password for future reference. 
<p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/e7cfa553-0d0f-45ff-9b7a-3e2e186f031a)

<p>
   Click on Execute to begin the MySQL instance configuration. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/34e0cc8a-15c4-4220-86cf-5e1ff816b048)

**6. Open IIS as an Admin, register PHP from within IIS, and reload IIS.**
<p>
   After MySQL has been installed, go to the search bar and enter "IIS". Right-click the IIS Manager app and click Run as Administrator. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/99db4834-626a-4aa4-8b0b-36d9773a564e)

<p>
   Double-click PHP Manager. We see that PHP is currently not enabled. Click on Register new PHP version. Navigate to C:\PHP\php-7.3.8-nts-Win32-VC15-x86 (right-click and download)\ and select php-cgi. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/ba86182a-a288-4bf5-9307-a6ad4cdedb94)

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/f924a58c-8d6c-40ab-9e00-7d876f746639)

<p>
   Click on VM-osTicket (VM-osTicket\labuser) on the left side-bar. On the right side-bar, click on Restart to reload IIS to ensure that our PHP configuration is applied.  
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/32589abd-b198-4629-be0c-50ff355bf4a1)

**7. Open osTicket on the browser and enable additional PHP extensions.**

<p>
   Go to the Installation Files folder and download "osTicket-v1.15.8.zip".
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/b907cc40-65b8-47e9-b2ba-25aff79580db)

<p>
   When the file has downloaded, navigate inside the osTicket-v1.15.8 file in File Explorer. Open another File Explorer window and navigate to C:\inetpub\wwwroot. Copy the upload folder insider osTicket-v1.15.8 into the wwwroot folder. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/05ff9467-6aa5-4bef-a3b5-14520ad58c5d)

<p>
   When the upload folder has finished copying into the wwwroot folder, rename it to "osTicket".
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/8b30f563-ac92-46d8-8a64-70aedfacdaef)

<p>
   Go back to the IIS Manager and restart the VM-osTicket server. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/ba824cfc-ec94-41f6-bc32-97990967c2b0)

<p>
   On the left sidebar, go to Sites > Default Web Site > osTicket. Then click Browse *:80 (http) on the right sidebar. Inside the browser, osTicket Installer should appear stating that some extensions are not enabled. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/afbb132b-4ad2-48f8-8fc2-c0df78b6b333)

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/1f098047-8b34-4ed8-81e3-50c104e05911)

<p>
   To enable the additional PHP extensions, go back to IIS Manager with osTicket selected on the left sidebar. Open PHP Manager.  
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/33c09c97-3543-409e-a517-38ace97b12a6)

<p>
   Inside PHP Manager, under PHP Extensions, click on Enable or disable an extension. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/63c6dda6-5f77-423b-895e-7bc771026e1f)


<p>
   We will enable the following additional extensions: php_imap.dll, php_intl.dll, and php_opcache.dll. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/7908f3e1-f273-41e9-8adc-c5a43364d158)

<p>
   Go back to the osTicket Installer on the browser and refresh the page. We see that the additional PHP extensions we enabled now have a green check mark. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/98cfcd40-c084-4ada-acac-2411d42dae95)

**8. Rename "ost-sampleconfig.php" to "ost-config.php". Assign the necessary permissions in ost-config.php.**
<p>
   Go to File Explorer and navigate to the C:\inetpub\wwwroot\osTicket\include folder. Scroll down to find the ost-sampleconfig.php file and rename it to "ost-config.php".
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/725f0d96-7ca8-4179-917f-acdd4bfdb9c7)

<p>
   Now we'll change the permissions of ost-config.php to alow Everyone to make changes to the file. This ensures that the osTicket Installer will have no trouble editing the file during installation. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/b0231e6e-0206-4f02-b765-9de5fa882235)

<p>
   Inside ost-config.php Properties, go to the Security tab and click Advanced. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/5fdd18fd-65e4-4fb2-a84f-8c0caf0bd53d)

<p>
   Inside Advanced Security Settings, click on Disable inheritance and select Remove all inherited permissions from this object.
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/c766b90e-c19c-4a76-87df-7127e11dd766)

<p>
   After all previous permissions have been cleared, click Add. Click Select a principal. In the text box, enter "everyone" and click on Check Names. Then click OK. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/ea31b028-8fb0-4504-ab1f-473c0855bb89)

<p>
   Check the Full control box and click OK. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/71bd9d93-ef14-4ad6-94c2-e1b8205c8e66)

<p>
   Click Apply to apply the permissions then click OK. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/bd2abb88-433e-42ad-b9cc-a812d93d6135)

**9. Continue osTicket setup in the browser. Download and install HeidiSQL from the installation files and create an "osTicket" database. Finish the osTicket setup and install.**
<p>
   Go back to osTicket Installer on the Browser and click Continue. For Helpdesk Name, enter "<your name> Help Desk". For Default Email, enter "<your name>@osticket.com". Enter your First Name, Last Name, and Email Address. For Username, we will use "user_admin". For Password, we'll use "Password1". Make sure to record the username and password for future reference.
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/f61c05c4-f903-46cc-8bf2-d5bc4fd4bba7)

<p>
   Before setting up the Database Settings, we will need to create a database for osTicket. From the Installation Files folder, download HeidiSQL. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/557f76ee-e12d-4037-b184-c0b0e54d1122)

<p>
   When the file has finished downloading, open the file and proceed with the HeidiSQL installation wizard. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/c3f12579-270f-46ab-8af1-cc22378c1792)

<p>
   Inside HeidiSQL, click on New to create a new session and enter the root MySQL password, "Password1", we previously setup. Click Open. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/075237c1-9a4c-4f1c-ba8e-47b9abc9784e)

<p>
   to create a new database, right-click Unnamed and select Create new > Database. We will name the databse "osTicket". Click OK. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/23159f78-dc5b-4a36-8ec3-c04f06766313)

<p>
   Back in osTicket Installer, we can proceed with the Database settings. For MySQL Database, enter "osTicket", the database we just created. For MySQL Username, enter "root". For MySQL Password, enter "Password1". Click Install Now. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/27a430ee-e9ca-4d1f-95c2-4cb7b6f1a883)

<p>
   We have successfully installed osTicket. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/b98e778e-62b1-49ab-94ac-9ca226f0daf2)

**10. Perform final cleanup.**
<p>
   We will now perform a final cleanup before concluding the installation process. In File Explorer, navigate to C:\inetpub\wwwroot\osTicket and delete the setup folder. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/42dac6b3-7cb0-4c7e-b920-af41e4228976)

<p>
   Now, navigate to C:\inetpub\wwwroot\osTicket\include and change the permissions of ost-config to Read only. Click OK. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/6ee4a849-54ad-45d4-8938-f96aec444b92)

<p>
   Now we'll try logging in to osTicket as an Admin. Back in the browser, copy, paste, and go to the following URL: http://localhost/osTicket/scp/login.php. Login to osTicket with "user_admin" for the username and "Password1" as the password. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/e1cbcf0e-6f3c-4744-bcbe-04a6578f6eaf)

<p>
   We are successfully able to login to osTicket. 
</p>

![image](https://github.com/marbienjimeno/osticket-prereqs/assets/29347863/fc1dffe4-1800-4c88-b1a3-eca4cfc49fe5)

**Conclusion**
<p>
   In this lab, we installed osTicket and its necessary prerequisites. We were able to login as an Admin and confirm that it was successfully installed. In the next section, we will perform the post-installation configuration of osTicket. 
</p>
