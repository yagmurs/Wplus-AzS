#WorkshopPLUS – Architecting Hybrid Cloud Solutions – Azure Stack

#Introduction

Microsoft Azure Stack is a hybrid cloud platform that lets you provide Azure services from your datacenter. You'll need an awareness of which services you can make available to your users. Azure Stack supports a subset of Azure services. The list of supported services will continue to evolve.

##Foundational services
By default, Azure Stack includes the following "foundational services" when you deploy Azure Stack:
1. Compute
1. Storage
1. Networking
1. Key Vault

With these foundational services, you can offer Infrastructure-as-a-Service (IaaS) to your users with minimal configuration.

##Additional services
Currently, we support the following additional Platform-as-a-Service (PaaS) services:
1. App Service
1. SQL databases
1. MySQL databases

These services require additional configuration before you can make them available to your users. Throughout this lab you will be deploying Azure Stack Development kit and enable above services acting as Azure Stack Operator role.

Your users want to use services. From their perspective, your main role is to make these services available to them. You must decide which services to offer, and make those services available by creating plans, offers, and quotas.

In short you will learn how to deploy Azure Stack Development Kit, how to manage the infrastructure, and how to offer services. In addition you will learn tools needed to manage and monitor the infrastructure.

Click **Next** to advance to [**Table of content**](#toc) window

===
######toc
#Table of Content
>[**Lab 01**](#lab01)
-   [Exercise 1](#lab01e1)
-   [Exercise 2](#lab01e2)
-   [Exercise 3](#lab01e3)
-   [Exercise 4](#lab01e4)
-   [Exercise 5](#lab01e5)

>[**Lab 02**](#lab02)
-   [Exercise 1](#lab02e1)

>[**Lab 03**](#lab03)
-   [Exercise 1](#lab03e1)
-   [Exercise 2](#lab03e2)
-   [Exercise 3](#lab03e3)
-   [Exercise 4](#lab03e4)
-   [Exercise 5](#lab03e5)

>[**Lab 04**](#lab04)
-   [Exercise 1](#lab04e1)
-   [Exercise 2](#lab04e2)

>[**Lab 05**](#lab05)
-   [Exercise 1](#lab05e1)
-   [Exercise 2](#lab05e2)
-   [Exercise 3](#lab05e3)


>[**Lab 06**](#lab06)
-   [Exercise 1](#lab06e1)
-   [Exercise 2](#lab06e2)

>[**Lab 07**](#lab07)
-   [Exercise 1](#lab07e1)
-   [Exercise 2](#lab07e2)
-   [Exercise 3](#lab07e3)
-   [Exercise 4](#lab07e4)
-   [Exercise 5](#lab07e5)

>[**Lab 08**](#lab08)
-   [Exercise 1](#lab08e1)
-   [Exercise 2](#lab08e2)

>[**Lab 09**](#lab09)
-   [Exercise 1](#lab09e1)
-   [Exercise 2](#lab09e2)
-   [Exercise 3](#lab09e3)

>[**Lab 10**](#lab10)
-   [Exercise 1](#lab10e1)
-   [Exercise 2](#lab10e2)
-   [Exercise 3](#lab10e3)
-   [Exercise 4](#lab10e4)
-   [Exercise 5](#lab10e5)

---

Click **Next** to advance to [**LAB 01**](#lab01) window

---

===
######lab01

[**Table of content**](#toc)

---

#LAB 01 - Deploy Azure VM to host Azure Stack Development Kit in connected mode

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
>>!INSTRUCTIONS[][variables]
>
> ###Estimated time to complete this lab
>> **6 Hours**
>
> ###Scenario
>> Deploy Azure Stack Development Kit on an Azure VM and register to Azure on **connected** mode.

---

Click **Next** to advance to [**LAB 01 - Exercise 1**](#lab01e1) window

---
[**Table of content**](#toc)

---

===
######lab01e1

[**Table of content**](#toc)

---

#Lab 01 - Exercise 1 - Deploy Azure Stack host using ARM Template
> In this exercise, you will deploy Azure VM to host Azure Stack using ARM template
> - Create additional admin account to use on following lab exercises
> - Delegate the account on subscription to be able to manage subscription and register Azure Stack on following lab exercises
> - Deploy Azure Stack Host VM to Azure using ARM template

##Task 1: Create additional admin account
> In this task, you will:
> - Create additional admin account to use on following lab exercises

- [] Within **Client 01**, open a browser
- [] Logon @[Azure Portal][azure-portal] using following information;
    
    |||
    |---:|:---|
    |Username:| +++**@lab.UserEmail**+++
    |Password:| **<\Your Password>**

> [!ALERT] If you don't have Microsoft Account, please @[Create a new Microsoft Account][rl1]

- [] @[*Register Azure Pass*][azure-pass]
- [] In the @[Azure Portal][azure-portal], click **More services**, and then **Azure Active Directory**
- [] On **Quick tasks**, click **Add a user**, specify the following settings.

    |||
    |---:|:---|
    | Name:| +++**@lab.UserFirstName**+++|
    | User name:| +++**@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**+++|
    | Profile:|++**Not configured**++|
    | Directory Role:|**Global administrator**|
    | Password:|check **Show Password**|
    
- []**Copy** the password to **clipboard**


- [] Click **Create**

> [!KNOWLEDGE] Tenant user now created, needs to be the **Owner** for the Azure subscription

##Task 2: Delegate the account on subscription
> In this task, you will:
> - Delegate the account on subscription to be able to manage subscription and register Azure Stack on following lab exercises

- [] In the @[Azure Portal][azure-portal], click **More services**, and then **Subscriptions**
- [] On the **Subscriptions** blade, click **Azure Pass** subscription
- [] On the **Azure Pass** blade, click **Access control (IAM)**, and then **+Add**

- [] On the **Add permissions** blade, specify the following settings

    |||
    |---:|:---|
    |Role:| **Owner**|
    |Assign Access to:| **Azure AD user,group or application**|
    |Select:| +++**@lab.UserFirstName**+++|

- [] User should be listed. Click **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com** to add user to Selected members
- [] Click **Save**

- [] Sign out from current session.
- [] Logon @[Azure Portal][azure-portal] using following information;
    
    |||
    |---:|:---|
    |Username:| +++**@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**+++|
    |Password:| Paste **temporary password previously saved**|

- [] You will be prompted to change the password.
- [] Enter previous password and **@lab.VirtualMachine(55267).Password** twice.
- [] Click **OK**

    > [!ALERT] This will make sure that you have changed the temporary password

> [!KNOWLEDGE] Above instructions have configured Azure Active Directory to start Azure Stack Development Kit (ASDK) installation

> [!KNOWLEDGE] Now, we may start deploying Azure VM to host ASDK

##Task 3: Deploy Azure Stack Host VM
> In this task, you will:
> - Deploy Azure Stack Host VM to Azure using ARM template

- [] Within **Client 01**, open a browser
- [] Navigate to @[https://github.com/yagmurs/AzureStack-VM-PoC][github-repo]
- [] Click @[**Deploy to Azure**][github-repo-deploytoazure] button to deploy Azure Stack host template to Azure
- [] Browser will be redirected to your <\tenant_name>.onmicrosoft.com portal with ARM template settings to be completed

- [] On **Custom deployment** blade, specify the following settings

    |||
    |---:|:---|
    |Resource Group:|select **Create new** +++**AzureStackPremierWorkshop**+++|
    |Location:|**<\Any region that is closer to your location>**|
    |Virtual Machine Name:|+++**AzS-Host1**+++|
    |Admin Password:|+++**@lab.VirtualMachine(55267).Password**+++|
    |Public DNS Name:|**<\Any name that does not conflict>**|
    |Auto Shutdown Status:|**Disabled**|

- [] Check, **TERMS AND CONDITIONS** and click **Purchase**

> [!ALERT] Wait for the deployment to complete. This should take about 10 to 20 minutes depends on the region and load.

> **Result:**
> After completing this exercise, you have deployed Azure VM to host Azure Stack using ARM template and created addtional Global Administrator account on Azure Active Directory

---

Click **Next** to advance to [**LAB 01 - Exercise 2**](#lab01e2) window

---

[**Table of content**](#toc)

---

===
######lab01e2

[**Table of content**](#toc)

---

#Lab 01 - Exercise 2 - Deploy Azure Stack Development Kit (ASDK)
> In this exercise, you will install Azure Stack Development Kit

##Task 1: Start Azure Stack Development Kit installation
> In this task, you will:
> - Start installing Azure Stack Development Kit

- [] Logon @[Azure Portal][azure-portal] using following information;
    
    |||
    |---:|:---|
    |Username: |+++**@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**+++|
    |Password: |+++**@lab.VirtualMachine(55267).Password**+++|

- [] In the @[Azure Portal][azure-portal], click **More services**, and then **Virtual Machines**

- [] On **Virtual Machines** blade, *right click* on created VM **AzS-Host1** > click **Connect**

- [] Logon Azure Stack host VM with @[RDP][rdp] client using following information;
    
    |||
    |---:|:---|
    |Username: |+++ **\Administrator**+++
    |Password: |+++**@lab.VirtualMachine(55267).Password**+++

- [] On desktop *right click* **Install-ASDK.ps1**, click **Run with PowerShell** to run. 

- [] Within the **PowerShell** window, specify the following information;

    |||
    |---:|:---|
    |Execution Policy Change: |**Y** to continue if prompted|
    |Local 'Administrator' Password: |+++**@lab.VirtualMachine(55267).Password**+++ twice|
    |Azure AD Global Administrator user: |+++**@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**+++|
    |Azure AD Global Administrator password: |+++**@lab.VirtualMachine(55267).Password**+++ twice|
    |Admin Password: |+++**@lab.VirtualMachine(55267).Password**+++|
    |Select: |**1**

> [!ALERT] Unless you instructed by the trainer, always select **1** to install the latest ASDK version

- [] **Enter to continue**

> [!KNOWLEDGE] Above automated setup will continue as follows:
- *Download latest version of ASDK*
- *Extract files to D:\*
- *Copy required files to C:\*
- *Extract setup packages to C:\*
- *Run C:\CloudDeployment\Setup\InstallAzureStackPOC.ps1 with required parameters*

> [!ALERT] During ASDK deployment you may lose connectivity to host due to network configuration changes.

> [!ALERT] Azure Stack host will also reboot during the deployment to join the host to AZURESTACK domain.

> [!ALERT] To follow setup progress after reboot, logon using AZURESTACK\AzureStackAdmin and password @lab.VirtualMachine(55267).Password

> [!ALERT] This process takes about 5-6 hours to complete

> **Result:**
> After completing this exercise, you have successfully deployed Azure Stack Development Kit (**ASDK**)

---

Click **Next** to advance to [**LAB 01 - Exercise 3**](#lab01e3) window

---

[**Table of content**](#toc)

---

===
######lab01e3

[**Table of content**](#toc)

---

#Lab 01 - Exercise 3 - First logon to Azure Stack Admin and Tenant Portal

> [!KNOWLEDGE] Now Azure Stack Development Kit is installed

##Task 1: Explore Admin Portal

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

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

##Task 2: Explore Tenant Portal

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-globaladmin-logon]

> [!KNOWLEDGE] Notice that background color has been changed to Blue from Grey, meaning you are logged on as a normal tenant, you do not have administrative rights. Most of the items are missing like **Marketplace Management**, **Plans**, **Offers** 

- [] On Main menu click **+New** and examine new options as you’ve done on admin portal. 

When you try to create any of the items notice that it’s asked you to **Sign up for a new Subscription**.

> [!KNOWLEDGE] Within the following exercise we will continue to setup management environment as a stepping stone to register Azure Stack to Azure Active Directory 

---

Click **Next** to advance to [**LAB 01 - Exercise 4**](#lab01e4) window

---

[**Table of content**](#toc)

---

===
######lab01e4

[**Table of content**](#toc)

---

#Lab 01 - Exercise 4 - Setup Admin Management Environment

##Task 1: Install Azure Stack PowerShell

!INSTRUCTIONS[][rdp-logon]


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

##Task 2: Download Azure Stack tools and extract

> [!KNOWLEDGE] AzureStack-Tools is a GitHub repository that hosts PowerShell modules for managing and deploying resources to Azure Stack 

!INSTRUCTIONS[][rdp-logon]

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

##Task 3: Configure PowerShell - Cloud operator

> [!KNOWLEDGE] As an Azure Stack operator, you can configure your Azure Stack Development Kit's PowerShell environment. After you configure, you can use PowerShell to manage Azure Stack resources such as creating offers, plans, quotas, managing alerts, etc. by PowerShell

!INSTRUCTIONS[][rdp-logon]

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
##Task 4: Test Azure Stack PowerShell

> [!KNOWLEDGE] Now we have configured PowerShell, we can test the configuration by creating a resource group

```PowerShell
New-AzureRMResourceGroup -Name "Test-Resource-Group" -Location Local
```

---

Click **Next** to advance to [**LAB 01 - Exercise 5**](#lab01e5) window

---

[**Table of content**](#toc)

===
######lab01e5

[**Table of content**](#toc)

---

#Lab 01 - Exercise 5 - Register Azure Stack with Azure

##Task 1: Register Azure Stack

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
!IMAGE[Lab Image](https://raw.githubusercontent.com/yagmurs/Wplus-AzS/master/screens/lab01-e5-i1.png)

[Reference Document][lab01-e5-rl]

---

Click **Next** to advance to [**LAB 02 - Objectives and Summary**](#lab02) window

---

[**Table of content**](#toc)

---

===
######lab02

[**Table of content**](#toc)

---

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
>>> [!ALERT] In order to successfuly complete this lab [**Lab 01**](#lab01) must be completed.
>
> ###Variables
>>!INSTRUCTIONS[][variables]
>
> ###Estimated time to complete this lab
>> **30 minutes**
>
> ###Scenario
>> Download several Azure Market Place items to use on installing Resource providers.

---

Click **Next** to advance to [**LAB 02 - Exercise 1**](#lab02e1) window

---

[**Table of content**](#toc)

---

===
######lab02e1

[**Table of content**](#toc)

---

#Lab 02 - Exercise 1 - Download an Azure (Stack) Marketplace Items using Management Portal

##Task 1: Download marketplace items

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] 
- [] Click **More Services** > **Marketplace Management** > **+Add from Azure**

    !IMAGE[Lab Image](https://raw.githubusercontent.com/yagmurs/Wplus-AzS/master/screens/lab02-e1-i1.png)

- [] Search and Select following items one by one from Marketplace

> - Custom Script Extension
> - SQL IaaS Extension
> - PowerShell Desired State Configuration
> - Windows Server 2016 Datacenter - Server Core
> - Windows Server 2016 Datacenter
> - SQL Server 2016 SP1 Enterprise on Windows Server 2016

- [] On **Add from Azure** blade click on the item

    !IMAGE[Lab Image](https://raw.githubusercontent.com/yagmurs/Wplus-AzS/master/screens/lab02-e1-i2.png)

- [] On each item's blade click **Download**

    !IMAGE[Lab Image](https://raw.githubusercontent.com/yagmurs/Wplus-AzS/master/screens/lab02-e1-i3.png)


>   [!KNOWLEDGE] Download process for all items will take about 1 hour to complete

[Reference Document][lab02-e1-rl1]

[Reference Document][lab02-e1-rl2]

---

Click **Next** to advance to [**LAB 03 - Objetives and Summary**](#lab03) window

---

[**Table of content**](#toc)

---

===
######lab03

[**Table of content**](#toc)

---

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
>>> [!ALERT] In order to successfuly complete this lab [**Lab 01](#lab01), and [**Lab 02**](#lab02) must be completed.
>
> ###Variables
>>!INSTRUCTIONS[][variables]
>
> ###Estimated time to complete this lab
>> **60 minutes**
>
> ###Scenario
>> Implement Azure Stack offer delegation by using the Azure Stack portals

---

Click **Next** to advance to [**LAB 03 - Exercise 1**](#lab03e1) window

---

[**Table of content**](#toc)

---

===
######lab03e1

[**Table of content**](#toc)

---

#Lab 03 - Exercise 1 - Prepare for the lab

> In this exercise, you will create Azure Active Directory user accounts that you will be using in this lab:
> - Create a delegated admin user account (as a cloud operator)
> - Create a tenant user account tenantuser (as a cloud operator)

##Task 1: Create a delegated admin user account - delegatedprovider (as a cloud operator)

> In this task, you will:
> - Create a delegated admin user account (as a cloud operator)

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

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

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

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

---

Click **Next** to advance to [**LAB 03 - Exercise 2**](#lab03e2) window

---

[**Table of content**](#toc)

---

===
######lab03e2

[**Table of content**](#toc)

---

#Lab 03 - Exercise 2 - Designate the delegated provider (as a cloud operator)

> In this exercise, you will act as a cloud operator and create a plan consisting of the Subscription service and the corresponding offer.
> Next, you will create a subscription containing the new offer and designate the delegated provider as its subscriber:
> - Create a plan consisting of the Subscription service (as a cloud operator)
> - Create an offer based on the plan (as a cloud operator)
> - Create a new subscription containing the offer (as a delegated provider)

##Task 1: Create a plan consisting of the Subscription service (as a cloud operator)

> In this task, you will:
> - Create a plan consisting of the Subscription service (as a cloud operator)

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

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

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

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

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

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

---

Click **Next** to advance to [**LAB 03 - Exercise 3**](#lab03e3) window

---

[**Table of content**](#toc)

---

===
######lab03e3

[**Table of content**](#toc)

---

#Lab 03 - Exercise 3 - Create and delegate a delegated offer to a delegated provider (as a cloud operator)
> In this exercise, you will act as a cloud operator and create an offer containing services, which the delegated provider will then offer to its tenants:
> - Create a plan to delegate (as a cloud operator)
> - Create an offer based on the plan (as a cloud operator)
> - Delegate the offer to the delegated provider (as a cloud operator)

##Task 1: Create a plan to delegate (as a cloud operator)
> In this task, you will:
> - Create a plan to delegate (as a cloud operator)

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

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

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

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

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

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

---

Click **Next** to advance to [**LAB 03 - Exercise 4**](#lab03e4) window

---

[**Table of content**](#toc)

---

===
######lab03e4

[**Table of content**](#toc)

---

#Lab 03 - Exercise 4 - Create a delegated offer to a tenant (as a delegated provider)
> In this exercise, you will act as a delegated provider and create an offer (based on the offer from the cloud operator) to which your tenants will be able to subscribe:
> - Create a delegated provider offer for a tenant (as a cloud operator)
> - Identify the URL of the delegated provider portal (as a delegated provider)

##Task 1: Create a delegated provider offer for a tenant (as a delegated provider)
> In this task, you will:
> - Create an offer for a tenant (as a delegated provider) based on the cloud operator offer for the delegated provider
> This will allow tenants to create a subscription based on the offer from the delegated provider

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-delegatedprovider-logon]

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

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-delegatedprovider-logon]

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **More services**
- [] In the list of services, click **Subscriptions**
- [] Click **delegatedprovider-subscription**
- [] On the delegated provider subscription blade, click **Properties**
- [] On the properties blade, copy the value of the **PORTAL URL** to clipboard. You will need it in the next exercise of this lab,
- [] **Sign out** from the [Azure Stack Tenant Portal][azurestack-tenantportal]

> [!KNOWLEDGE]Tenants need to subscribe to the offer from the delegated provider portal URL

> **Result:**
> After completing this exercise, you have created a delegated provider offer

---

Click **Next** to advance to [**LAB 03 - Exercise 5**](#lab03e5) window

---

[**Table of content**](#toc)

---

===
######lab03e5

[**Table of content**](#toc)

---

#Lab 03 - Exercise 5 - Sign up for a delegated provider’s offer (as a tenant user-tenantuser)
> In this exercise, you will act as a tenant user who signs up for a delegated provider’s offer and creates a resource in the new subscription. The exercise consists of the following tasks:
> - Sign up for the delegated provider’s offer (as a tenant user-tenantuser)
> - Create a resource in the new subscription (as a tenant user-tenantuser)

##Task 1: Sign up for the delegated provider’s offer (as a tenant user-tenantuser)
> In this task, you will:
> - Sign up for the delegated offer (as a tenant user-tenantuser)

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]

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
- [] On the **Create storage account** blade, specify the following settings;

    > - Name: any unique name consisting of between 3 and 24 lower case letters or digits
    > - Account kind: **General purpose**
    > - Performance: **Standard**
    > - Replication: **Locally-redundant storage (LRS)**
    > - Subscription: **leave as is**
    > - Resource group: create a new group **delegatedlab-RG**
    > - Location: **local**

- [] Click **Create**

> [!ALERT] Wait for the deployment to complete. This should take less than a minute

- [] Verify that the storage account has been successfully created.
- [] Repeat above steps to **Create storage account**. Note that this time you will receive an error message due to exceeded quotas.

> **Result:**
> After completing this exercise, you have subscribed to a delegated provider’s offer, creating this way a new subscription and provisioned a storage account in the new subscription.

---

Click **Next** to advance to [**LAB 04 - Objectives and Summary**](#lab04) window

---

[**Table of content**](#toc)

---

===
######lab04

[**Table of content**](#toc)

---

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
>>> [!ALERT] In order to successfuly complete this lab [**Lab 01](#lab01), and [**Lab 02**](#lab02) must be completed.
>
> ###Variables
>>!INSTRUCTIONS[][variables]
>
> ###Estimated time to complete this lab
>> **20 minutes**
>
> ###Scenario
>> Deploy single file server to be used on App Service installation,

---

Click **Next** to advance to [**LAB 04 - Exercise 1**](#lab04e1) window

---

[**Table of content**](#toc)

---

===
######lab04e1

[**Table of content**](#toc)

---

#Lab 04 - Exercise 1 - Deploy Templates using the Portal

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] Click **+New** > **Custom** > **Template deployment**
- [] On Custom template blade click  **Template / Edit template** > **QuickStart template**
- [] On Edit template blade click  **Quick template** > **QuickStart template**

    !IMAGE[Lab Image](https://raw.githubusercontent.com/yagmurs/Wplus-AzS/master/screens/lab04-e1-i1.png)

- [] Click **select a template (disclaimer)**. Review wide range of the templates.
- [] On Load a quick template section in select a template (disclaimer) combobox select **appservice-fileserver-standalone**

    !IMAGE[Lab Image](https://raw.githubusercontent.com/yagmurs/Wplus-AzS/master/screens/lab04-e1-i2.png)

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

!IMAGE[Lab Image](https://raw.githubusercontent.com/yagmurs/Wplus-AzS/master/screens/lab04-e1-i3.png)

- [] Click **OK**
- [] On Custom deployment blade set Resource Group name as **appservicefilesharerg** select **create new** radio button, and click **Create**

> [!KNOWLEDGE] This template is being downloaded from Azure Stack Quickstart templates gallery on **github.com**

> [!KNOWLEDGE] Template may be called as in following format. Inside the URI which is called there is a redirection to the address below displayed
>
> https://adminportal.local.azurestack.external/#create/Microsoft.Template/uri/**https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAzureStack-QuickStart-Templates%2Fmaster%2Fappservice-fileserver-standalone%2Fazuredeploy.json**

---

Click **Next** to advance to [**LAB 04 - Exercise 2**](#lab04e2) window

---

[**Table of content**](#toc)

---

===
######lab04e2

[**Table of content**](#toc)

---

#Lab 04 - Exercise 2 - Deploy Templates with PowerShell

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

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

---

Click **Next** to advance to [**LAB 05 - Objectives and Summary**](#lab05) window

---

[**Table of content**](#toc)

---

===
######lab05

[**Table of content**](#toc)

---

#Lab 05 - Enable Virtual Machine Scale Sets in Azure Stack

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
>>> [!ALERT] In order to successfuly complete this lab [**Lab 01](#lab01), [**Lab 02**](#lab02), and [**Lab 03**](#lab03) must be completed.
>
> ###Variables
>>!INSTRUCTIONS[][variables]
>
> ###Estimated time to complete this lab
>> **60 Minutes**
>
> ###Scenario
>> Enable Virtual Machine Scale Set feature in Azure Stack using PowerShell via Cloud Operator account.
Afterwards logon to your tenant and create a virtual machine scale set and then manually scale it

---

Click **Next** to advance to [**LAB 05 - Exercise 1**](#lab05e1) window

---

[**Table of content**](#toc)

---

===
######lab05e1

[**Table of content**](#toc)

---

#Lab 05 - Exercise 1 - Enabling VM scale set on Azure Stack
> In this exercise, you will act as a cloud operator and enable Virtual Machine scale sets feature on Azure Stack:
> - Enable Virtual Machine scale sets feature on Azure Stack as a cloud operator
> - Publish VM scale set marketplace item

##Task 1: Enable Virtual Machine Scale Sets
> In this task, you will:
> - Enable Virtual Machine scale sets feature on Azure Stack
> - Publish VM scale set marketplace item

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

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

---

Click **Next** to advance to [**LAB 05 - Exercise 2**](#lab05e2) window

---

[**Table of content**](#toc)

---

===
######lab05e2

[**Table of content**](#toc)

---

#Lab 05 - Exercise 2 - Deploy a virtual machine scale set (VMSS) using Azure Stack tenant portal
> In this exercise, you will act as a tenant and deploy Virtual Machine Scale Set:
> - Deploy a Virtual Machine scale set

##Task 1: Deploy a virtual machine scale set
> In this task, you will:
> - Deploy a Virtual Machine scale set

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **New**
- [] On the **New** blade, click **Compute**
- [] On the **Compute** blade, click **Virtual Machine scale set**
- [] On the **Create Virtual machine scale set** blade, **STEP 1 - Basics** specify the following information:
    |||
    |---:|:---|
    |Virtual machine scale set name: |**rg01vmss**|
    |OS type: |**Windows**|
    |User name: |**localadm**|
    |Password: |**@lab.VirtualMachine(55267).Password** twice|
    |Subscription: |**leave as is**|
    |Resource group: | **Create new**|
    |Resource group name: |**rg01**|
    |Location: |**local**|

- [] Click **OK**
    
- [] On the **Create Virtual machine scale set** blade, **STEP 2 - Virtual machine scale set service settings**:
- [] Click on **Public IP address**, specify the following information:
    |||
    |---:|:---|
    |Name:| **rg01ip01**|
    |Assignment:| **Dynamic**|

- [] Click **OK**

- [] Back in the **Create Virtual machine scale set** blade, **STEP 2 - Virtual machine scale set service settings** specify the following information:
    |||
    |---:|:---|
    |Domain name label:| **testapp01**|
    |Operating system disk image:| **2016-Datacenter**|
    |Instance count:| **1**|

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

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]

- [] Open **Start Menu**
- [] Search for **Remote Desktop Connection**
- [] In the **Remote Desktop Connection**, connect **++testapp01.local.cloudapp.azurestack.external:50000++** using following information;

    > - Username: **\localadm**
    > - Password: **@lab.VirtualMachine(55267).Password**

> **Result:**
> After completing this exercise, you have deployed Virtual Machine scale sets using tenant subscription

---

Click **Next** to advance to [**LAB 05 - Exercise 3**](#lab05e3) window

---

[**Table of content**](#toc)

---

===
######lab05e3

[**Table of content**](#toc)

---

#Lab 05 - Exercise 3 - Manually scale up VMs using PowerShell
> In this exercise, you will act as a tenant and manually scale your Virtual Machine Scale Set:
> - Configure PowerShell to access tenant subscription
> - Manually scale a Virtual Machine scale set

##Task 1: Configure PowerShell Environment for Tenant
> In this task, you will:
> - Configure PowerShell to access tenant subscription

!INSTRUCTIONS[][rdp-logon]

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and replace **< >** fields between brackets with the values represent your environment.

> [!KNOWLEDGE] The following Powershell script will set up tenant PowerShell environment and logon using Powershell to manage Azure Stack cloud resources

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

!INSTRUCTIONS[][rdp-logon]

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and replace **< >** fields between brackets with the values represent your environment.

> [!KNOWLEDGE] The following Powershell script scales Virtual Machine scale set in **rg01** resource group Intances to **2**

```PowerShell
# Select resource group by name
$vmss = Get-AzureRmVmss -ResourceGroupName rg01

# Select first VMSS in the resource group
$vmss = Get-AzureRmVmss -ResourceGroupName rg01 -VMScaleSetName $vmss[0].Name

#Increase the VMSS size to 2
$vmss.sku.Capacity = 2

#Apply settings to VMSS
Update-AzureRmVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss
```

!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Resource Groups**
- [] On the **Resource groups** blade, click **rg01**
- [] On the **rg01 - Deployments** blade, click on **Deployments**
- [] Select **Virtual Machine Scale Set** type from the list of resources
- [] On the **rg01vmms** blade, click **Instances**

> [!KNOWLEDGE] You should see 1 instance running and another one being created.

> [!ALERT] Wait for the deployment to complete. This should take about 15 minutes

##Task 3: Validate Virtual Machine Scale set
> In this task, you will:
> - Validate the result of scaling up Virtual Machine Scale set on previous task


- [] To validate the VM you may open a Remote Desktop Connection.

> [!KNOWLEDGE] Because of the default load balancer rules, port 50001 on the Load Balancer will be mapped to the second VM in the scale set on port 3389.

- [] Open **Start Menu**
- [] Search for **Remote Desktop Connection**
- [] In the **Remote Desktop Connection**, connect **++testapp01.local.cloudapp.azurestack.external:50001++** using following information;

    > - Username: **\localadm**
    > - Password: **@lab.VirtualMachine(55267).Password**

> **Result:**
> After completing this exercise, you have connected to your tenant subscription using PowersShell, manually scaled up Virtual Machine scale set, and validated the connectivity to all intances

---

Click **Next** to advance to [**LAB 06 - Objectives and Summary**](#lab06) window

---

[**Table of content**](#toc)

---

===
######lab06

[**Table of content**](#toc)

---

#Lab 06 - Manage Storage Resources

##^[**Objectives and Summary**][lab06-os]

> [lab06-os]:
> ###Introduction
>> This Lab covers basic operations with storage accounts, such as creation, deletion and restoration of such resources.
>
> ###Objectives
> In this lab, you will:
> - Enable Retention Policy
> - Create a new resource group
> - Create a new storage account
> - Delete a storage account
> - Recover a deleted storage account
> - Confirm storage account recovery
> - Reclaim space
>
> ###Prerequisites
>>> [!ALERT] In order to successfuly complete this lab [**Lab 01](#lab01), [**Lab 02**](#lab02), and [**Lab 03**](#lab03) must be completed.
>
> ###Variables
>>!INSTRUCTIONS[][variables]
>
> ###Estimated time to complete this lab
>> **30 Minutes**
>
> ###Scenario
>> Enable Retention policy for storage accounts in Azure Stack using Admin Portal via Cloud Admin account.
Afterwards logon to your tenant and create a new resource group, and storage account under this resource group. Delete the same storage account to test retention policy.
Login back to the Admin Portal and recover said storage account and validate that it is restored.

---

Click **Next** to advance to [**LAB 06 - Exercise 1**](#lab06e1) window

---

[**Table of content**](#toc)

---

===
######lab06e1

[**Table of content**](#toc)

---

#Lab 06 - Exercise 1 - Working with retention policy
> In this exercise, you will act as cloud operator to configure retention policy, recover storage account,
and act as a tenant user to create, delete storage account, and confirm recovery of the storage account:
> - Enable Retention Policy
> - Create a new resource group
> - Create a new storage account
> - Delete a storage account
> - Recover a deleted storage account
> - Confirm storage account recovery

##Task 1: Setup retention policy (as cloud operator)
> In this task, you will:
> - Change retention policy settings

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **Dashboard**
- [] In the Dashboard click **local** in **Region management** list
- [] On the **local** blade, click **Resource providers**, then click **Storage**
- [] On the **Storage** blade, click **Configuration**
- [] Set *Retention period for deleted storage accounts (days)*: **15**
- [] Click **Save**

##Task 2: Create a storage account (as tenant user)
> In this task, you will:
> - Create a new resource group and a new storage account to be prepared upcoming lab exercises

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Resource Groups**
- [] On the **Resource Groups** blade, click **+Add**
- [] On **Resource Group** blade, specify the following settings;

    > - Resource group name: **rg02**
    > - Subscription: **leave as is**
    > - Resource group location: **local**

- [] Click **Create**

> [!ALERT] Wait for the deployment to complete. This should take just a few seconds

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **+New**
- [] On the **New** blade, click **Data + Storage**
- [] On the **Storage** blade, click **Storage account**
- [] On the **Create storage account** blade, specify the following settings;

    > - Name: **rg02sa**
    > - Account kind: **General purpose**
    > - Performance: **Standard**
    > - Replication: **Locally-redundant storage (LRS)**
    > - Subscription: **leave as is**
    > - Resource group: Use existing and select **rg02**
    > - Location: **local**

- [] Click **Create**

> [!ALERT] Wait for the deployment to complete. This should take less than a minute

##Task 3: Delete Storage Account (as tenant user)
> In this task, you will:
> - Delete a storage account to demostrate how to recover storage account on upcoming lab exercises

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Resource Groups**
- [] On the **Resource Groups** blade, click **rg02**
- [] On the **rg02** blade, click **rg02sa** storage account
- [] On the **rg02sa** blade, click **Delete** button
- [] On the **Delete storage account** blade, specify the following settings;

    > - Type the name of the storage account (rg02sa) to confirm: **rg02sa**

- [] Click **Delete**

> [!ALERT] Wait for processing to complete. This should take less than a minute

- [] Confirm that rg02 is **empty**

##Task 4: Restore the Storage Account (as cloud operator)
> In this task, you will:
> - Recover a deleted storage account on previous lab exercise

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **Dashboard**
- [] In the Dashboard click **local** in **Region management** list
- [] On the **local** blade, click **Storage accounts**, then click **Filter**, and specify the following settings;

    > - Account name: **rg02sa**
    > - Tenant subscription ID: **leave empty**
    > - Storage account status: **leave empty**

- [] Click **Done**

- [] On the **local - Storage accounts** blade, click **rg02sa@<\DateTime>**
- [] On the **Storage account** blade, click **Recover** button, then click **yes**

> [!ALERT] Wait for processing to complete. This should take less than a minute

##Task 5: Confirm Recover operation (as tenant user)
> In this task, you will:
> - Confirm recovered storage account

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Resource groups**
- [] On the **Resource Groups** blade, click **rg02**
- [] On the **rg02** blade, click **rg02sa** storage account
- [] Deleted storage account should be recovered and visible as a resource in the resource group.

> **Result:**
> After completing this exercise, you have configured retention policy, deletes storage account, and recovered storage account

---

Click **Next** to advance to [**LAB 06 - Exercise 2**](#lab06e2) window

---

[**Table of content**](#toc)

---

===
######lab06e2

[**Table of content**](#toc)

---

#Lab 06 - Exercise 2 - Reclaim space from deleted storage accounts
> In this exercise, you will act as a cloud operator and manually reclaim space to trigger cleanup process:
> - Reclaim space

##Task 1: Reclaim space (as cloud operator)
> In this task, you will:
> - Manually reclaim space to trigger cleanup process

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]


- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **Dashboard**
- [] In the Dashboard click **local** in **Region management** list
- [] On the **local** blade, click **Storage accounts**, then click **Reclaim space**, then click **OK**

> [!KNOWLEDGE] **Reclaim space** process will force garbage collection of all deleted storage accounts, regardless of the retention period setting

> **Result:**
> After completing this exercise, you have manually triggered garbage collection by clicking **Reclaim space** button

---

Click **Next** to advance to [**LAB 07 - Objectives and Summary**](#lab07) window

---

[**Table of content**](#toc)

---

===

######lab07

[**Table of content**](#toc)

---

#Lab 07 - 

##^[**Objectives and Summary**][lab07-os]

> [lab07-os]:
> ###Introduction
>> This Lab covers basic operations with storage accounts, such as creation, deletion and restoration of such resources.
>
> ###Objectives
> In this lab, you will:
> - Enable Retention Policy
> - Create a new resource group
> - Create a new storage account
> - Delete a storage account
> - Recover a deleted storage account
> - Confirm storage account recovery
> - Reclaim space
>
> ###Prerequisites
>>> [!ALERT] In order to successfuly complete this lab [**Lab 01](#lab01), [**Lab 02**](#lab02), and [**Lab 03**](#lab03) must be completed.
>
> ###Variables
>>!INSTRUCTIONS[][variables]
>
> ###Estimated time to complete this lab
>> **30 Minutes**
>
> ###Scenario
>> Enable Retention policy for storage accounts in Azure Stack using Admin Portal via Cloud Admin account.
Afterwards logon to your tenant and create a new resource group, and storage account under this resource group. Delete the same storage account to test retention policy.
Login back to the Admin Portal and recover said storage account and validate that it is restored.

---

Click **Next** to advance to [**LAB 07 - Exercise 1**](#lab07e1) window

---

[**Table of content**](#toc)

---

===
######lab07e1

[**Table of content**](#toc)

---

#Lab 07 - Exercise 1 - 
> In this exercise, you will act as cloud operator to configure retention policy, recover storage account,
and act as a tenant user to create, delete storage account, and confirm recovery of the storage account:
> - Enable Retention Policy
> - Create a new resource group
> - Create a new storage account
> - Delete a storage account
> - Recover a deleted storage account
> - Confirm storage account recovery

##Task 1: 
> In this task, you will:
> - Change retention policy settings

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]
!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **Dashboard**

##Task 2: 
> In this task, you will:
> - Create a new resource group and a new storage account to be prepared upcoming lab exercises

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]
!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Resource Groups**

> [!ALERT] Wait for the deployment to complete. This should take just a few seconds

##Task 3: 
> In this task, you will:
> - Delete a storage account to demostrate how to recover storage account on upcoming lab exercises

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]
!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Resource Groups**


##Task 4: 
> In this task, you will:
> - Recover a deleted storage account on previous lab exercise

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]
!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **Dashboard**

##Task 5: 
> In this task, you will:
> - Confirm recovered storage account

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]
!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Resource groups**

> **Result:**
> After completing this exercise, you have configured retention policy, deletes storage account, and recovered storage account

---

Click **Next** to advance to [**LAB 07 - Exercise 2**](#lab07e2) window

---

[**Table of content**](#toc)

---

===
######lab07e2

[**Table of content**](#toc)

---

#Lab 07 - Exercise 2 - 
> In this exercise, you will act as a cloud operator and manually reclaim space to trigger cleanup process:
> - Reclaim space

##Task 1: 
> In this task, you will:
> - Manually reclaim space to trigger cleanup process

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]
!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **Dashboard**

> **Result:**
> After completing this exercise, you have manually triggered garbage collection by clicking **Reclaim space** button

---

Click **Next** to advance to [**LAB 08 - Objectives and Summary**](#lab08) window

---

[**Table of content**](#toc)

---

===

######lab08

[**Table of content**](#toc)

---

#Lab 08 - Monitor Health and Alerts in Azure Stack

##^[**Objectives and Summary**][lab08-os]

> [lab08-os]:
> ###Introduction
>> Azure Stack includes infrastructure monitoring capabilities that enable you to view health and alerts for an Azure Stack region. Health and alerts are managed by the Health resource provider. Azure Stack infrastructure components register with the Health resource provider during Azure Stack deployment and configuration. This registration enables the display of health and alerts for each component.
>
> ###Objectives
>> In this lab, you will:
> Monitor health and alerts of local Azure Stack components. Furthermore you will also establish a remote session to privileged endpoint to run tests on Azure Stack.
>
> ###Prerequisites
>> No prerequisites
>
> ###Variables
>>!INSTRUCTIONS[][variables]
>
> ###Estimated time to complete this lab
>> **15 Minutes**
>

---

Click **Next** to advance to [**LAB 08 - Exercise 1**](#lab08e1) window

---

[**Table of content**](#toc)

---

===
######lab08e1

[**Table of content**](#toc)

---

#Lab 08 - Exercise 1 - View Health and Alerts
> In this exercise, you will act as cloud operator to monitor health and alerts of local Azure Stack.

##Task 1: View Health 
> In this task, you will:
> - view health of local region

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **Dashboard**
- [] In the Dashboard click **local** in **Region management** list

!IMAGE[image8_1.jpg](image8_1.jpg)

> [!KNOWLEDGE] The Region management tile, pinned by default in the administrator portal for the Default Provider Subscription, lists all the deployed regions of Azure Stack. The tile shows the number of active critical and warning alerts for each region and is your entry point into the health and alert functionality of Azure Stack.

!IMAGE[image8_2.jpg](image8_2.jpg)

- [] On the **local** blade, click **Resource providers**, then click **Storage** to view more detailed information.

> [!KNOWLEDGE] You can click a resource provider or infrastructure role to view more detailed information. If you click an infrastructure role, and then click the role instance, there are options to Start, Restart, or Shutdown. Do not use these actions when you apply updates to an integrated system. Also, do not use these options in an Azure Stack Development Kit environment. These options are designed only for an integrated systems environment, where there is more than one role instance per infrastructure role. Restarting a role instance (especially AzS-Xrp01) in the development kit causes system instability.

##Task 2: View Alerts
> In this task, you will:
> - View alerts for the local region

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **Dashboard**
- [] In the Dashboard click **local** in **Region management** list

> [!KNOWLEDGE] The list of active alerts for each Azure Stack region is available directly from the Region management blade. The first tile in the default configuration is the Alerts tile, which displays a summary of the critical and warning alerts for the region.

!IMAGE[image8_3.jpg](image8_3.jpg)

- [] Click on top part of the Alerts tile to view list of all active alerts.

!IMAGE[image8_4.jpg](image8_4.jpg)

> [!KNOWLEDGE] By selecting the top part of the Alerts tile, you navigate to the list of all active alerts for the region. If you select either the Critical or Warning line item within the tile, you navigate to a filtered list of alerts (Critical or Warning).

##Task 3: View the public IP address usage information
> In this task, you will:
> view the total number of public IP addresses that have been consumed in the region

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **Dashboard**
- [] In the Dashboard click **local** in **Region management** list
- [] Under **local** blade, click **Resource providers**
- [] From the list of Resource Providers, select **Network**.
- [] The **Network** blade displays the **Public IP pools usage** tile in the **Overview** section.

!IMAGE[image8_5.jpg](image8_5.jpg)

##Task 4: Find a storage account
> In this task, you will:
> view list of all storage accounts

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the Dashboard click **local** in **Region management** list
- [] Select **Storage** from the **Resource Providers** list.
- [] Now, on the storage Resource Provider administrator blade – scroll down to the **Storage accounts** tab and click it.

> The resulting page is the list of storage accounts in that region.

---

Click **Next** to advance to [**LAB 08 - Exercise 2**](#lab08e2) window

---

[**Table of content**](#toc)

---

===
######lab08e2

[**Table of content**](#toc)

---

#Lab 08 - Exercise 2 - Using the privileged endpoint in Azure Stack
> In this exercise, you will logon to the privileged endpoint and test Azure Stack health

> [!KNOWLEDGE] As an Azure Stack operator, you should use the administrator portal, PowerShell, or Azure Resource Manager APIs for most day-to-day management tasks. However, for some less common operations, you need to use the privileged endpoint (PEP). The PEP is a pre-configured remote PowerShell console that provides you with just enough capabilities to help you perform a required task. You access the PEP through a remote PowerShell session on the virtual machine that hosts the PEP. In the ASDK, this virtual machine is named AzS-ERCS01. If you’re using an integrated system, there are three instances of the PEP, each running inside a virtual machine (Prefix-ERCS01, Prefix-ERCS02, or Prefix-ERCS03) on different hosts for resiliency.

##Task 1: Login to Privileged Endpoint
> In this task, you will:
> - Establish a remote sesssion to privileged endpoint

!INSTRUCTIONS[][rdp-logon]

- [] Within Azure Stack host VM (Azs-Host1), open an elevated Windows Powershell session.
- [] Run following command to add PEP as a trusted host.

```PowerShell
winrm s winrm/config/client '@{TrustedHosts="azs-ercs01"}'
```
- [] Run following command in the same elevated Windows Powershell session to establish a remote session;
    
```PowerShell
$cred = Get-Credential
 Enter-PSSession -ComputerName azs-ercs01 -ConfigurationName PrivilegedEndpoint -Credential $cred
```

- [] When prompted, use the following credentials:

!INSTRUCTIONS[][azurestackadmin-user]


> [!KNOWLEDGE] You can validate the status of your Azure Stack. When you have an issue, contact Microsoft Customer Services Support. Support asks you to run Test-AzureStack from your management node. The validation test isolates the failure. Support can then analyze the detailed logs, focus on the area where the error occurred, and work with you in resolving the issue.

- [] Run following command in the same elevated Windows Powershell session to test Azure Stack;

```PowerShell
Test-AzureStack
```
> [!KNOWLEDGE] If any tests report fail, run: Get-AzureStackLog -FilterByRole "rolename" -OutputPath "Log output path". The cmdlet gathers the logs from Test-AzureStack
!IMAGE[5px64167.jpg](5px64167.jpg)


> **Result:**
> After completing this exercise, 

---

Click **Next** to advance to [**LAB 09 - Objectives and Summary**](#lab09) window

---

[**Table of content**](#toc)

---

===
######lab09

[**Table of content**](#toc)

---

#Lab 09 - Working with SQL Resource Provider

##^[**Objectives and Summary**][lab09-os]

> [lab09-os]:
> ###Introduction
>> Use the SQL Server resource provider adapter to expose SQL databases as a service of Azure Stack. After you install the resource provider and connect it to one or more SQL Server instances, you and your users can create:
> - Databases for cloud-native apps.
> - Websites that are based on SQL.
> - Workloads that are based on SQL. You don't have to provision a virtual machine (VM) that hosts SQL Server each time.
>
> ###Objectives
>> In this lab, you will:
> - Install Azure Stack SQL Resource Provider
> - Install SQL Server
> - Configure SQL server as SQL Hosting Server
> - Create a database as a tenant
>
> ###Prerequisites
>>> [!ALERT] In order to successfuly complete this lab [**Lab 01](#lab01), [**Lab 02**](#lab02), and [**Lab 03**](#lab03) must be completed.
>
> ###Variables
>>!INSTRUCTIONS[][variables]
>
> ###Estimated time to complete this lab
>> **30 Minutes**
>

---

Click **Next** to advance to [**LAB 09 - Exercise 1**](#lab09e1) window

---

[**Table of content**](#toc)

---

===
######lab09e1

[**Table of content**](#toc)

---

#Lab 09 - Exercise 1 - Install SQL Resource Provider and Configure Quotas
> In this exercise, you will deploy SQL Resource Provider to enable SQL PaaS services.
- Find out Azure Stack version to decide which SQL Resource provider version to install
- Download and install the SQL resource provider
- Configure SQL Adapter Quota and SKU

##Task 1: Get Azure Stack version
> In this task, you will:
> - Find out Azure Stack version to decide which SQL Resource provider version to install

!INSTRUCTIONS[][rdp-logon]

- [] Within Azure Stack host VM (Azs-Host1), open an elevated Windows Powershell session.

- [] Run following command to find out ASDK version;
    
```PowerShell
$cred = Get-Credential
$session = New-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint -Credential $cred
$stamp = Invoke-Command -Session $session -ScriptBlock {Get-AzureStackStampInformation}
$stamp.StampVersion 
```
> When prompted, use the following credentials:
> - User name: **AZURESTACK\AzureStackAdmin**
> - Password: **@lab.VirtualMachine(55267).Password**

##Task 2: Download and Install SQL Resource Provider
> In this task, you will:
> - Download and install the SQL resource provider

!INSTRUCTIONS[][rdp-logon]

- [] Navigate to https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-sql-resource-provider-deploy
- [] Find relevant Resource Provider matching Azure Stack Development Kit version and download the file to **C:\AzureStackOnAzureVM** folder.

> You may need to enable file download on IE Internet Sites.

- [] Open **File Explorer** and navigate to **C:\AzureStackOnAzureVM** folder.
- [] Right click and go to properties of the downloaded file.
- [] **Unblock** the file.
- [] Double click on the downloaded file and extract it to **c:\AzureStackOnAzureVM\SQLRP**
- [] Click **Start** menu
- [] Search and run **PowerShell ISE** as **Administrator**.
- [] Copy & paste the below script to a new file and replace **< >** fields between brackets with the values represent your environment.

```PowerShell
# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack and the default prefix is AzS
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"
	 
# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\azurestackonazurevm\SQLRP'
	 
# The service admin account (can be AAD or ADFS)
$serviceAdmin =  "@lab.UserFirstName@<\tenant name>.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "@lab.VirtualMachine(55267).Password" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)
	 
# Set credentials for the new Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "@lab.VirtualMachine(55267).Password" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)
	 
# and the cloudadmin credential required for Privileged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "@lab.VirtualMachine(55267).Password" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)
	 
# change the following as appropriate
$PfxPass = ConvertTo-SecureString "@lab.VirtualMachine(55267).Password" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files
# and adjust the endpoints
$tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds -VMLocalCredential $vmLocalAdminCreds -CloudAdminCredential $cloudAdminCreds -PrivilegedEndpoint $privilegedEndpoint -DefaultSSLCertificatePassword $PfxPass -DependencyFilesLocalPath $tempDir\cert

```

##Task 3: Create SQL Adapter Quota and SKU
> In this task, you will:
> - Learn how to configure SQL Adapter Quota and SKU

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **More Services**
- [] Search for **SQL Adapter**, select it.
- [] Under **Settings** blade select **Quota**
- [] Click **New**
- [] On the **bladename** blade, **STEP 1 - stepname** specify the following information:

    > - Quota Name: **SQLQuota_1**
    > - Maximum size for all databases (GB): **1**
    > - Maximum number of databases: **2**

- [] Click **Create**
- [] Under **Settings** blade select **SKUs**
- [] Click **New**
- [] On the **bladename** blade, **STEP 1 - stepname** specify the following information:

    > - Name: **SQLSKU_1**
    > - Family: **SQL Server 2016**
    > - Tier: **Standalone**
    > - Edition: **Enterprise**

- [] Click **Create**

> **Result:**
> After completing this exercise, you have installed a SQL resource providers and published marketplace items to Azure Stack.

---

Click **Next** to advance to [**LAB 09 - Exercise 2**](#lab09e2) window

---

[**Table of content**](#toc)

---

===
######lab09e2

[**Table of content**](#toc)

---

#Lab 09 - Exercise 2 - Install SQL Hosting Servers
> In this exercise, you will deploy SQL server instance to host SQL DB as PaaS
- Install SQL server
- Configure SQL servers as SQL Hosting Servers
- Make SQLAdapter available to existing plan for tenants to consume

##Task 1: Install SQL Server
> In this task, you will:
> - Install SQL server

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **ServiceName (SQL)**
- [] On the **Create Virtual Machine** blade, **STEP 1 - Basics** specify the following information:
	    
	> - Name: **sqlhosting**
	> - User name: **localadm**
	> - Password: **@lab.VirtualMachine(55267).Password**
	> - Subscription: **leave as is**
	> - Resource group: select **Create new**
	> - Resource group name: **rgSQL**
	> - Location: **local**

 - [] On the **Create Virtual Machine** blade, **STEP 2 - Size** specify the following information:

	> - Select VM Size **DS1v2**
	
 - [] On the **Create Virtual Machine** blade, **STEP 3 - Settings** specify the following information:

	> - Review and accept the default settings and continue

- [] On the **Create Virtual Machine** blade, **STEP 4 - SQL Server Settings** specify the following information:

	> - SQL connectivity: **Public (Internet)**
	> - Port: **1433**
	> - SQL Authentication: **Enable**
	> - Login name: **localsa**
	> - Password: **@lab.VirtualMachine(55267).Password**
	
- [] On the **Create Virtual Machine** blade, **STEP 5 - Summary** specify the following information:
	> - Verify and click **OK**

> - Wait for deployment to finish, this may take some time.

> - Once the Server deployment finishes go to the **Virtual Machine** settings and make note of **public IP address** of the server.

- [] In the Resource Groups blade select the newly created resource group **rgSQL**.
- [] In the Overview blade select the Public IP address resource for the Virtual machine **sqlhosting-ip**.
- [] Make note of the public IP address.


##Task 2: Configure SQL Server as SQL Hosting Server
> In this task, you will:
> - Configure SQL servers as SQL Hosting Servers

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **More Services**
- [] Search for **SQL Hosting Servers**,  and select it.
- [] On the **Add a SQL Hosting Server** blade, specify the following information:

	> - SQL Server Name: **sqlhosting-ip** from task 1.
	> - User name: **localsa**
	> - Password: **@lab.VirtualMachine(55267).Password**
	> - Size of Hosting Server in GB: **5**
	> - Subscription: **leave as is**
	> - Resource group: select **Use existing**
	> - Resource group name: **rgSQL**
	> - Location: **local**
	> - SKUs: Select **SQLSKU_1**
	
##Task 3: Add SQL Adapter service to the plan
> In this task, you will:
> - Make SQLAdapter available to the already existing plan for tenants to consume.

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **Plans**
- [] Under Plans blade select **Plan name**
- [] Select **Services and quotas** on the new blade.
- [] Click **Add**
- [] Click on **Services** and select **Microsoft.SQLAdapter**
- [] Click **OK**
- [] Click **Quotas** and select **SQLQuota_1**
- [] Click **OK** twice

> **Result:**
> After completing this exercise, you have deployed SQL server to enable SQL DB PaaS services on Azure Stack

---

Click **Next** to advance to [**LAB 09 - Exercise 3**](#lab09e3) window

---

[**Table of content**](#toc)

---

===
######lab09e3

[**Table of content**](#toc)

---

#Lab 09 - Exercise 3 - Create SQL Database
> In this exercise, you will test deployment of a SQL DB PaaS functionality
- Create a SQL database as a tenant
- Validate the previously created SQL database
- Make SQLAdapter available to existing plan for tenants to consume

##Task 1: Create a SQL database
> In this task, you will:
> - Create a SQL database as a tenant

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Resource Groups**
- [] On the **Add Resource Group** blade, specify the following information:

	> - Resource group name: **rgSQLDB**
	> - Subscription: **leave as is** 
	> - Resource group location: **local**
	
- [] Under Resource Groups blade select the newly created resource group **rgSQLDB**
- [] Click **Add** on the Resource Blade
- [] Find **SQL Database** and select it.
- [] On the **Create Database** blade, specify the following information:

	> - Database name: **db01**
	> - Collation: **SQL_Latin1_General_CP1_CI_AS**
	> - Max Size in MB: **512**
	> - Subscription: **leave as is**
	> - Resource group: select **Use existing**
	> - Resource group name: **rgSQLDB**
	> - Location: **local**
	> - SKU: Select **SQLSKU_1**
	> - Login: **Create a new database login**
	> - Database login: **dbuser01**
	> - Password: **@lab.VirtualMachine(55267).Password**
	
##Task 2: Validate the SQL database
> In this task, you will:
> - Validate the previously created SQL database

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]

- [] In the [Azure Stack Tenant Portal][azurestack-tenantportal], click **Resource Groups**
- [] Select **rgSQLDB**

> [!KNOWLEDGE] You should see two resources inside this resource group labelled **db01** for the SQL Database and **dbuser01** for SQL Login.

> **Result:**
> After completing this exercise, you have deployed a SQL database to demostrate Azure Stack SQL DB PaaS and validated the database deployment

---

Click **Next** to advance to [**LAB 10**](#lab10) window

---

[**Table of content**](#toc)

---

===

######lab10

[**Table of content**](#toc)

---

#Lab 10 - 

##^[**Objectives and Summary**][lab10-os]

> [lab10-os]:
> ###Introduction
>> This lab portion is about deploying App Service Resource Provider.
>
> ###Objectives
> - Deploy prerequisite services for Azure App Service on Azure Stack 
> - Download the Azure App Service on Azure Stack Installer and Helper Scripts
> - Complete all prerequisites for Azure App Service on Azure Stack 
> - Run Azure App Service installer on Azure Stack 
>
> ###Prerequisites
>>> [!ALERT] In order to successfuly complete this lab [**Lab 01](#lab01), [**Lab 02**](#lab02), [**Lab 03**](#lab03), [**Lab 04**](#lab04), and [**Lab 09**](#lab09) must be completed.
>
> ###Variables
>>!INSTRUCTIONS[][variables]
>
> ###Estimated time to complete this lab
>> **4 Hours**
>
> ###Scenario
>> Enable App Service Resource Provider in Azure Stack to provide your users the ability to create web and API applications.
Afterwards you will include it in your offers, and plans and will be included in subscription. Users will then deploy a Database and DNN app using App Service resource provider.

---

Click **Next** to advance to [**LAB 10 - Exercise 1**](#lab10e1) window

---

[**Table of content**](#toc)

---

===
######lab10e1

[**Table of content**](#toc)

---

#Lab 10 - Exercise 1 - Deploy and Validate App Service Infrastructure requirements
> In this exercise, you will act as cloud operator and;
> - Validate File Server 
> - Deploy a SQL Server for App Service Resource Provider

##Task 1: Validate File Server installed for App Service on [**Lab 04**](#lab04)
> In this task, you will:
> - Validate File Server

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **Virtual Machines**
- [] On the **Resource Groups** blade, click **appservicefilesharerg**
- [] On the **appservicefilesharerg** blade, click **FileServerPublicIp** type of Public IP address
- [] On the **FileServerPublicIp** blade, note the value under **DNS name**

    > [!NOTE] The name is supposed to be **appservicefileshare.local.cloudapp.azurestack.external**

    > [!KNOWLEDGE] This will be used on App Service Registration later within this lab.

##Task 2: Install SQL Server 2016 SP1 Enterprise on Windows Server 2016 VM
> In this task, you will deploy SQL Server 2016 SP1 Enterprise VM from Market Place:
> - Deploy a SQL Server for App Service Resource Provider

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] In the [Azure Stack Admin Portal][azurestack-adminportal], click **+New**
- [] On the **New** blade, click **Compute**
- [] On the **Compute** blade, click **SQL Server 2016 SP1 Enterprise on Windows Server 2016**, then click **Create** 
- [] On the **Create virtual machine** blade, **STEP 1 - Basics** specify the following information:
    |||
    |---:|:---|
    |Name: |**Appsvcsql**|
    |User name: |**appsvcsqluser**|
    |Password: |**@lab.VirtualMachine(55267).Password** twice|
    |Subscription: |**leave as is**|
    |Resource group: | **Create new**|
    |Resource group name: |**appsvcsqlrg**|
    |Location: |**local**|

- [] Click **OK**
    
- [] On the **Create Virtual machine** blade, **STEP 2 - Size**:
- [] Click on **DS1_V2**, click **Select**

- [] On the **Create Virtual machine** blade, **STEP 3 - Setting**
- [] Examine default settings and click **Select**

- [] On the **Create Virtual machine** blade, **STEP 4 - SQL Server settings** specify the following information:
    |||
    |---:|:---|
    |SQL connectivity: | **Public (Internet)**|
    |Port: | **1433**|
    |SQL Authentication: | click **Enable**|
    |Login name: | **appsvcsqluser**|
    |Password: | **leave as is** or set **@lab.VirtualMachine(55267).Password**|
    |Storage configuration: | **leave as is**|
    |Automated patching: | **leave as is**|
    |Automated backup: | **leave as is**|
    |Azure Key Vault integration: | **leave as is**|
    |R Services (Advanced analytics): | **leave as is**|

!IMAGE[Lab Image](https://raw.githubusercontent.com/yagmurs/Wplus-AzS/master/screens/lab10-e1-i1.png)

- [] On the **Create Virtual machine** blade, **STEP 5 - Summary**, examine settings
- [] Click **OK**

> [!ALERT] Wait for deployment to finish, this will take approximately 20 minutes

> **Result:**
> After completing this exercise, you have 

---

Click **Next** to advance to [**LAB 10 - Exercise 2**](#lab10e2) window

---

[**Table of content**](#toc)

---

===
######lab10e2

[**Table of content**](#toc)

---

#Lab 10 - Exercise 2 - Download the Azure App Service on Azure Stack Installer and Helper Scripts
> In this exercise, you will act as a cloud operator and:
> - Download and extract the App Service on Azure Stack deployment helper scripts.
> - Download the App Service on Azure Stack installer.

##Task 1: 
> In this task, you will you will act as a cloud operator and:
> - Download and extract the App Service on Azure Stack deployment helper scripts.

!INSTRUCTIONS[][rdp-logon]

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and run.

```PowerShell
#Download and extract AppServiceHelperScripts
$defaultLocalPath = "C:\AzureStackonAzureVM"
Invoke-WebRequest -Uri "https://aka.ms/appsvconmashelpers" -OutFile "$defaultLocalPath\AppServiceHelperScripts.zip"
Expand-Archive -Path "$defaultLocalPath\AppServiceHelperScripts.zip" -DestinationPath "$defaultLocalPath\AppServiceHelperScripts"
```

##Task 2: 
> In this task, you will you will act as a cloud operator and:
> - Download the App Service on Azure Stack installer.

!INSTRUCTIONS[][rdp-logon]

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and run.

```PowerShell
#Download AppService
$defaultLocalPath = "C:\AzureStackonAzureVM"
Invoke-WebRequest -Uri "https://aka.ms/appsvconmasinstaller" -OutFile "$defaultLocalPath\AppService.exe"
```

> **Result:**
> After completing this exercise, you have downloaded and extracted the App Service installer and helper scripts before you start installing App Service installation on Azure Stack Development Kit

---

Click **Next** to advance to [**LAB 10 - Exercise 3**](#lab10e3) window

---

[**Table of content**](#toc)

---

===
######lab10e3

[**Table of content**](#toc)

---

#Lab 10 - Exercise 3 - Completing all prerequisites for Azure App Service on Azure Stack Installer
> In this exercise, you will act as a cloud operator and:
> - Validate Appservice helper scripts existance
> - Request certificates required by App Service on Azure Stack
> - Download Azure Stack Root certificate
> - Create an Azure Active Directory application ID to be used by App Service on Azure Stack
> - Grant permission to Azure Active Directory application ID

##Task 1: Validate helper scripts
> In this task, you will you will act as a cloud operator and:
> - Validate Appservice helper scripts existance

!INSTRUCTIONS[][rdp-logon]

- [] Check if the following folder structure is available on **C:\AzureStackOnAzureVM\AppServiceHelperScripts**

>[!NOTE]
> - Common.ps1
> - Create-AADIdentityApp.ps1
> - Create-ADFSIdentityApp.ps1
> - Create-AppServiceCerts.ps1
> - Get-AzureStackRootCert.ps1
> - Remove-AppService.ps1
> - Modules
>   - GraphAPI.psm1

> [!KNOWLEDGE] Following exercises will use above scripts.

##Task 2: Request App Service certificates
> In this task, you will you will act as a cloud operator and:
> - Request certificates required by App Service on Azure Stack

!INSTRUCTIONS[][rdp-logon]

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and run.
- [] Use +++@lab.VirtualMachine(55267).Password+++ when password prompted.

```PowerShell
#Run following PowerShell command to request certificates
$defaultLocalPath = "C:\AzureStackonAzureVM"
Cd \
Cd $defaultLocalPath\AppServiceHelperScripts
.\Create-AppServiceCerts.ps1 -PfxPassword ("@lab.VirtualMachine(55267).Password" | ConvertTo-SecureString -AsPlainText -Force) -DomainName local.azurestack.external
```

> [!KNOWLEDGE]  **Create-AppServiceCerts.ps1** script request all certificates that is required by App Service installation on Azure Stack.

> [!KNOWLEDGE] Following files will be used on App service installation on upcoming exercises

**Following 4 certificate files will be created**
> [!NOTE] 
|||
|:---|:---|
|App Service default SSL certificate| **_.appservice.local.azurestack.external.pfx**|
|App Service API SSL certificate| **Api.appservice.local.azurestack.external.pfx**|
|App Service publisher SSL certificate| **ftp.appservice.local.azurestack.external.pfx**|
|App Service identity application certificate| **Sso.appservice.local.azurestack.external.pfx**|

!IMAGE[Lab Image](https://raw.githubusercontent.com/yagmurs/Wplus-AzS/master/screens/lab10-e3-i1.png)

##Task 3: Download Azure Stack root certificate
> In this task, you will you will act as a cloud operator and:
> - Download Azure Stack Root certificate

!INSTRUCTIONS[][rdp-logon]

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and run.
- [] Use +++@lab.VirtualMachine(55267).Password+++ when password prompted.

```PowerShell
#Run following PowerShell command to download root certificate 
$defaultLocalPath = "C:\AzureStackonAzureVM"
Cd \
Cd $defaultLocalPath\AppServiceHelperScripts
.\Get-AzureStackRootCert.ps1 -PrivilegedEndpoint AzS-ERCS01 -CloudAdminCredential (Get-Credential -Credential AZURESTACK\CloudAdmin)
```

> [!KNOWLEDGE] **Get-AzureStackRootCert.ps1** script requests Azure Resource Manager root certificate for Azure Stack from Priviledged EndPoint.

> [!KNOWLEDGE] Following file will be used on App service installation on upcoming exercises

**Following root certificate will be downloaded**
> [!NOTE] 
|||
|:---|:---|
|Azure Stack Certificate Authority Root certificate| **AzureStackCertificationAuthority.cer**|

##Task 4: Register an Azure Active Directory application ID
> In this task, you will you will act as a cloud operator and:
> - Create an Azure Active Directory application ID and grant permission to be used by App Service on Azure Stack

!INSTRUCTIONS[][rdp-logon]

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and replace **< >** fields between brackets with the values represent your environment.

```PowerShell
#Run following PowerShell register Azure AD Application ID
$defaultLocalPath = "C:\AzureStackonAzureVM"
Cd \
Cd $defaultLocalPath\AppServiceHelperScripts
 
# The service admin account (Azure AD)
$serviceAdmin = "@lab.UserFirstName@<\tenant_name>.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "@lab.VirtualMachine(55267).Password" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)
 
.\Create-AADIdentityApp.ps1 -DirectoryTenantName <\tenant_name>.onmicrosoft.com -AdminArmEndpoint adminmanagement.local.azurestack.external -TenantArmEndpoint management.local.azurestack.external -CertificateFilePath $defaultLocalPath\AppServiceHelperScripts\sso.appservice.local.azurestack.external.pfx -CertificatePassword ("@lab.VirtualMachine(55267).Password" | ConvertTo-SecureString -AsPlainText -Force) -AzureStackAdminCredential $AdminCreds
```

> [!KNOWLEDGE] This script creates a new application ID in Azure Active Directory. Make **note the application ID** that's returned in the PowerShell output.

- [] The output should look like:

!IMAGE[Lab Image](https://raw.githubusercontent.com/yagmurs/Wplus-AzS/master/screens/lab10-e3-i5.png)

> [!KNOWLEDGE] Application ID registered in this task will be used on App service installation on upcoming exercises

##Task 5: Grant permission to Azure Active Directory application ID
> In this task, you will you will act as a cloud operator and:
> - Grant permission to Azure Active Directory application ID

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azure-portal-logon]

- [] In the @[Azure Portal][azure-portal], click **More services**, and then **Azure Active Directory**
- [] On the **Azure Active Directory** blade, click **App Registrations**
- [] On the **- App registrations** blade, select **All apps** type to search application ID previously noted and select **App Service**

!IMAGE[Lab Image](https://raw.githubusercontent.com/yagmurs/Wplus-AzS/master/screens/lab10-e3-i2.png)

- [] On the **Application ID** blade, click **Settings**
- [] On the **Settings** blade, click **Required permissions**

!IMAGE[Lab Image](https://raw.githubusercontent.com/yagmurs/Wplus-AzS/master/screens/lab10-e3-i3.png)

- [] On the **Required permissions** blade, click **Grant permissions** then click **Yes**
- [] Following confimation message will be displayed

!IMAGE[Lab Image](https://raw.githubusercontent.com/yagmurs/Wplus-AzS/master/screens/lab10-e3-i4.png)

> **Result:**
> After completing this exercise, you have requested and downloaded required certificates and root certificates, registered Azure Active Directory Application ID for App Service installation Azure Stack

---

Click **Next** to advance to [**LAB 10 - Exercise 4**](#lab10e4) window

---

[**Table of content**](#toc)

---

===
######lab10e4

[**Table of content**](#toc)

---

#Lab 10 - Exercise 4 - 
> In this exercise, you will act as a cloud operator and:
> - Download and extract the App Service on Azure Stack deployment helper scripts.
> - Download the App Service on Azure Stack installer.

##Task 1: 
> In this task, you will you will act as a cloud operator and:
> - Download and extract the App Service on Azure Stack deployment helper scripts.

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]
!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and run.

```PowerShell
#Download and extract AppServiceHelperScripts
$defaultLocalPath = "C:\AzureStackonAzureVM"
Invoke-WebRequest -Uri "https://aka.ms/appsvconmashelpers" -OutFile "$defaultLocalPath\AppServiceHelperScripts.zip"
Expand-Archive -Path "$defaultLocalPath\AppServiceHelperScripts.zip" -DestinationPath "$defaultLocalPath\AppServiceHelperScripts"
```

##Task 2: 
> In this task, you will you will act as a cloud operator and:
> - Download the App Service on Azure Stack installer.

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]
!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and run.

```PowerShell
#Download AppService
$defaultLocalPath = "C:\AzureStackonAzureVM"
Invoke-WebRequest -Uri "https://aka.ms/appsvconmasinstaller" -OutFile "$defaultLocalPath\AppService.exe"
```

> **Result:**
> After completing this exercise, you have downloaded and extracted the App Service installer and helper scripts before you start installing App Service installation on Azure Stack Development Kit

---

Click **Next** to advance to [**LAB 10 - Exercise 5**](#lab10e5) window

---

[**Table of content**](#toc)

---

===
######lab10e5

[**Table of content**](#toc)

---

#Lab 10 - Exercise 5 - 
> In this exercise, you will act as a cloud operator and:
> - Download and extract the App Service on Azure Stack deployment helper scripts.
> - Download the App Service on Azure Stack installer.

##Task 1: 
> In this task, you will you will act as a cloud operator and:
> - Download and extract the App Service on Azure Stack deployment helper scripts.

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]
!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and run.

```PowerShell
#Download and extract AppServiceHelperScripts
$defaultLocalPath = "C:\AzureStackonAzureVM"
Invoke-WebRequest -Uri "https://aka.ms/appsvconmashelpers" -OutFile "$defaultLocalPath\AppServiceHelperScripts.zip"
Expand-Archive -Path "$defaultLocalPath\AppServiceHelperScripts.zip" -DestinationPath "$defaultLocalPath\AppServiceHelperScripts"
```

##Task 2: 
> In this task, you will you will act as a cloud operator and:
> - Download the App Service on Azure Stack installer.

!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-tenantportal-tenantuser-logon]
!INSTRUCTIONS[][rdp-logon]
!INSTRUCTIONS[][azurestack-adminportal-globaladmin-logon]

- [] Open PowerShell ISE **Run as an Administrator**. Copy following script to Powershell ISE and run.

```PowerShell
#Download AppService
$defaultLocalPath = "C:\AzureStackonAzureVM"
Invoke-WebRequest -Uri "https://aka.ms/appsvconmasinstaller" -OutFile "$defaultLocalPath\AppService.exe"
```

> **Result:**
> After completing this exercise, you have downloaded and extracted the App Service installer and helper scripts before you start installing App Service installation on Azure Stack Development Kit

---

Click **Next** to advance to [**LAB 11 - Objectives and Summary**](#lab11) window

---

[**Table of content**](#toc)

---

===
####References
[lab01-e1-rl]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-deploy
[lab01-e2-rl]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-deploy
[lab01-e4-rl1]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-powershell-install 
[lab01-e4-rl2]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-powershell-download
[lab01-e4-rl3]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-powershell-configure-admin
[lab01-e5-rl]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-register
[lab02-e1-rl1]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-marketplace-azure-items
[lab02-e1-rl2]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-add-default-image
[lab10-e2-rl1]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-app-service-before-you-get-started

===

####Links

@[Azure Portal][azure-portal]

[Azure Stack Admin Portal][azurestack-adminportal]

[Azure Stack Tenant Portal][azurestack-tenantportal]

@[Register Azure Pass][azure-pass]

@[RDP][rdp]

[azure-portal]:
```
start microsoft-edge:https://portal.azure.com
```

[azurestack-adminportal]:https://adminportal.local.azurestack.external "https://adminportal.local.azurestack.external"
[azurestack-tenantportal]:https://portal.local.azurestack.external "https://portal.local.azurestack.external"
[azure-pass]:
```
start microsoft-edge:https://www.microsoftazurepass.com/SubmitPromoCode "https://www.microsoftazurepass.com/SubmitPromoCode"
```

[rl1]:
```
start microsoft-edge:https://signup.live.com/?wa=wsignin1.0&rpsnv=13&ct=1518075390&rver=6.7.6643.0&wp=MBI_SSL&wreply=https:%2f%2faccount.microsoft.com%2fauth%2fcomplete-signin%3fru%3dhttps%253a%252f%252faccount.microsoft.com%252f%253frefp%253dsignedout-index%2526refd%253dwww.google.com.tr&id=292666&lw=1&fl=easi2&contextid=0DED367F3FADAAB6&bk=1518075545&uiflavor=web&uaid=2c5d00f6f13e46c0aab951ad21af64d8&mkt=EN-US&lc=1033
```

[github-repo]:
```
start microsoft-edge:https://github.com/yagmurs/AzureStack-VM-PoC "https://github.com/yagmurs/AzureStack-VM-PoC"
```

[github-repo-deploytoazure]
```
start microsoft-edge:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fyagmurs%2FAzureStack-VM-PoC%2Fmaster%2Fazuredeploy.json
```

[rdp]:
```
mstsc.exe
```

[github-azurestackquicktemplates]:https://github.com/Azure/AzureStack-QuickStart-Templates "https://github.com/Azure/AzureStack-QuickStart-Templates"

>[variables]:
> The password you assigned during ARM Template deployment on [Lab 01 - Exercise 1](#lab01e1) to **Administrator** are also used for *AZURESTACK\AzureStackAdmin*, and *AZURESTACK\CloudAdmin* on Azure Stack development kit. These are AZURESTACK domain identities that may be used to logon Azure VM.
>
> Following scheme <\ > stands for a replacement with the values represent your environment.
>
>|||
>|:---|:---|
>|**Azure Portal**|**:** +++https://portal.azure.com/+++|
>|**Azure Stack Admin Portal**|**:** +++https://adminportal.local.azurestack.external/+++|
>|**Azure Stack Tenant Portal**|**:** +++https://portal.local.azurestack.external/+++|
>|**Azure Stack Tenant Portal**|**:** +++https://portal.local.azurestack.external/+++|
>|**Password**|**:** +++**@lab.VirtualMachine(55267).Password**+++|
>|**Azure Stack Host VM**|**:** **<\the VM deployed on Lab 01 - Exercise 1>**|

>[azurestack-adminportal-globaladmin-logon]:
>- [] Logon [Azure Stack Admin Portal][azurestack-adminportal] using following information;
>
>    |||
>    |---:|:---|
>    |**Username** |**:** +++@lab.UserFirstName@<\tenant_name>.onmicrosoft.com+++
>    |**Password** |**:** +++@lab.VirtualMachine(55267).Password+++

>[azurestack-tenantportal-globaladmin-logon]:
>- [] Within Azure Stack host VM (Azs-Host1), open a browser
>
>- [] Logon [Azure Stack Tenant Portal][azurestack-tenantportal] using following information;
>
>    |||
>    |---:|:---|
>    |**Username** |**:** +++@lab.UserFirstName@<\tenant_name>.onmicrosoft.com+++
>    |**Password** |**:** +++@lab.VirtualMachine(55267).Password+++

>[azurestack-tenantportal-tenantuser-logon]:
>- [] Within Azure Stack host VM (Azs-Host1), open a browser **(InPrivate/Incognito)**
>
>- [] Logon [Azure Stack Tenant Portal][azurestack-tenantportal] using following information;
>
>    |||
>    |---:|:---|
>    |**Username** |**:** +++tenantuser@<\tenant_name>.onmicrosoft.com+++
>    |**Password** |**:** +++@lab.VirtualMachine(55267).Password+++

>[azurestack-tenantportal-delegatedprovider-logon]:
>- [] Within Azure Stack host VM (Azs-Host1), open a browser **(InPrivate/Incognito)**
>
>- [] Logon [Azure Stack Tenant Portal][azurestack-tenantportal] using following information;
>
>    |||
>    |---:|:---|
>    |**Username** |**:** +++delegatedprovider@<\tenant_name>.onmicrosoft.com+++
>    |**Password** |**:** +++@lab.VirtualMachine(55267).Password+++

>[rdp-logon]:
>- [] Logon Azure Stack host VM with @[RDP][rdp] client using following information;
>
>    |||
>    |---:|:---|
>    |**Username** |**:** +++AZURESTACK\AzureStackAdmin+++
>    |**Password** |**:** +++@lab.VirtualMachine(55267).Password+++

>[azure-portal-logon]:
>- [] Logon @[Azure Portal][azure-portal] using following information;
>
>    |||
>    |---:|:---|
>    |**Username** |**:** +++@lab.UserFirstName@<\tenant_name>.onmicrosoft.com+++|
>    |**Password** |**:** +++@lab.VirtualMachine(55267).Password+++

>[azurestackadmin-user]:
>    |||
>    |---:|:---|
>    |**Username** |**:** +++AZURESTACK\AzureStackAdmin+++
>    |**Password** |**:** +++@lab.VirtualMachine(55267).Password+++

>[cloudadmin-user]:
>    |||
>    |---:|:---|
>    |**Username** |**:** +++AZURESTACK\CloudAdmin+++
>    |**Password** |**:** +++@lab.VirtualMachine(55267).Password+++
