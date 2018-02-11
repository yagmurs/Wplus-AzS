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
>>> [!ALERT] Azure Pass account has to be registered and ready to use
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

- [] On Virtual Machines blade > right click on created VM ***AzS-Host1** > click **Connect**

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
  -Credential $AadAdminCre
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

===

#Lab 03 - Working with ARM Templates

##^[**Objectives and Summary**][lab02-os]

> [lab02-os]:
> ###Introduction
>> In this lab we are going to deploy Standalone file server using an ARM template
>
> ###Objectives
> In this lab, you will:
> - •	Create a standalone file server VM 
> - •	Have a general view of ARM template structure
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
>> **20 minutes**
>
> ###Scenario
>> Deploy single file server to be used on App Service installation,

===
#Lab 03 - Exercise #1 - Deploy Templates using the Portal

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

    !IMAGE[Lab Image](https://github.com/yagmurs/Wplus-AzS/raw/master/screens/lab03-e1-i1.png)

- [] Click **select a template (disclaimer)**. Review wide range of the templates.
- [] On Load a quick template section in select a template (disclaimer) combobox select **appservice-fileserver-standalone**

    !IMAGE[Lab Image](https://github.com/yagmurs/Wplus-AzS/raw/master/screens/lab03-e1-i2.png)

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

!IMAGE[Lab Image](https://github.com/yagmurs/Wplus-AzS/raw/master/screens/lab03-e1-i3.png)

- [] Click **OK**
- [] On Custom deployment blade set Resource Group name as **appservicefilesharerg** select **create new** radio button, and click **Create**

> [!KNOWLEDGE] This template is being downloaded from Azure Stack Quickstart templates gallery on **github.com**

> [!KNOWLEDGE] Template may be called as in following format. Inside the URI which is called there is a redirection to the address below displayed
>
> https://adminportal.local.azurestack.external/#create/Microsoft.Template/uri/**https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAzureStack-QuickStart-Templates%2Fmaster%2Fappservice-fileserver-standalone%2Fazuredeploy.json**

===

#Lab 03 - Exercise #2 - Deploy Templates with PowerShell

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
-Name MyStorageAccount `
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
