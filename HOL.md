<a name="Title" />
# Deploying a SharePoint Farm with Windows Azure Virtual Machines #

---

<a name="Overview" />
## Overview ##

In this hands-on lab you will learn how to create a SharePoint farm, connected with Active Directory and SQL Server.	

<a name="Objectives" />
### Objectives ###

In this hands-on lab, you will learn how to:

1. Use the Windows Azure Portal to create a Sharepoint Image
1. Connect two virtual machines to the same cloud service for network connectivity.
1. Create and Configure SharePoint Server Farm.

<a name="Prerequisites" />
### Prerequisites ###

The following is required to complete this hands-on lab:

- Complete the _Deploying Active Directory_ HOL
- Complete the _Deploying SQL Server for SharePoint_ HOL
- [Windows Azure PowerShell CmdLets](http://msdn.microsoft.com/en-us/library/windowsazure/jj156055)
- A Windows Azure subscription with the Virtual Machines Preview enabled - you can sign up for free trial [here](http://bit.ly/WindowsAzureFreeTrial)

>**Note:** This lab was designed to use Windows 7 Operating System.

---
 
<a name="Exercises" />
## Exercises ##

This hands-on lab includes the following exercises:

1. [Creating a SharePoint Image](#GettingStarted)
1. [Create a SharePoint VM](#Exercise1)
1. [Configuring Load Balancing](#Exercise2)
 
Estimated time to complete this lab: **50 minutes**.
	
<a name="GettingStarted" />
### Getting Started: Creating SharePoint Image ###

You will now create the SharePoint Server disk image required to run this hands-on lab.

Make sure you have this image created before starting with the lab.	

<a name="GSTask1" />
#### Task 1 - Create a Windows Server VM from the Windows Azure portal ####

In this task, you will create a Windows Server 2008 Virtual Machine using the Windows Azure Management Portal. Then you will use this VM to install SharePoint and generate a reusable image.

1. Navigate to <https://manage.windowsazure.com> using a web browser, and sign in using your Microsoft account.

1. Click **NETWORKS** in the left pane. Select the desired **Virtual Network** and copy it's Affinity Group name. You will use this name later on to create the new  VM.

	![Virtual Networks](images/virtual-networks.png?raw=true "Virtual Networks")
	
	_Virtual Networks_

1. Click the **NEW** link located at the bottom of the page.

	![Creating a New VM](images/creating-a-new-vm4.png?raw=true "Creating a New VM")

	_Creating a New VM_

1. Click the **VIRTUAL MACHINE** link and then select **FROM GALLERY** 

	![New VM from gallery](images/new-vm-from-gallery.png?raw=true "New VM from gallery")

	_New VM from gallery_

1. In the **VM OS Selection** page, click **PLATFORM IMAGES** and then select **Windows Server 2008 R2 SP1** from the OS's list. Then click **Next**.

	![VM OS Selection](images/vm-os-selection.png?raw=true "VM OS Selection")
	
	_VM OS Selection_

1. Set the VM's name to _SPImage1_ and set a new password for the machine's Administrator by completing the **New Password** and **Confirm Password** fields. Then set the **Size** to _Small_ and click the **Next** button.

	![New VM Configuration](images/new-vm-configuration3.png?raw=true "New VM Configuration")

	_New VM Configuration_

1. In the **VM Mode** page, leave mode as _Standalone Virtual Machine_, set the **DNS Name** to _SPImage1_. Click the **Next** button to continue.


	![Configuring the VM mode](images/configuring-the-vm-mode.png?raw=true "Configuring the VM mode")

	_Configuring the VM mode_

 
1. In the Virtual Machines section, you will see the Virtual Machine you created with a _provisioning_ status. Wait until it changes to _Running_ in order to continue with the following task.

<a name="GS2Task2" />
#### Task 2 - Install SharePoint and Dependencies ####

In this task, you will download and install SharePoint 2010 and its dependencies.

1. In the **Windows Azure Portal**, within **Virtual Machines** section, select the _SPImage1_ virtual machine you created in Task 1, and click **Connect**.
1. Download the remote desktop client. Click **Open** and log on using the Administrator's credentials you defined when creating the VM.
1. Now, you need to download and install **SharePoint 2010**. In order to enable downloads from Internet Explorer you will need to update **Internet Explorer Enhanced Security Configuration**. In the Azure VM, open **Server Manager** from **Start | All Programs | Administrative Tools**.
1. In the **Server Manager**, click **Configure IE ESC** within **Security Information** section.

	![Internet Explorer Enhanced Security](images/internet-explorer-enhanced-security.png?raw=true)
		
	_Internet Explorer Enhanced Security_
 
1. In the **Internet explorer Enhanced Security** configuration, turn **off** the enhanced security for **Administrators** and click **OK**.

	![Internet Explorer Enhanced Security(2)](images/internet-explorer-enhanced-security2.png?raw=true)
	 
	_Internet Explorer Enhanced Security_
 
	>**Note:** Modifying **Internet Explorer Enhanced Security** configurations is not good practice and is only for the purpose of this particular lab. The correct approach should be to download the files locally and then copy them to a shared folder or directly to the VM.

1. Now that you have permissions to download files, open an **Internet Explorer** browser and browse to [http://sharepoint.microsoft.com/en-us/Pages/Try-It.aspx](http://sharepoint.microsoft.com/en-us/Pages/Try-It.aspx). Click **Download it now**.
1. Select **SharePoint Server for Internet Sites, Standard** version from the dropdown list and click **Get Started Now**.
1. Sign in with your **Windows Live ID** account and complete the **SharePoint Server 2010 for Internet Sites, Standard Trial** download. You will receive an email with the product key for your trial version.
1. Once the download is completed, double-click the downloaded **SharePointServer.exe** file.
1. Click **Install software prerequisites** and follow the **Microsoft SharePoint 2010 Products Preparation Tool** wizard to install the required products and updates.

	![SharePoint Server 2010 Installation Wizard](images/sharepoint-server-2010-installation-wizard.png?raw=true)
	 
	_SharePoint Server 2010 Installation Wizard_
 
1. Go back to **SharePoint Server 2010 Installation Wizard** and click **Install SharePoint Server** link.

	![SharePoint Server 2010 Installation Wizard(2)](images/sharepoint-server-2010-installation-wizard2.png?raw=true)
	 
	_SharePoint Server 2010 Installation Wizard_
 
	>**Note:** You might need to restart the VM before installing the SharePoint Server. If the SharePoint Server installation instructs you to restart the computer, do so now.
	
	>After the system restarts, open the SharePoint installer and continue with the steps.
	
1. In the **Enter your Product Key** dialog, use the **product key** you received by email. Make sure you use the product key for **SharePoint Server 2010 for Internet Sites, standard** trial version.

	![Microsoft SharePoint Server 2010 Trial](images/microsoft-sharepoint-server-2010-trial.png?raw=true)
	 
	_Microsoft SharePoint Server 2010 Trial_
 
1. Read and accept the License Terms and click **Continue**.
1. In the **Choose the installation you want** page, click **Server Farm**.
	 
	![Microsoft SharePoint Server 2010 Trial(2)](images/microsoft-sharepoint-server-2010-trial2.png?raw=true)

	_Microsoft SharePoint Server 2010 Trial_ 

1. In the **Server Type** page, select **Complete** option and then click **Install Now** to begin with the SharePoint Server 2010 installation.
	 
	![Microsoft SharePoint Server 2010 Trial(3)](images/microsoft-sharepoint-server-2010-trial3.png?raw=true)
	
	_Microsoft SharePoint Server 2010 Trial_
 
1. In the **Run Configuration Wizard** page, unselect the **Run the SharePoint Products Configuration Wizard now** check box and click **Close.**

	![Microsoft SharePoint Server 2010 Trial(4)](images/microsoft-sharepoint-server-2010-trial4.png?raw=true)
	 
	_Microsoft SharePoint Server 2010 Trial_
 
1. Click **Exit** to close the SharePoint Server installation wizard.

1. Go to [http://www.microsoft.com/download/en/details.aspx?id=26623](http://www.microsoft.com/download/en/details.aspx?id=26623), download the **Service Pack 1 for SharePoint Server 2010** and Install it.

	>**Note:** The Service Pack 1 is required to install SharePoint with SQL Server 2012.

<a name="GS2Task3" />
#### Task 3 - Capture the VM and generate an image ####

In this task, you will create an image based on the virtual machine you created in the previous tasks. Then you will reuse that image for creating a SharePoint Virtual Machine.

1. In the Virtual Machine, open the **Command Prompt** from **Start | All Programs | Accessories** as Administrator.
1. You will now run **Sysprep** tool to prepare the VM for duplication. You have to run **Sysprep** if you want to capture an image of an existing VM and reuse it for creating new ones.
	
	<!--mark: 1-->
	````Script
	%WINDIR%\system32\sysprep\sysprep.exe /oobe /generalize /shutdown
	```` 
	
	>**Note:** System Preparation Utility (Sysprep) is the Tool that Microsoft provides for preparing a Windows Installation for duplication, auditing and customer delivery. For more information about Sysprep tool, refer to this TechNet article: [http://technet.microsoft.com/en-us/library/cc721940(WS.10).aspx](http://technet.microsoft.com/en-us/library/cc721940(WS.10\).aspx).

1. Wait until the **System Preparation Utility** finishes, it will shut down the VM.
1. If not already opened, navigate to <https://manage.windowsazure.com> using a web browser, and sign in using your Windows account.
1. Click **Virtual Machines** on the left panel menu.
1. Select the Virtual Machine _SPImage1_ you created in Task 1 and click **Shutdown**. Wait until the VM is stopped.
1. Now click **Capture** to create an image from it.

	![Capture button](images/capture-button.png?raw=true "Capture button")

	_Capture VM_

1. In the **Capture** dialog, set the **Virtual Machine Name** to _sp2010syspreped_, complete the **Admin Password** and select the **Virtual Machine has been sysprepped** check box.
	 
	![Capturing a VM Image](images/capturing-a-vm-image.png?raw=true "Capturing a VM Image")
	
	_Capturing a Virtual Machine Image_ 

1. Wait until the image is created before continue to the next task.
	 
	![Creating a Virtual Machine Image(3)](images/creating-a-virtual-machine-image3.png?raw=true)

	_Creating a Virtual Machine Image_

<a name="Exercise1" />
### Exercise 1: Create a SharePoint Virtual Machine ###

In this exercise, you will create a SharePoint VM.
	
<a name="Ex1Task1" />
#### Task 1 - Create a SharePoint virtual machine from the SharePoint Base Image ####

In this task, you will create a SharePoint virtual machine from the Base Image you created in the previous task using the **Windows Azure Portal**. You will latter use this VM to configure the SharePoint Farm.

1. If you do not have the IP address of the Domain Controller Virtual Machine, Navigate to the **Windows Azure Portal** using a Web browser and sign in using the **Microsoft Account** associated with your Windows Azure account.

1. Go to **Virtual Machines**, select the VM where you deployed the AD and select the **Connect** button at the bottom panel.

1. In the VM, go to **Start**, type **cmd** and press **ENTER**.

1. Type **ipconfig** and press **ENTER**. Take note of the **IPv4 address**, you will use it later on this exercise. Close the **Remote Desktop** connection.

1. Open **Windows Azure PowerShell** from **Start** | **All Programs** | **Windows Azure** | **Windows Azure PowerShell**, right-click **Windows Azure Powershell** and choose **Run as Administrator**.

1. Execute the following command to obtain the names of the available OS Disk images. Take note of the **SharePoint** image disk name you created in the **Getting Started** section of this lab.

	<!-- mark:1 -->
	````PowerShell
	Get-AzureVMImage | Select ImageName
	````

1. Execute the following command to define the OS disk image name for the new Virtual Machine.
	
	<!-- mark:1 -->
	````PowerShell
	$imgName = '[SHAREPOINT-IMAGE-NAME]'
	````

1. Set up the VM's DNS settings. To do this, you will use the Virtual Machine you created in Exercise 1 were you configured the Active Directory. Replece the placeholders before executing the following command. Use the IP address you took note at the beginning of the exercise.
	
	<!-- mark:1-4 -->
	````PowerShell
	$advmIP = '[AD-IP-ADDRESS]'
	$advmName = '[AD-VM-NAME]'
	# Point to IP Address of Domain Controller Created Earlier
	$dns1 = New-AzureDns -Name $advmName -IPAddress $advmIP
	````


1. Set up the new VM's configuration settings to automatically join the domain in the provisioning process. Before executing the command, replace the placeholders with the administrator and domain passwords.

	<!-- mark:1-12 -->
	````PowerShell
	$vmName = 'spvm1'
	$adminPassword = '[YOUR-PASSWORD]'
	$domainPassword = '[YOUR-PASSWORD]'
	$domainUser = 'administrator'
	$FQDomainName = 'contoso.com'
	$subNet = 'AppSubnet'
	# Configuring VM to Automatically Join Domain
	$advm1 = New-AzureVMConfig -Name $vmName -InstanceSize Small -ImageName $imgName | 
				Add-AzureProvisioningConfig -WindowsDomain -Password $adminPassword `
				-Domain 'contoso' -DomainPassword $domainPassword `
				-DomainUserName $domainUser -JoinDomain $FQDomainName |
		 Set-AzureSubnet -SubnetNames $subNet
	````

	>**Note:** The previous command asumes that you used the proposed names for the Domain Name and the Subnets that are shown in the **Deploying Active Directory** hands on lab. You may need to update the values if you used different names.

1. Create a new Virtual Machine using the Domain and DNS settings you defined in the previous steps. Replace the placeholder with a unique Service Name.

	````PowerShell
	$serviceName = [YOUR-SERVICE-NAME]
	$affinityGroup = 'adag'
	$adVNET = 'ADVNET'
	# New Cloud Service with VNET and DNS settings
	New-AzureVM –ServiceName $serviceName -AffinityGroup $affinityGroup `
									-VMs $advm1 -DnsSettings $dns1 -VNetName $adVNET
	````

1. Once the provisioning proces finish, connect to the VM using Remote Desktop and verify if it was automatically joined to your existing domain. To do so, open server manager and verify that the machine is joined to the domain.

	![VM joined to the domain](images/vm-joined-to-the-domain.png?raw=true "VM joined to the domain")

	_VM joined to the domain_


1. Repeat steps 9 to 11 but use _spvm2_ as the **$vmName**. This second VM will be used to create the SharePoint farm.

<a name="Ex1Task2" />
#### Task 2 - Configure SharePoint ####

In this task, you will configure the SharePoint VM to create and connect to the SharePoint Farm. 

1. If not already opened, navigate to <https://manage.windowsazure.com> using a web browser, and sign in using your Windows account.
1. In the **Virtual Machines** section, select the first SharePoint VM (_SPVM1_) and click **Connect** to connect using **Remote Desktop Connection**.
1. Open the **SharePoint 2010 Products Configuration Wizard** from **Start | All Programs | Microsoft SharePoint 2010 Products**.
1. Follow the **SharePoint Products Configuration Wizard**. In the **Connect to a server farm** page, select **Create a new server farm** option.
	 
	![Create a new server farm](images/create-a-new-server-farm.png?raw=true)

	_Create a new server farm_
 
1. In the **Specify Configuration Database Settings** page, complete the fields with the following information and click **Next**.

	1. **Database Server**: type the name of the computer where you installed SQL Server.
	
	1. **Database name**: type the name for your SharePoint configuration database. The default name is _SharePoint_Config_.

	1. **Username**: type the user name for the server farm account. Ensure that you type the user name in the format DOMAIN\user name. For testing purposes this can be the contoso\administrator account you created in the deploying Active Directory hands on lab.
	
		>**Note**: The server farm account is used to create and access your configuration database. It also acts as the application pool identity account for the SharePoint Central Administration application pool, and it is the account under which the Microsoft SharePoint Foundation Workflow Timer service runs. The SharePoint Products Configuration Wizard adds this account to the SQL Server Login accounts, the SQL Server dbcreator server role, and the SQL Server securityadmin server role. The user account that you specify as the service account must be a domain user account, but it does not need to be a member of any specific security group on your front-end Web servers or your database servers. We recommend that you follow the principle of least privilege and specify a user account that is not a member of the Administrators group on your front-end Web servers or your database servers.
		>
		>Find more information about this topic [here](http://technet.microsoft.com/en-us/library/cc287960.aspx).
	
	1. **Password**: type the user’s password.

		![Configuration Database Settings](images/configuration-database-settings.png?raw=true)
	 
		_Configuration Database Settings_
 
1. In the **Specify Farm Security Settings** page, type a _passphrase_ and click **Next** to continue.

	![Farm Security Settings](images/farm-security-settings.png?raw=true)
	 
	_Farm Security Settings_
 
	>**Note:** A passphrase is similar to a password, but it is usually longer to enhance security.

1. In the **Configure SharePoint Central Administration Web Application** page, choose _NTLM_ as **Authentication provider** and click **Next**.

	![Configure SharePoint Central Administration Web Application](images/configure-sharepoint-central-administration-w.png?raw=true)
	 
	_Configure SharePoint Central Administration Web Application_
 
1. Review your configuration settings and click **Next**. Once the configuration settings are applied click **Finish**.

	![Completing the SharePoint Products Configuration Wizard](images/completing-the-sharepoint-products-configurat2.png?raw=true)
	 
	_Completing the SharePoint Products Configuration Wizard_
 
1. Now, you will enable Anonymous Access in your SharePoint Server. To do this, open **SharePoint Central Administration** from **Start | All Programs | Microsoft SharePoint 2010 Products**.

1. In the **Central Administration** section, under **Application Management**, click **Manage web applications** link.

	![SharePoint Central Administration](images/sharepoint-central-administration.png?raw=true)
	 
	_SharePoint Central Administration_
 
1. Select the default SharePoint site _SharePoint - 80_ and click **Authentication Providers** in the **Web Application** tab.

	![Manage Web Applications page](images/manage-web-applications-page.png?raw=true)
		
	_Manage Web Applications page_
 
1. In the **Authentication Providers** dialog box, click **Default** zone.

	![Authentication Providers Zone](images/authentication-providers-zone.png?raw=true)
	 
	_Authentication Providers Zone_
 
1. In the **Edit Authentication** dialog, locate **Anonymous Access** section and select **Enable anonymous access** check box.

	![Edit Authentication](images/edit-authentication.png?raw=true)
	 
	_Edit Authentication_
 
1. Back in the **Web Application Management** page, in the **Web Applications** tab, click **Anonymous Policy**.

	![Anonymous Policy](images/anonymous-policy.png?raw=true)
	 
	_Anonymous Policy_
 
1. In the **Anonymous Access Restrictions** dialog, locate **Permissions** section and select _None - No Policy_ as **Anonymous User Policy**.

	![Anonymous Access Restrictions](images/anonymous-access-restrictions.png?raw=true)
	 
	_Anonymous Access Restrictions_ 

1. Now, configure the second SharePoint VM (_SPVM2_) to connect to the SharePoint farm you created in the previous steps. To do this, go back to the **Windows Azure Portal** and go to **Virtual Machines** section.
1. Select the second SharePoint VM (_SPVM2_) and click **Connect** to connect using **Remote Desktop Connection**.
1. Open the **SharePoint 2010 Products Configuration Wizard** from **Start | All Programs | Microsoft SharePoint 2010 Products**.
1. Follow the **SharePoint Products Configuration Wizard**. In the **Connect to a server farm** page, select **Connect to an existing server farm** option.
	  
	![SharePoint Configuration Wizard](images/sharepoint-configuration-wizard.png?raw=true)

	_SharePoint Configuration Wizard_
 
1. In the **Specify Configuration Database Settings** page, type the name of the SQL Server instance in the **Database Server** box and click **Retrieve Database Names**.
1. In the **Database name** list, select the Configuration database’s name and click **Next**.
1. In the **Specify Farm Security Settings** page, type the **passphrase** you set in the SharePoint Server Farm and click **Next**.
1. Complete the **SharePoint Products Configuration Wizard**. Once the wizard finishes, it will launch the **Farm Configuration Wizard**. You do not need to run this wizard, close it to continue.

<a name="Verification" />
### Verification - Create a New SharePoint Site Collection ###

In this task, you will verify that the SharePoint Server was correctly configured by creating a new SharePoint Site Collection.

1. If not already connected, connect to the first SharePoint VM (_SPVM1_) using **Remote Desktop Connection**.
1. Open **SharePoint 2010 Products Central Administrator** from **Start | All Programs | Microsoft SharePoint 2010 Products**.
1. Create a new Site Collection. To do this, click **Create Site Collection** link under **Application Management** section within **Central Administration** page.

	![Application Management - Create Site Collections](images/application-management---create-site-collecti.png?raw=true)
	 
	_Application Management - Create Site Collections_
 
1. In the **Create Site Collection** page, type **Title** and **Description** for the site collection. In the **Web Site Address** section, select _/sites/_ from the dropdown list and enter _SPFWebApp_.

	![Create Site Collection](images/create-site-collection.png?raw=true)
	 
	_Create Site Collection_
 
1. In the **Template Selection** section, switch to **Publishing** tab and select **Publishing Portal** template. Then complete the **Primary and Secondary Site Collection Administrators**, use _azuretk\Administrator_.

	![Create Site Collection(2)](images/create-site-collection2.png?raw=true)
	 
	_Create Site Collection_
 
1. Leave the **Quota Template** with the default value and click **OK** to create the new Site Collection.
1. Once the Site Collection is ready, you will see a successfully created message. To test the site, click URL shown.

	![Site Collection Created](images/site-collection-created.png?raw=true)
	 
	_Site Collection Created_
 
1. You will see an Access Denied message. To enter the site, you will need to log on using a domain account (i.e.: The one you used for the Primary Site Collection Administrator).

	![Logging On the New Site](images/logging-on-the-new-site.png?raw=true)
	 
	_Logging On the New Site_
 
1. Once logged on, you will see a site like the following one.

	![Site's Home Page](images/sites-home-page.png?raw=true)
	 
	_Site’s Home Page_

1. Once in the site click on **Site Actions** | **Site Permissions** and click on **Anonymous Access**.

	![ConfigureAnonymous](images/configureanonymous.png?raw=true)

	_Configuring anonymous access_

1. Change settings to **Entire Web Site** and click **OK**.

	![changeanonperms](images/changeanonperms.png?raw=true)

1. Now, test the SharePoint Farm connecting to the second SharePoint VM (_SPVM2_). To do this, go back to the **Windows Azure Portal** and go to **Virtual Machines** section.
1. Select the second SharePoint VM (_SPVM2_) and click **Connect** to connect using **Remote Desktop Connection**.
1. Open **SharePoint 2010 Products Central Administrator** from **Start | All Programs | Microsoft SharePoint 2010 Products**.
1. In the **Central Administration** page, click **Manage Web Applications** under **Application Management**.
1. In the **Web Application** page, select the web application and click **Extend**.
1. Click **Application Management** in the left menu and then click **View all sites collections** link.
1. Select the site you created in the first SharePoint server (SPFWebApp), copy the site’s URL and paste it in an Internet Explorer browser. If the site is working properly, you will be able to log on and access to the same home page you accessed from the first SharePoint server.

<a name="Exercise2" />
### Exercise 2: Configuring Load Balancing ###

#### Task 1 - Adding load balancing endpoints in Windows Azure portal ####

1. In the Windows Azure Portal click on the first VM **SPVM1 | Endpoints | Add Endpoint** to open the endpoint create wizard.

	1. In the **Add endpoint to virtual machine** page, select **Add Endpoint** option and then click the **right arrow** to continue.

		![add-endpoint](images/add-endpoint.png?raw=true)

		_Selecting Add endpoint_

	1. In the **Specify endpoint details** page, enter _web_ in the name field, select the **TCP** protocol, and enter _80_ in the public and private port field. Finally, click the arrow to confirm the endpoint creation.

		![webendpoint](images/webendpoint.png?raw=true)

		_Creating a web endpoint_

1. Once the web endpoint is created in the first VM, you will access the second VM and add a load balancing endpoint. Enter the second VM dashboard, click **Endpoints** link, and click **Add Endpoint** button on the bottom bar to start the endpoint creation wizard.

	1. In the **Add endpoint to virtual machine** page, select **Load-balance traffic on an existing endpoint** option. Then, select **web** endpoint from the list and click the arrow to continue.

		![Add load balancing endpoint wizard](images/add-load-balancing-endpoint-wizard.png?raw=true "Add load balancing endpoint wizard")
		
		_Add load balancing endpoint wizard_

	1. In the **Specify endpoint details** page, define the same settings as the previous endpoint. Enter a Name (e.g. web) and a private port (e.g. 80). Click the arrow to create the load balancing endpoint.

		![Load balancing endpoint details](images/load-balancing-endpoint-details.png?raw=true "Load balancing endpoint details")

		_Load balancing endpoint details_


1. Wait until the endpoint is created, and the load balancing is enabled in both VMs.

1. To verify, select the endpoint in the list and click **Edit endpoint**. 

	![Edit Endpoint](images/edit-endpoint.png?raw=true "Edit endpoint")

	_Edit Endpoint_

1. Notice that both VMs are configured as load-balanced machines. If you enter to the first VM and edit its web endpoint, it will show the same configuration.

	![Edit endpoint details](images/edit-endpoint-details.png?raw=true "Edit endpoint details")

	_Edit endpoint details_

1. Enter SPVM1 dashboard and locate the quick glance section. Take note of the VM DNS and IP.

	![VM IP load balancing](images/vm-ip-load-balancing.png?raw=true "VM IP load balancing")

	_VM IP load balancing_

1. Now enter SPVM2 dashboard and locate the quick glance section. Notice that the both VMs have the same virtual IP address and URL. That means, the load balancing is transparent for the user when a web site is retrieved. Internally, Windows Azure will redirect the traffic to either SPVM1 or SPVM2 hosts.

	![VM IP load balancing 2](images/vm-ip-load-balancing-2.png?raw=true "VM IP load balancing 2")

	_VM IP load balancing 2_

1. Finally, start a new browser session and browse to the VM URL. 

