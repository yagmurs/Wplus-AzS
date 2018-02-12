#WorkshopPLUS – Architecting Hybrid Cloud Solutions – Azure Stack
#Introduction
> [!ALERT] Introduction Information will be completed

===
#Lab 01 - Deploy Azure VM to host Azure Stack Development Kit (ASDK) in connected mode

##^[**Objectives and Summary**][lab01-os]

> [lab01-os]:
> ###Introduction
>> This Lab covers configuring Azure Active Directory tenant to be used for ASDK installation and Installing Azure Stack Development Kit on an Azure VM, registering Azure Stack Installation to Azure on **connected** mode. 
>
> ###Objectives
> In this lab, you will:
> - Configure Azure Active Directory tenant
> - Deploy Azure VM to host ASDK
> - Deploy ASDK on Azure VM
> - First logon to Azure Stack Environment
> - Setup Management Environment
> - Register Azure Stack to Azure
> ###Prerequisites
>>> [!ALERT] There are no prerequisites for this lab
>
> ###Variables
>> Azure Stack Administrator password, Azure Active Directory Identity will be used to logon Azure and Azure Stack portals.
>>
>>  **@lab.VirtualMachine(55267).Username**
>> 
>>
>> Following scheme stands for password you have assigned to **Administrator**. Same password is used for *AZURESTACK\AzureStackAdmin*, and *AZURESTACK\CloudAdmin* which will be used on Azure Stack development kit. These are AZURESTACK domain identities that may be used to logon Azure VM.
>>
>>  **@lab.VirtualMachine(55267).Username**
>> 
>
> ###Estimated time to complete this lab
>> **6 Hours**
>
> ###Scenario
>> Deploy Azure Stack Development Kit on an Azure VM and register to Azure on **connected** mode.

===
#Lab 01 - Exercise #1 - Deploy Azure Stack host using ARM Template
- [] Logon [Azure Portal][azure-portal] using your Microsoft Account **@lab.UserEmail**

    > [!ALERT] If you don't have Microsoft Account, please [Create a new Microsoft Account][rl1]

- [] [*Register Azure Pass*][azure-pass]
- [] On Main menu click **More services** > **Azure Active Directory**
- [] On Quick tasks click **Add a user**.

Specify the following settings.
> - Name: **@lab.UserFirstName**
> - User name: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
> - Profile: **Not configured**
> - Directory Role: **Global administrator**
> - Password: check **Show Password**
> - **Copy** the password to **clipboard**

- [] Click **Create**

> [!KNOWLEDGE] Tenant user now created, needs to be the **Owner** for the Azure subscription

- [] On Main menu click **More services** > **Subscriptions**
- [] On the subscriptions blade click on your **Azure Pass** Subscription
- [] On the Azure Pass blade click **Access control (IAM)** > **+Add**

On the Add permissions blade, specify the following settings
> - Role: **Owner**
> - Assign Access to: **Azure AD user,group or application**
> - Select: **@lab.UserFirstName**

- [] User should be listed. Click @lab.UserFirstName@<\tenant_name>.onmicrosoft.com to add user to Selected members
- [] Click **Save**

- [] Sign out from current session.
- [] Logon [Azure Portal][azure-portal] with **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com** and use the temporary password previously saved
- [] You will be prompted to change the password.
- [] Enter previous password and **@lab.VirtualMachine(55267).Password** twice. Click **OK**

    > [!ALERT] This will make sure that you have changed the temporary password

> [!KNOWLEDGE] Above instructions have configured Azure Active Directory to start Azure Stack Development Kit (ASDK) installation

> [!KNOWLEDGE] Now, we may start deploying Azure VM to host ASDK

- [] Open up new browser and go to [https://github.com/yagmurs/AzureStack-VM-PoC][github-repo]
- [] Click [**Deploy to Azure**][github-repo-deploytoazure] button to deploy Azure Stack host template to Azure
- [] Browser will be redirected to your <\tenant_name>.onmicrosoft.com portal with ARM template settings to be completed
- [] On **Customized template** blade > **Resource Group**
- [] Click **Create New**

On Custom deployment blade, Specify the following settings
> - Resource Group: **AzureStackPremierWorkshop**
> - Location: **<\Any region that is closer to your location>**
> - Virtual Machine Name: **AzS-Host1**
> - Admin Password: **@lab.VirtualMachine(55267).Password**
> - Public DNS Name: **<\Any name that does not conflict>**

- [] Check TERMS AND CONDITIONS and click **Purchase**

> [!NOTE] VM installation takes about 10 to 20 minutes depends on the region and load.

===

#Lab 01 - Exercise #2 - Deploy Azure Stack Development Kit (ASDK)

- [] Logon [Azure Portal][azure-portal] using your Microsoft Account **@lab.UserEmail**

- [] On Main menu click **More services** > **Virtual Machines**

- [] On Virtual Machines blade > right click on created VM **AzS-Host1** > click **Connect**

- [] Logon to host VM with RDP client using **Administrator** user with **@lab.VirtualMachine(55267).Password**

- [] On desktop right click **Install-ASDK.ps1**, click **Run with PowerShell** to run. 

Within the PowerShell window, specify the following information.
> - Execution Policy Change: **Y** to continue if prompted
> - Local 'Administrator' Password: **@lab.VirtualMachine(55267).Password** twice
> - Azure AD Global Administrator user: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
> - Azure AD Global Administrator password: **@lab.VirtualMachine(55267).Password** twice
> - Admin Password: **@lab.VirtualMachine(55267).Password**
> - Select: **1**

> [!ALERT] Unless you instructed by the trainer, always select **1** to install the latest ASDK version

- [] Enter to continue 

Automated setup will continue as follows:
> [!KNOWLEDGE] 
- Download latest version of ASDK
- Extract files to D:\
- Copy required files to C:\
- Extract setup packages to C:\
- Run C:\CloudDeployment\Setup\InstallAzureStackPOC.ps1 with required parameters

> [!ALERT] During ASDK deployment you may lose connectivity to host due to network configuration changes.

> [!KNOWLEDGE] Azure Stack host will also reboot during the deployment to join the host to AZURESTACK domain.

> [!KNOWLEDGE] To follow setup progress after reboot, logon using AZURESTACK\AzureStackAdmin and password @lab.VirtualMachine(55267).Password

> [!ALERT] This process takes about 5-6 hours to complete

===

#Lab 01 - Exercise #3 - First logon to Azure Stack Admin and Tenant Portal

> [!KNOWLEDGE] Now Azure Stack Development Kit is installed

##Explore Admin Portal
- [] Logon Azure Stack host VM with RDP client using following information;
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password** 

- [] On Main menu click **+New** > **Offers + Plans** and examine the Offers, Plans and Quotas

    > [!NOTE] Do not set up any of them yet, we will be configuring later. Close **Offers + Plans** blade 

- [] On Main menu click **+New** > **Compute**. Notice that there are only two items yet, **Availability Set** and **Storage Account**

    > [!NOTE] Do not set up any of them yet, we will be configuring later. Close **Compute** blade 

- [] On Main menu click **+New** > **Data + Storage** and examine.

    > [!NOTE] Do not set up any of them yet, we will be configuring later. Close **Compute** blade 

- [] On Main menu click **+New** > **Networking** and examine.

    > [!NOTE] Do not set up any of them yet, we will be configuring later. Close **Compute** blade 

- [] On Main menu click **+New** > **Security + Identity** and examine.

    > [!NOTE] Do not set up any of them yet, we will be configuring later. Close **Compute** blade

- [] On Main menu click **+New** > **Capacity** and examine.

    > [!NOTE] Do not set up any of them yet, we will be configuring later. Close **Compute** blade 

##Explore Tenant Portal 

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Tenant Portal][azurestack-tenantportal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

> [!KNOWLEDGE] Notice that background color has been changed to Blue from Grey, meaning you are logged on as a normal tenant, you do not have administrative rights. Most of the items are missing like **Marketplace Management**, **Plans**, **Offers** 

- [] On Main menu click **+New** and examine new options as you’ve done on admin portal. 

When you try to create any of the items notice that it’s asked you to **Sign up for a new Subscription**.

> [!KNOWLEDGE] Within the following exercise we will continue to setup management environment as a stepping stone to register Azure Stack to Azure Active Directory 

===

#Lab 01 - Exercise #4

##Install Azure Stack PowerShell

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Run following PowerShell script as an **Administrator**

```PowerShell
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted
  
# Install the AzureRM.Bootstrapper module. If prompted, Select Yes to install NuGet
Install-Module `
  -Name AzureRm.BootStrapper
  
# Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
Use-AzureRmProfile `
  -Profile 2017-03-09-profile -Force
  
Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.11
```  

> [!KNOWLEDGE] Within the following exercise we will continue to setup management environment as a stepping stone to register Azure Stack to Azure Active Directory
[Reference Document][lab01-e4-rl1]

##Download Azure Stack tools and extract

> [!KNOWLEDGE] AzureStack-Tools is a GitHub repository that hosts PowerShell modules for managing and deploying resources to Azure Stack 

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Run following PowerShell script as an **Administrator**

```PowerShell
# Variables 
$defaultLocalPath = "C:\AzureStackOnAzureVM"
  
# Change directory to the AzureStackonAzureVM directory.
cd \
cd $defaultLocalPath
  
# Download the tools archive.
Invoke-WebRequest https://github.com/Azure/AzureStack-Tools/archive/master.zip -OutFile master.zip
  
# Expand the downloaded files.
expand-archive master.zip -DestinationPath . -Force
```
[Reference Document][lab01-e4-rl2]

##Configure PowerShell - Cloud operator

> [!KNOWLEDGE] As an Azure Stack operator, you can configure your Azure Stack Development Kit's PowerShell environment. After you configure, you can use PowerShell to manage Azure Stack resources such as creating offers, plans, quotas, managing alerts, etc. by PowerShell

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and replace **< >** fields between brackets with the values represent your environment.

```PowerShell
# Variables 
$defaultLocalPath = "C:\AzureStackOnAzureVM" 
$AadAdmin = "@lab.UserFirstName@<\tenant_name>.onmicrosoft.com" 
$AadTenant = "<\tenant_name>.onmicrosoft.com"
  
# Create Azure AD credential object 
$AadAdminPass = ConvertTo-SecureString "@lab.VirtualMachine(55267).Password" -AsPlainText -Force 
$AadAdminCred = New-Object System.Management.Automation.PSCredential ("$AadAdmin", $AadAdminPass) 
  
# Change to tools directory. 
cd \ 
cd $defaultLocalPath 
cd AzureStack-Tools-master 
  
# Navigate to the downloaded folder and import the **Connect** PowerShell module 
Import-Module .\Connect\AzureStack.Connect.psm1 
  
# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external.
$ArmEndpoint =  "https://adminmanagement.local.azurestack.external" 
  
# For Azure Stack development kit, this value is adminvault.local.azurestack.external  
$KeyvaultDnsSuffix = “adminvault.local.azurestack.external” 
  
# Register an AzureRM environment that targets your Azure Stack instance 
Add-AzureRMEnvironment ` 
  -Name "AzureStackAdmin" ` 
  -ArmEndpoint $ArmEndpoint 
  
Set-AzureRmEnvironment ` 
  -Name "AzureStackAdmin" ` 
  -GraphAudience $GraphAudience  
  
# Get the Active Directory tenantId that is used to deploy Azure Stack 
$TenantID = Get-AzsDirectoryTenantId ` 
  -AADTenantName $AadTenant ` 
  -EnvironmentName "AzureStackAdmin" 
  
# Sign in to your environment 
Login-AzureRmAccount ` 
  -EnvironmentName "AzureStackAdmin" ` 
  -TenantId $TenantID ` 
  -Credential $AadAdminCred
```
##Test Azure Stack PowerShell

> [!KNOWLEDGE] Now we have configured PowerShell, we can test the configuration by creating a resource group

```PowerShell
New-AzureRMResourceGroup -Name "Test-Resource-Group" -Location Local
```

===

#Lab 01 - Exercise #5
> [!KNOWLEDGE] Registration is recommended because it enables you to test important Azure Stack functionality, like marketplace syndication and usage reporting.
After you register Azure Stack, usage is reported to Azure commerce. You can see it under the subscription you used for registration.
Azure Stack Development Kit users are not charged for any usage they report.

> [!KNOWLEDGE] **CloudAdmin** user account is introduced here has been created as an account on AZURESTACK Active Directory for internal use.
Password of **CloudAdmin** will be as same as @lab.VirtualMachine(55267).Password as have been configured for other users, this is valid only for ASDK deployment.

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and replace **< >** fields between brackets with the values represent your environment.

```PowerShell
# Variables 
$defaultLocalPath = "C:\AzureStackOnAzureVM"
$cloudAdmin = "CloudAdmin"
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"
$AadAdmin = "@lab.UserFirstName@<\tenant name>.onmicrosoft.com"
  
# Create cloudadmin credential object
$CloudAdminPass = ConvertTo-SecureString "@lab.VirtualMachine(55267).Password" -AsPlainText -Force
$CloudAdminCred = New-Object System.Management.Automation.PSCredential ("$domain\$cloudadmin", $CloudAdminPass)
  
# Create Azure AD credential object 
$AadAdminPass = ConvertTo-SecureString "@lab.VirtualMachine(55267).Password" -AsPlainText -Force
$AadAdminCred = New-Object System.Management.Automation.PSCredential ("$AadAdmin", $AadAdminPass)
  
# Change directory to the AzureStackonAzureVM directory.
cd \
cd $defaultLocalPath
  
# Download RegisterWithAzure Module
Start-BitsTransfer -Source "https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Registration/RegisterWithAzure.psm1" -Destination .\RegisterWithAzure.psm1

# Login with Cloud Administrator
Login-AzureRmAccount -Credential $AadAdminCred

# Select subscription
$SubscriptionID = (Get-AzsSubscription).SubscriptionId

Add-AzureRmAccount -EnvironmentName "AzureCloud" -Credential $AadAdminCred
Get-AzureRmSubscription -SubscriptionID $SubscriptionID | Select-AzureRmSubscription
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
Import-Module .\RegisterWithAzure.psm1
$AzureContext = Get-AzureRmContext

Set-AzsRegistration `
    -CloudAdminCredential $CloudAdminCred `
    -PrivilegedEndpoint $privilegedEndpoint `
    -BillingModel Development
```
>   [!KNOWLEDGE] This process takes about 10 minutes to complete

The output will be similar to as follows:
!IMAGE[Lab Image](https://github.com/yagmurs/Wplus-AzS/raw/master/screens/lab01-e5-i1.png)

[Reference Document][lab01-e5-rl]

===

#Lab 02 - Marketplace

##^[**Objectives and Summary**][lab02-os]

> [lab02-os]:
> ###Introduction
>> This lab is about downloading Azure Market Place items to use on Azure Stack. **In Connected** mode it is possible to download and use Azure Market Place items.
>> In disconnected mode we can use offline download tools to prepare items to be used.
>
> ###Objectives
> In this lab, you will:
> - Download Azure Market Place items using Azure Stack GUI.
> - Download Azure Market Place items using offline download tools.
> ###Prerequisites
>>> [!ALERT] In order to successfuly complete this lab **Lab 01** must be completed.
>
> ###Variables
>> Azure Stack Administrator password, Azure Active Directory Identity will be used to logon Azure and Azure Stack portals.
>>
>>  **@lab.VirtualMachine(55267).Username**
>> 
>>
>> Following scheme stands for password you have assigned to **Administrator**. Same password is used for *AZURESTACK\AzureStackAdmin*, and *AZURESTACK\CloudAdmin* which will be used on Azure Stack development kit. These are AZURESTACK domain identities that may be used to logon Azure VM.
>>
>>  **@lab.VirtualMachine(55267).Username**
>> 
>
> ###Estimated time to complete this lab
>> **30 minutes**
>
> ###Scenario
>> Download several Azure Market Place items to use on installing Resource providers.

===
#Lab 02 - Exercise #1 - Download an Azure (Stack) Marketplace Items using Management Portal

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Click **More Services** > **Marketplace Management** > **+Add from Azure**

    !IMAGE[Lab Image](https://github.com/yagmurs/Wplus-AzS/raw/master/screens/lab02-e1-i1.png)

- [] Search and Select following items one by one from Marketplace

> - Custom Script Extension
> - PowerShell Desired State Configuration
> - Windows Server 2016 Datacenter - Server Core
> - Windows Server 2016 Datacenter
> - SQL Server 2016 SP1 Enterprise on Windows Server 2016

- [] On **Add from Azure** blade click on the item

    !IMAGE[Lab Image](https://github.com/yagmurs/Wplus-AzS/raw/master/screens/lab02-e1-i2.png)

- [] On each item's blade click **Download**

    !IMAGE[Lab Image](https://github.com/yagmurs/Wplus-AzS/raw/master/screens/lab02-e1-i3.png)


>   [!KNOWLEDGE] Download process for all items will take about 1 hour to complete

[Reference Document][lab02-e1-rl1]

[Reference Document][lab02-e1-rl2]

#Lab 03 - Configuring Delegation using the Azure Stack Admin Portal

##^[**Objectives and Summary**][lab03-os]

> [lab03-os]:
> ###Introduction
>> After completing this lab, students will be able to: Implement Azure Stack offer delegation by using the Azure Stack portals
>
> ###Objectives
> In this lab, you will:
> - Create a plan 
> - Create an offer
> - Subscribe to an offer
> - Delegate offers in Azure Stack
>
> ###Prerequisites
>>> [!ALERT] In order to successfuly complete this lab **Lab 01 & Lab 02** must be completed.
>
> ###Variables
>> Azure Stack Administrator password, Azure Active Directory Identity will be used to logon Azure and Azure Stack portals.
>>
>>  **@lab.VirtualMachine(55267).Username**
>> 
>>
>> Following scheme stands for password you have assigned to **Administrator**. Same password is used for *AZURESTACK\AzureStackAdmin*, and *AZURESTACK\CloudAdmin* which will be used on Azure Stack development kit. These are AZURESTACK domain identities that may be used to logon Azure VM.
>>
>>  **@lab.VirtualMachine(55267).Username**
>> 
>
> ###Estimated time to complete this lab
>> **60 minutes**
>
> ###Scenario
>> Implement Azure Stack offer delegation by using the Azure Stack portals

===
#Lab 03 - Exercise #1 - Prepare for the lab

> In this exercise, you will create Azure Active Directory user accounts that you will be using in this lab:
> - Create a delegated admin user account (as a cloud operator)
> - Create a tenant user account-tenantuser (as a cloud operator)

##Task 1: Create a delegated admin user account - delegatedprovider (as a cloud operator)

> In this task, you will:
> - Create a delegated admin user account (as a cloud operator)

- [] Logon Azure Stack host VM with RDP client using following information;
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser **(InPrivate/Incognito)**

- [] Logon [Azure Portal][azure-portal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] In the Azure portal, click **More services**
- [] On New blade, search for **Azure Active Directory**, click **Azure Active Directory**
- [] On **Azure Active Directory** blade, under manage, click **Users and groups**
- [] On the **Users and groups** blade, under manage, click **All users**
- [] On the **Users and groups – All Users**, click **+New user**
- [] In the **User** blade, specify the following settings
   
    > - Name: **DelegatedProvider**
    > - User name: **delegatedprovider@<\tenant_name>.onmicrosoft.com**
    > - Profile: **Not configured**
    > - Properties: **Default**
    > - Groups: **0 groups selected**
    > - Directory role: **User**

- [] Click **Show Password** check box, note the password
- [] Click **Create** to create the user account.

##Task 2: Create a tenant user account - tenantuser (as a cloud operator)

> In this task, you will:
> - Create a tenant user account-tenantuser (as a cloud operator)


- [] While logged on Azure Stack host VM with RDP client using following information;
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser **(InPrivate/Incognito)**

- [] Logon [Azure Portal][azure-portal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] In the Azure portal, click **More services**
- [] On New blade, search for **Azure Active Directory**, click **Azure Active Directory**
- [] On **Azure Active Directory** blade, under manage, click **Users and groups**
- [] On the **Users and groups** blade, under manage, click **All users**
- [] On the **Users and groups – All Users**, click **+New user**
- [] In the **User** blade, specify the following settings
   
    > - Name: **TenantUser**
    > - User name: **tenantuser@<\tenant_name>.onmicrosoft.com**
    > - Profile: **Not configured**
    > - Properties: **Default**
    > - Groups: **0 groups selected**
    > - Directory role: **User**

- [] Click **Show Password** check box, note the password
- [] Click **Create** to create the user account.

> **Result:**
> After completing this exercise, you have created the Active Directory accounts you will use in this lab

#Lab 03 - Exercise #2 - Designate the delegated provider (as a cloud operator)

> In this exercise, you will act as a cloud operator and create a plan consisting of the Subscription service and the corresponding offer.
> Next, you will create a subscription containing the new offer and designate the delegated provider as its subscriber:
> - Create a plan consisting of the Subscription service (as a cloud operator)
> - Create an offer based on the plan (as a cloud operator)
> - Create a new subscription containing the offer (as a delegated provider)

##Task 1: Create a plan consisting of the Subscription service (as a cloud operator)

> In this task, you will:
> - Create a plan consisting of the Subscription service (as a cloud operator)

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] On the **New** blade, click **Offers + Plans**
- [] On the **Offers + Plans** blade, click **Plan**
- [] On the **New plan** blade, specify the following information:

    > - Display name: **cotodp-subscription-plan**
    > - Resource name: **cotodp-subscription-plan**
    > - Resource group: click **Create new** and type **dp-subscription-rg**

- [] Click **Services**
- [] On the **Services** blade, click **Microsoft.Subscriptions** and click **Select**
- [] Click **Quotas**
- [] On the **Quotas** blade, click **Microsoft.Subscriptions**
- [] On the **Quotas** blade, click **delegatedProviderQuota**
- [] Click **OK**
- [] Click **Create**

> [!ALERT] Wait for the deployment to complete. This should take just a few seconds.

##Task 2: Create an offer based on the plan (as a cloud operator)

> In this task, you will:
> - Create an offer based on the plan (as a cloud operator)

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **New**
- [] On the **New** blade, click **Offers + Plans**
- [] On the **Offers + Plans** blade, click **Offer**
- [] On the **New offer** blade, specify the following information:

    > - Display name: **Cotodp-subscription-offer**
    > - Resource name: **cotodp-subscription-offer**
    > - Provider subscription: **Default Provider Subscription**
    > - Resource group: **CO-subscription-rg**

- [] Click **Base plans**
- [] On the **Plan** blade, click **cotodp-subscription-plan**
- [] Click **Select**.
- [] Leave **Add-on plans** set to **None** and click **Create**

> [!ALERT] Wait for the deployment to complete. This should take just a few seconds.

##Task 3: Create a new subscription containing the offer and add the delegated provider as its subscriber (as a cloud operator)
> In this task, you will:
> - Create a new subscription containing the offer and add the delegated provider as its subscriber (as a cloud operator)

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **New**
- [] On the **New** blade, click **Offers + Plans**
- [] On the **Offers + Plans** blade, click **Subscription**.
- [] On the **New user subscription** blade, specify the following information:

    > - Display name: **DelegatedProvider-subscription**
    > - User: **delegatedprovider@<\tenant_name>.onmicrosoft.com**
    > - Provider subscription: **Default Provider Subscription**
    > - Directory tenant: **<\tenant_name>.onmicrosoft.com**

- [] Click **Offer**
- [] On the **Offers** blade, click **cotodp-subscription-offer**
- [] Click **Create**

> [!ALERT] Wait for the deployment to complete. This should take just a few seconds.

> **Result:**
> After completing this exercise, you have designated the delegated provider

#Lab 03 - Exercise #3 - Create and delegate a delegated offer to a delegated provider (as a cloud operator)
> In this exercise, you will act as a cloud operator and create an offer containing services, which the delegated provider will then offer to its tenants:
> - Create a plan to delegate (as a cloud operator)
> - Create an offer based on the plan (as a cloud operator)
> - Delegate the offer to the delegated provider (as a cloud operator)

##Task 1: Create a plan to delegate (as a cloud operator)
> In this task, you will:
> - Create a plan to delegate (as a cloud operator)

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **New**
- [] On the **New** blade, click **Offers + Plans**
- [] On the **Offers + Plans** blade, click **Plan**
- [] On the **New plan** blade, specify the following information:

    > - Display name: **co-services-plan**
    > - Resource name: **co-services-plan**
    > - Resource group: click **Create new** and type **CO-services-rg**

- [] Click **Services**
- [] On the **Services** blade, click **Microsoft.Storage**
- [] Click **Select**
- [] Click **Quotas**
- [] On the **Quotas** blade, click **Microsoft.Storage (local)**
- [] On the **Quotas** blade, click **Create new quota**
- [] On the **Create quota** blade, specify the following settings:

    > - Name: **CO-services-Storage-quota**
    > - Maximum capacity (GB): **50**
    > - Total number of storage accounts: **1**

- [] Click **OK**
- [] Click **CO-services-Storage-quota**
- [] Click **OK**
- [] Click **Create**

> [!ALERT] Wait for the deployment to complete. This should take just a few seconds.

##Task 2: Create an offer based on the plan (as a cloud operator)
> In this task, you will:
> - Create an offer based on the plan (as a cloud operator)

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **New**
- [] On the **New** blade, click **Offers + Plans**
- [] On the **Offers + Plans** blade, click **Offer**
- [] On the **New offer** blade, specify the following information:

    > - Display name: **Cotodp-services-offer**
    > - Resource name: **cotodp-services-offer**
    > - Provider subscription: **Default Provider Subscription**
    > - Resource group: **CO-services-rg**

- [] Click **Base plans**
- [] On the **Plan** blade, click **CO-services-plan**
- [] Click **Select**
- [] Leave **Add-on plans** set to **None** and click **Create**

> [!ALERT] Wait for the deployment to complete. This should take just a few seconds.

##Task 3: Delegate the offer to a delegated provider (as a cloud operator)

> In this task, you will:
> - Delegate the offer to the delegated provider (as a cloud operator)

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **Offers**
- [] On the **Offers** blade, click **cotodpservices-offer**
- [] On the **Cotodp-services-offer** blade, click **Delegated providers**
- [] Click **+Add**
- [] On the **Delegate offer** blade, specify the following:

    > - Name: **cotodp-services-offer**
    > - Pick the delegated provider subscription: **delegatedprovider-subscription**

- [] Click **Delegate**

> [!ALERT] Wait for the deployment to complete. This should take just a few seconds.

> **Result:**
> After completing this exercise, you have created and delegated the offer to a delegated provider.

#Lab 03 - Exercise #4 - Create a delegated offer to a tenant (as a delegated provider)
> In this exercise, you will act as a delegated provider and create an offer (based on the offer from the cloud operator) to which your tenants will be able to subscribe:
> - Create a delegated provider offer for a tenant (as a cloud operator)
> - Identify the URL of the delegated provider portal (as a delegated provider)

##Task 1: Create a delegated provider offer for a tenant (as a delegated provider)
> In this task, you will:
> - Create an offer for a tenant (as a delegated provider) based on the cloud operator offer for the delegated provider
> This will allow tenants to create a subscription based on the offer from the delegated provider

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser **(InPrivate/Incognito)**

- [] Logon [Azure Stack Tenant Portal][azurestack-tenantportal] using following information;
    
    > - Username: **delegatedprovider@<\tenant_name>.onmicrosoft.com**
    > - Password: use previously noted password for delegatedprovider and change with **@lab.VirtualMachine(55267).Password**

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Offers**
- [] On the "**Offers** blade, click **+Add**
- [] On the **New offer** blade, specify the following settings:

    > - Display name: **dptotenantuser-services-offer**
    > - Resource name: **dptotenantuser-services-offer**
    > - Provider subscription: **delegatedprovider-subscription**
    > - Resource group: create a **new resource group** called **DP-rg**

> [!ALERT] Wait for the deployment to complete. This should take just a few seconds.

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Offers**
- [] Click the newly created offer **dptotenantuser-services-offer**
- [] On the **dptotenantuser-services-offer** blade, click **Change state** and, in the drop-down list, click **Public**

##Task 2: Identify the URL of the delegated provider portal (as a delegated provider)
> In this task, you will:
> - Identify the URL of the delegated provider portal (as a delegated provider)

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser **(InPrivate/Incognito)**

- [] Logon [Azure Stack Tenant Portal][azurestack-tenantportal] using following information;
    
    > - Username: **delegatedprovider@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **More services**
- [] In the list of services, click **Subscriptions**
- [] Click **delegatedprovider-subscription**
- [] On the delegated provider subscription blade, click **Properties**
- [] On the properties blade, copy the value of the **PORTAL URL** to clipboard. You will need it in the next exercise of this lab,
- [] **Sign out** from the [Azure Stack Tenant Portal][azurestack-tenantportal]

> [!KNOWLEDGE]Tenants need to subscribe to the offer from the delegated provider portal URL

> **Result:**
> After completing this exercise, you have created a delegated provider offer

#Lab 03 - Exercise #5 - Sign up for a delegated provider’s offer (as a tenant user-tenantuser)
> In this exercise, you will act as a tenant user who signs up for a delegated provider’s offer and creates a resource in the new subscription. The exercise consists of the following tasks:
> - Sign up for the delegated provider’s offer (as a tenant user-tenantuser)
> - Create a resource in the new subscription (as a tenant user-tenantuser)

##Task 1: Sign up for the delegated provider’s offer (as a tenant user-tenantuser)
> In this task, you will:
> - Sign up for the delegated offer (as a tenant user-tenantuser)

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser **(InPrivate/Incognito)**

- [] Logon [Azure Stack Tenant Portal][azurestack-tenantportal] using following information;
    
    > - Username: **tenantuser@<\tenant_name>.onmicrosoft.com**
    > - Password: use previously noted password for tenantuser and change with **@lab.VirtualMachine(55267).Password**

- [] Once you logged on with **tenantuser@<\tenant_name>.onmicrosoft.com** navigate to the Azure Stack delegated provider portal by using the **URL identified** in task 2 of the **previous exercise**

> [!ALERT] Make sure NOT to use the Azure tenant portal **URL** in this case

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Get a Subscription** on the dashboard
- [] On the **Get a subscription** blade, in the **Display name** text box, type **Tenantuser-subscription**
- [] Click **Select an offer**
- [] On the **Choose an offer** blade, click **Dptotenantuser-services-offer**
- [] Click **Create**
- [] In the message box **Your subscription has been created. You must refresh the portal to start using your subscription**, click **Refresh**


##Task 2: Create a resource in the new subscription (as a tenant user-tenantuser)    
> In this task, you will:
> - Create a resource in the new subscription (as a tenant user-tenantuser)

- [] Back in the [Azure Stack Tenant Portal][azurestack-tenantportal], click **More services**
- [] In the list of services, click **Subscriptions**
- [] On the **Subscriptions** blade, click **tenantuser-subscription**
- [] Click **Resources**
- [] Click **+Add**
- [] On the **New** blade, click **Data + Storage**
- [] On the **Storage** blade, click **Storage account**
- [] On the **Create storage account** blade, specify the following settings and click

    > - Name: any unique name consisting of between 3 and 24 lower case letters or digits
    > - Account kind: **General purpose**
    > - Performance: **Standard**
    > - Replication: **Locally-redundant storage (LRS)**
    > - Subscription: **tenantuser-subscription**
    > - Resource group: create a new group **delegatedlab-RG**
    > - Location: **local**

> [!ALERT] Wait for the deployment to complete. This should take less than a minute

- [] Verify that the storage account has been successfully created.
- [] Repeat above steps to **Create storage account**. Note that this time you will receive an error message due to exceeded quotas.

> **Result:**
> After completing this exercise, you have subscribed to a delegated provider’s offer, creating this way a new subscription and provisioned a storage account in the new subscription.

===

#Lab 04 - Working with ARM Templates

##^[**Objectives and Summary**][lab04-os]

> [lab04-os]:
> ###Introduction
>> In this lab we are going to deploy Standalone file server using an ARM template
>
> ###Objectives
> In this lab, you will:
> - Create a standalone file server VM 
> - Have a general view of ARM template structure
> ###Prerequisites
>>> [!ALERT] In order to successfuly complete this lab **Lab 01**, and **Lab 02** must be completed.
>
> ###Variables
>> Azure Stack Administrator password, Azure Active Directory Identity will be used to logon Azure and Azure Stack portals.
>>
>>  **@lab.VirtualMachine(55267).Username**
>> 
>>
>> Following scheme stands for password you have assigned to **Administrator**. Same password is used for *AZURESTACK\AzureStackAdmin*, and *AZURESTACK\CloudAdmin* which will be used on Azure Stack development kit. These are AZURESTACK domain identities that may be used to logon Azure VM.
>>
>>  **@lab.VirtualMachine(55267).Username**
>> 
>
> ###Estimated time to complete this lab
>> **20 minutes**
>
> ###Scenario
>> Deploy single file server to be used on App Service installation,

===
#Lab 04 - Exercise #1 - Deploy Templates using the Portal

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Click **+New** > **Custom** > **Template deployment**
- [] On Custom template blade click  **Template / Edit template** > **QuickStart template**
- [] On Edit template blade click  **Quick template** > **QuickStart template**

    !IMAGE[Lab Image](https://github.com/yagmurs/Wplus-AzS/raw/master/screens/lab04-e1-i1.png)

- [] Click **select a template (disclaimer)**. Review wide range of the templates.
- [] On Load a quick template section in select a template (disclaimer) combobox select **appservice-fileserver-standalone**

    !IMAGE[Lab Image](https://github.com/yagmurs/Wplus-AzS/raw/master/screens/lab04-e1-i2.png)

- [] Click **OK** to load template

- [] On **Edit template** blade click, examine the template and click **Save**

- [] On Custom template blade click **Edit parameters**

- [] On **Parameters** blade, specify the following settings
> - FILESERVERVIRTUALMACHINESIZE: **Standard_A1**
> - IMAGEREFERENCE (string): leave default settings
> - IMAGEREFERENCE (string): leave default settings
> - ADMINUSERNAME (string): **fileshareowner**
> - ADMINPASSWORD (securestring): **@lab.VirtualMachine(55267).Password**
> - FILESHAREOWNER (string): **fileshareowner**
> - FILESHAREOWNERPASSWORD (securestring): **@lab.VirtualMachine(55267).Password**
> - FILESHAREUSER (string): **fileshareuser**
> - FILESHAREUSERPASSWORD (securestring): **@lab.VirtualMachine(55267).Password**
> - VMEXTENSIONSCRIPTLOCATION (string): leave default settings

!IMAGE[Lab Image](https://github.com/yagmurs/Wplus-AzS/raw/master/screens/lab04-e1-i3.png)

- [] Click **OK**
- [] On Custom deployment blade set Resource Group name as **appservicefilesharerg** select **create new** radio button, and click **Create**

> [!KNOWLEDGE] This template is being downloaded from Azure Stack Quickstart templates gallery on **github.com**

> [!KNOWLEDGE] Template may be called as in following format. Inside the URI which is called there is a redirection to the address below displayed
>
> https://adminportal.local.azurestack.external/#create/Microsoft.Template/uri/**https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAzureStack-QuickStart-Templates%2Fmaster%2Fappservice-fileserver-standalone%2Fazuredeploy.json**

===

#Lab 04 - Exercise #2 - Deploy Templates with PowerShell

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser

- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
    
    > - Username: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open another browser or a browser tab
- [] Navigate [https://github.com/Azure/AzureStack-QuickStart-Templates][github-azurestackquicktemplates]
- [] Click **101-create-storage-account** and click 
- [] Click **azuredeploy.json** and click **Raw** button on the new page
- [] Copy link on the address bar to **clipboard**
- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and replace **< >** fields between brackets with the values represent your environment.

```PowerShell
# Variables
$defaultLocalPath = "C:\AzureStackOnAzureVM"
$AadAdmin = "@lab.UserFirstName@<\tenant_name>.onmicrosoft.com"
$AadTenant = "<\tenant_name>.onmicrosoft.com"
$RGName = "AzsStorageAccountRG"
$myLocation = "local"

# Create Azure AD credential object
$AadAdminPass = ConvertTo-SecureString "@lab.VirtualMachine(55267).Password" -AsPlainText -Force
$AadAdminCred = New-Object System.Management.Automation.PSCredential ("$AadAdmin", $AadAdminPass)

# Navigate to the downloaded folder and import the **Connect** PowerShell module
Import-Module $defaultLocalPath\AzureStack-Tools-master\Connect\AzureStack.Connect.psm1
 
# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external.
$ArmEndpoint =  "https://adminmanagement.local.azurestack.external"
 
# For Azure Stack development kit, this value is adminvault.local.azurestack.external
$KeyvaultDnsSuffix = “adminvault.local.azurestack.external”
 
# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRMEnvironment `
  -Name "AzureStackAdmin" `
  -ArmEndpoint $ArmEndpoint
 
Set-AzureRmEnvironment `
  -Name "AzureStackAdmin" `
  -GraphAudience $GraphAudience

# Get the Active Directory tenantId that is used to deploy Azure Stack
$TenantID = Get-AzsDirectoryTenantId `
  -AADTenantName $AadTenant `
  -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
Login-AzureRmAccount `
  -EnvironmentName "AzureStackAdmin" `
  -TenantId $TenantID `
  -Credential $AadAdminCred

# Create Resource Group for Template Deployment
New-AzureRmResourceGroup -Name $RGName -Location $myLocation

# Create Storage Group refering Uri on Azure Stack Quickstart gallery
New-AzureRmResourceGroupDeployment `
-ResourceGroupName $RGName `
-TemplateUri <\link on clipboard> `
-StorageaccountType Standard_LRS

```

>  [!NOTE] Clipboard link: 
>>
>>   ++https://raw.githubusercontent.com/Azure/AzureStack-QuickStart-Templates/master/101-create-storage-account/azuredeploy.json++

- [] On Main menu click **Resource Groups** and examine new options as you’ve done on admin portal.
- [] On Resource groups blade, click **AzsStorageAccountRG** to check if there is a Storage account with random name in **AzsStorageAccountRG** blade

===

#Lab 05 - Deploy Azure VM to host Azure Stack Development Kit (ASDK) in connected mode

##^[**Objectives and Summary**][lab05-os]

> [lab05-os]:
> ###Introduction
>> This Lab covers enabling, configuring, and manually scaling Virtual Machine scale sets to be used in Azure Stack
>
> ###Objectives
> In this lab, you will:
> - Enable Virtual Machine scale sets feature
> - Connect to Azure Stack as a tenant
> - Deploy a Virtual Machine scale set
> - Manually scale up Virtual Machine scale set
>
> ###Prerequisites
>>> [!ALERT] In order to successfuly complete this lab **Lab 01**, **Lab 02**, and **Lab 03** must be completed.
>
> ###Variables
>> Azure Stack Administrator password, Azure Active Directory Identity will be used to logon Azure and Azure Stack portals.
>>
>>  **@lab.VirtualMachine(55267).Username**
>> 
>>
>> Following scheme stands for password you have assigned to **Administrator**. Same password is used for *AZURESTACK\AzureStackAdmin*, and *AZURESTACK\CloudAdmin* which will be used on Azure Stack development kit. These are AZURESTACK domain identities that may be used to logon Azure VM.
>>
>>  **@lab.VirtualMachine(55267).Username**
>> 
>
> ###Estimated time to complete this lab
>> **60 Minutes**
>
> ###Scenario
>> Enable Virtual Machine Scale Set feature in Azure Stack using PowerShell via Cloud Operator account.
Afterwards logon to your tenant and create a virtual machine scale set and then manually scale it

===

#Lab 05 - Exercise #1 - Enabling VM scale set on Azure Stack
> In this exercise, you will act as a cloud operator and enable Virtual Machine scale sets feature on Azure Stack:
> - Enable Virtual Machine scale sets feature on Azure Stack as a cloud operator
> - Publish VM scale set marketplace item

##Task 1: Enable Virtual Machine Scale Sets
> In this task, you will:
> - Enable Virtual Machine scale sets feature on Azure Stack
> - Publish VM scale set marketplace item

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and replace **< >** fields between brackets with the values represent your environment.

> [!KNOWLEDGE] This will enable Virtual Machine scale sets on your Azure Stack deployment

```PowerShell
#Variables
$defaultLocalPath = "C:\AzureStackOnAzureVM"
$location = "local"

$AadAdmin = "@lab.UserFirstName@<\tenant_name>.onmicrosoft.com"
$AadTenant = "<\tenant_name>.onmicrosoft.com"
 
# Create Azure AD credential object
$AadAdminPass = ConvertTo-SecureString "@lab.VirtualMachine(55267).Password" -AsPlainText -Force
$AadAdminCred = New-Object System.Management.Automation.PSCredential ("$AadAdmin", $AadAdminPass)
 
cd\
cd $defaultLocalPath
cd AzureStack-Tools-master

Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1
Import-Module .\Connect\AzureStack.Connect.psm1

$ArmEndpoint = "https://adminmanagement.local.azurestack.external"
$KeyvaultDnsSuffix = “adminvault.local.azurestack.external”
 
# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint $ArmEndpoint

Set-AzureRmEnvironment -Name "AzureStackAdmin" -GraphAudience $GraphAudience 

# Get the Active Directory tenantId that is used to deploy Azure Stack
$TenantID = Get-AzsDirectoryTenantId -AADTenantName $AadTenant -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
Login-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID -Credential $AadAdminCred

Add-AzsVMSSGalleryItem -Location $location
```

> **Result:**
> After completing this exercise, you have enabled Virtual Machine scale sets on your Azure Stack deployment

#Lab 05 - Exercise #2 - Deploy a virtual machine scale set (VMSS) using Azure Stack tenant portal
> In this exercise, you will act as a tenant and deploy Virtual Machine Scale Set:
> - Deploy a Virtual Machine scale set

##Task 1: Deploy a virtual machine scale set
> In this task, you will:
> - Deploy a Virtual Machine scale set

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Within Azure Stack host VM (Azs-Host1), open a browser **(InPrivate/Incognito)**

- [] Logon [Azure Stack Tenant Portal][azurestack-tenantportal] using following information;
    
    > - Username: **tenantuser@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **New**
- [] On the **New** blade, click **Compute**
- [] On the **Compute** blade, click **Virtual Machine scale set**
- [] On the **Create Virtual machine scale set** blade, **STEP 1 - Basics** specify the following information:

    > - Virtual machine scale set name: rg01vmss
    > - OS type: select **Windows**
    > - User name: **localadm**
    > - Password: **@lab.VirtualMachine(55267).Password** twice
    > - Subscription: **<\Subscription>**
    > - Resource group: select **Create new**
    > - Resource group name: **rg01**
    > - Location: **local**

- [] Click **OK**
    
- [] On the **Create Virtual machine scale set** blade, **STEP 2 - Virtual machine scale set service settings**:
- [] Click on **Public IP address**, specify the following information:

    > - Name: **rg01ip01**
    > - Assignment: **Dynamic**

- [] Click **OK**

- [] Back in the **Create Virtual machine scale set** blade, **STEP 2 - Virtual machine scale set service settings** specify the following information:

    > - Domain name label: **testapp01**
    > - Operating system disk image: **2016-Datacenter**
    > - Instance count: **1**

- [] Click **OK**
 
- [] On the **Create Virtual machine scale set** blade, **STEP 3 - Summary**, Make sure all information is correct:
- [] Click **OK**

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Resource Groups**
- [] On the **Resource groups** blade, click **rg01**
- [] On the **rg01 - Deployments** blade, click on **Deployments**

> [!ALERT] Wait for deployment to finish, this will take approximately 15 minutes

##Task 2: Validate virtual machine scale set deployment

> In this task, you will:
> - Validate Virtual Machine scale set deployment

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Open **Start Menu**
- [] Search for **Remote Desktop Connection**
- [] In the **Remote Desktop Connection**, connect **++testapp01.local.cloudapp.azurestack.external:50000++** using following information;

    > - Username: **\localadm**
    > - Password: **@lab.VirtualMachine(55267).Password**

> **Result:**
> After completing this exercise, you have deployed Virtual Machine scale sets using tenant subscription

#Lab 05 - Exercise #3 - Manually scale up VMs using PowerShell
> In this exercise, you will act as a tenant and manually scale your Virtual Machine Scale Set:
> - Configure PowerShell to access tenant subscription
> - Manually scale a Virtual Machine scale set

##Task 1: Configure PowerShell Environment for Tenant
> In this task, you will:
> - Configure PowerShell to access tenant subscription

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and replace **< >** fields between brackets with the values represent your environment.

> [!KNOWLEDGE] This will set up tenant PowerShell environment and logon using Powershell

```PowerShell
# Variables
$defaultLocalPath = "C:\AzureStackOnAzureVM"
$Tenant = "tenantuser@<\tenant_name>.onmicrosoft.com"
$AadTenant = <\tenant_name>.onmicrosoft.com
$location = "local"

$TenantPass = ConvertTo-SecureString "@lab.VirtualMachine(55267).Password" -AsPlainText -Force 
$TenantCred = New-Object System.Management.Automation.PSCredential ("$Tenant", $TenantPass) 

# Change to tools directory. 
cd\
cd $defaultLocalPath
cd AzureStack-Tools-master

# Navigate to the downloaded folder and import the **Connect** PowerShell module 
Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1
Import-Module .\Connect\AzureStack.Connect.psm1

$ArmEndpoint =  "https://management.local.azurestack.external"
$KeyvaultDnsSuffix = “vault.local.azurestack.external”

# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRMEnvironment ` 
  -Name "AzureStackTenant" ` 
  -ArmEndpoint $ArmEndpoint 

Set-AzureRmEnvironment `
  -Name "AzureStackTenant" `
  -GraphAudience $GraphAudience

# Get the Active Directory tenantId
$TenantID = Get-AzsDirectoryTenantId -AADTenantName $AadTenant -EnvironmentName "AzureStackTenant"

# Sign in to your environment 
Login-AzureRmAccount ` 
  -EnvironmentName "AzureStackTenant" `
  -TenantId $TenantID `
  -Credential $TenantCred
```


##Task 2: Manually scale up VMs using PowerShell
> In this task, you will:
> - Manually scale a Virtual Machine scale set

- [] Logon Azure Stack host VM with RDP client using following information;
    
    > - Username: **AZURESTACK\AzureStackAdmin**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and replace **< >** fields between brackets with the values represent your environment.

> [!KNOWLEDGE] This will scale up Virtual Machine scale set called **rg01** instance count from **1** to **2**

```PowerShell
$vmss = Get-AzureRmVmss -ResourceGroupName rg01
$vmss = Get-AzureRmVmss -ResourceGroupName rg01 -VMScaleSetName $vmss[0].Name

$vmss.sku.Capacity = 2
Update-AzureRmVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss
```

- [] Within Azure Stack host VM (Azs-Host1), open a browser **(InPrivate/Incognito)**

- [] Logon [Azure Stack Tenant Portal][azurestack-tenantportal] using following information;
    
    > - Username: **tenantuser@<\tenant_name>.onmicrosoft.com**
    > - Password: **@lab.VirtualMachine(55267).Password**

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Resource Groups**
- [] On the **Resource groups** blade, click **rg01**
- [] On the **rg01 - Deployments** blade, click on **Deployments**
- [] Select **Virtual Machine Scale Set** type from the list of resources
- [] On the **rg01vmms** blade, click **Instances**
- [] On the **rg01vmms** blade, click **Instances**

> [!KNOWLEDGE] You should see 1 instance running and another one being created.

> [!ALERT] Wait for the deployment to complete. This should take about 15 minutes

##Task 3: Validate Virtual Machine Scale set
> In this task, you will:
> - Validate the result of sclaing up Virtual Machine Scale set on previous task


- [] To validate the VM you may open a Remote Desktop Connection.

> [!KNOWLEDGE] Because of the default load balancer rules, port 50001 on the Load Balancer will be mapped to the second VM in the scale set on port 3389.

- [] Open **Start Menu**
- [] Search for **Remote Desktop Connection**
- [] In the **Remote Desktop Connection**, connect **++testapp01.local.cloudapp.azurestack.external:50001++** using following information;

    > - Username: **\localadm**
    > - Password: **@lab.VirtualMachine(55267).Password**

> **Result:**
> After completing this exercise, you have connected to your tenant subscription using PowersShell, manually scaled up Virtual Machine scale set, and validated the connectivity to all intances

[Reference Document][labxx-e1-rl]

#References
[lab01-e1-rl]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-deploy
[lab01-e2-rl]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-deploy
[lab01-e4-rl1]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-powershell-install 
[lab01-e4-rl2]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-powershell-download
[lab01-e4-rl3]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-powershell-configure-admin
[lab01-e5-rl]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-register
[lab02-e1-rl1]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-marketplace-azure-items
[lab02-e1-rl2]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-add-default-image

===

#### Links

[Azure Portal][azure-portal]

[Azure Stack Admin Portal][azurestack-adminportal]

[Azure Stack Tenant Portal][azurestack-tenantportal]

[Register Azure Pass][azure-pass]

[azure-portal]:https://portal.azure.com "https://portal.azure.com/"
[azurestack-adminportal]:https://adminportal.local.azurestack.external "https://adminportal.local.azurestack.external"
[azurestack-tenantportal]:https://portal.local.azurestack.external "https://portal.local.azurestack.external"
[azure-pass]:https://www.microsoftazurepass.com/SubmitPromoCode "https://www.microsoftazurepass.com/"
[rl1]:https://signup.live.com/?wa=wsignin1.0&rpsnv=13&ct=1518075390&rver=6.7.6643.0&wp=MBI_SSL&wreply=https:%2f%2faccount.microsoft.com%2fauth%2fcomplete-signin%3fru%3dhttps%253a%252f%252faccount.microsoft.com%252f%253frefp%253dsignedout-index%2526refd%253dwww.google.com.tr&id=292666&lw=1&fl=easi2&contextid=0DED367F3FADAAB6&bk=1518075545&uiflavor=web&uaid=2c5d00f6f13e46c0aab951ad21af64d8&mkt=EN-US&lc=1033 "Create one"
[github-repo]:https://github.com/yagmurs/AzureStack-VM-PoC "https://github.com/yagmurs/AzureStack-VM-PoC"
[github-repo-deploytoazure]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fyagmurs%2FAzureStack-VM-PoC%2Fmaster%2Fazuredeploy.json "Deploy to Azure"
[github-azurestackquicktemplates]:https://github.com/Azure/AzureStack-QuickStart-Templates "https://github.com/Azure/AzureStack-QuickStart-Templates"

===

#Lab xx

##^[**Objectives and Summary**][labxx-os]

> [labxx-os]:
###Introduction
###Objectives
###Prerequisites
###Variables
###Estimated time to complete this lab
###Scenario

===

#Lab xx - Exercise #1

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

[Reference Document][labxx-e1-rl]

===

#Lab xx - Exercise #2

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 


[Reference Document][labxx-e2-rl]

===

#Lab xx - Exercise #3

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 


[Reference Document][labxx-e3-rl]

===

#Lab xx - Exercise #4

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

[Reference Document][labxx-e4-rl]

===

#Lab xx - Exercise #5

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

[Reference Document][labxx-e5-rl]

===

#Lab xx - Exercise #6

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

[Reference Document][labxx-e6-rl]

===

#Lab xx - Exercise #7

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

- [] 

[Reference Document][labxx-e7-rl]

===

##Lab Variables

**@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**

**@lab.VirtualMachine(55267).Password**

@lab.LabInstanceId	The unique ID of the running lab instance.

@lab.GlobalLabInstanceId	The globally unique ID of the running lab instance.

@lab.LabProfileId	The unique ID of the lab profile.

@lab.UserId	The unique ID of user running the lab.

@lab.UserFirstName	The first name of the user running the lab.

@lab.UserLastName	The last name of the user running the lab.

@lab.UserEmail	The e-mail address of the user running the lab.

@lab.UserExternalId	The external ID of the user running the lab (if launched via API).

@lab.Tag	The tag associated with the lab instance (if specified when launched via API).

@lab.VirtualMachine(55059).SelectLink	A link to select the Client01 - Windows 10 1709 virtual machine.

@lab.VirtualMachine(55059).Username	Username for signing into the Client01 - Windows 10 1709 virtual machine.

@lab.VirtualMachine(55059).Password	Password for signing into the Client01 - Windows 10 1709 virtual machine.

===
