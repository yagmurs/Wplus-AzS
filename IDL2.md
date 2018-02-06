#WorkshopPLUS – Architecting Hybrid Cloud Solutions – Azure Stack
#Introduction
Blah blah
blah

===
#Lab 01
> [!KNOWLEDGE] knowledge text here

*@lab.UserEmail*

*@lab.VirtualMachine(55059).Username*

*@lab.VirtualMachine(55059).Password*

^[**Objectives and Summary**][OS1]

> [OS1]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===
#Lab 02
> [!ALERT] alert text here.

^[**Objectives and Summary**][OS2]

> [OS2]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===
#Lab 03

^[**Objectives and Summary**][OS3]

> [OS3]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===
#Lab 04

^[**Objectives and Summary**][OS4]

> [OS4]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===
#Lab 05

^[**Objectives and Summary**][OS5]

> [OS5]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===
#Lab 06
^[**Objectives and Summary**][OS6]

> [OS6]:
##Introduction
This Lab covers enabling, configuring, and manually scaling Virtual Machine scale sets to be used in Azure Stack.
##Objectives  
In this lab, you will: 
Enable Virtual Machine scale sets feature, 
Connect to Azure Stack as a tenant, 
Deploy a Virtual Machine scale set, 
Manually scale up Virtual Machine scale set 
##Prerequisites   
You must complete Lab 07: Plans & Offers before you can start this lab. 
Environment 
Admin Portal Address: https://adminportal.local.azurestack.external 
Tenant Portal Address: https://portal.local.azurestack.external 
Cloud Operator Account: **Tenant Alias**@**AAD Tenant**.onmicrosoft.com 
Tenant User Account: T1U1@**AAD Tenant**.onmicrosoft.com 
##Variables 
Change **AAD Tenant** with your Tenant ID.  
Change **Tenant Alias** with your Tenant Administrator alias.
**Password**: Virtual Machine Local Administrator Password
**Subscription**: Subscription for the Azure Stack tenant user account 
##Estimated time to complete this lab  
60 minutes 
##Scenario  
Enable Virtual Machine Scale Set feature in Azure Stack using PowerShell via Cloud Admin account. Afterwards logon to your tenant and create a virtual machine scale set and then manually scale it. 

##Exercices
####Exercise 1: Enable Virtual Machine Scale Sets 
- [] Open PowerShell 
- [] Run below command to connect to Azure Stack as Cloud Admin and enable VMSS. (replace the fields between <> with required information)
```Powershell
#Variables 
$defaultLocalPath = "C:\AzureStackOnAzureVM" 
$location = "local" 
 
$AadAdmin = "<Tenant Alias>@<AAD Tenant>.onmicrosoft.com" 
$AadTenant = "<AAD Tenant>.onmicrosoft.com" 
  
# Create Azure AD credential object 
$AadAdminPass = ConvertTo-SecureString "<Azure AD Admin Password>" -AsPlainText -Force 
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
This will enable Virtual Machine scale sets on your Azure Stack deployment. 
###Exercise 2: Deploy a virtual machine scale sets 
- [] Log on to Azure Stack Tenant Portal using T1U1@<AAD Tenant>.onmicrosoft.com 
- [] Click New > Compute > Virtual Machine scale set
- [] Step 1: Fill in the form 
    - Virtual machine scale set name: rg01vmss 
    - OS type: select Windows 
    - User name: localadm 
    - Password: <Password> 
    - Subscription: <Subscription> 
    - Resource group: select Create new 
    - Resource group name: rg01 
    - Location: local 
- [] Click **OK** 
- [] Step 2: Fill in the form
    - Click on Public IP address 
    - Change name to: rg01ip01 
    - Make sure Dynamic is selected 
- [] Click **OK** 
    - Domain name label: testapp01 
    - Operating system disk image: 2016-Datacenter 
    - Instance count: 1 
    - Scale set virtual machine size: 1x Standard D1 
- [] Click **OK** 
- [] Step 3: Summary

    > [!NOTE] Make sure all information is correct

    > [!ALERT] Make sure all information is correct

- [] Click **OK** 
In the Azure Portal click on Resource Groups 
- [] Select **rg01**
- [] Click on **Deployments**
    - Wait for deployment to finish, this will take approximately 15 minutes. 
- [] Validate that Windows machine is deployed, to validate the VM you can open a remote desktop session. Because of the default load balancer rules port 50000 on the Load Balancer will be mapped to the first VM in the scale set on port 3389. 
- [] Open **Start Menu**
- [] Search for Remote Desktop Connection and connect to testapp01.local.cloudapp.azurestack.external:50000
###Exercise 3 Configure PowerShell Environment for Tenant 
- [] Open a new PowerShell session
```Powershell
# Variables 
$defaultLocalPath = "C:\AzureStackOnAzureVM" 
$location = "local" 
 
cd\ 
cd $defaultLocalPath 
cd AzureStack-Tools-master 
 
Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1 
Import-Module .\Connect\AzureStack.Connect.psm1 
$AadTenant = <AAD Tenant>.onmicrosoft.com 
$ArmEndpoint =  "https://management.local.azurestack.external" 
$KeyvaultDnsSuffix = “vault.local.azurestack.external” 
 
# Register an AzureRM environment that targets your Azure Stack instance 
Add-AzureRMEnvironment -Name "AzureStackTenant" -ArmEndpoint $ArmEndpoint 
 
Set-AzureRmEnvironment -Name "AzureStackTenant" -GraphAudience $GraphAudience  
 
# Get the Active Directory tenantId that is used to deploy Azure Stack 
$TenantID = Get-AzsDirectoryTenantId -AADTenantName $AadTenant -EnvironmentName "AzureStackTenant" 
 
# Sign in to your environment 
Login-AzureRmAccount -EnvironmentName "AzureStackTenant" -TenantId $TenantID 
```
```PowerShell
Get-Item 
```


When asked for credentials logon using T1U1@<AAD Tenant>.onmicrosoft.com account
###Exercise 4 Manually scale up VMs using PowerShell 
- [] Using the same PowerShell session created in Exercise 3 
- [] Run below commands
```Powershell
$vmss = Get-AzureRmVmss -ResourceGroupName rg01 
$vmss = Get-AzureRmVmss -ResourceGroupName rg01 -VMScaleSetName $vmss[0].Name 
 
$vmss.sku.Capacity = 2 
Update-AzureRmVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss 
```
- [] Log on to Azure Stack Tenant Portal 
- [] In the Azure Portal click on Resource Groups 
- [] Select **rg01**
- [] Select the resource with Virtual Machine Scale Set type from the list of resources 
- [] Click on **Instances** 
- [] You should see 1 instance running and another one being Created. 
- [] Wait for deployment to finish, this will take approximately 15 minutes. 
- [] Validate that Windows machine is deployed, to validate the VM you can open a remote desktop session. Because of the default load balancer rules port 50001 on the Load Balancer will be mapped to the second VM in the scale set on port 3389. 
- [] Open Start Menu 
- [] Search for Remote Desktop Connection and connect to testapp01.local.cloudapp.azurestack.external:50001 

===
#Lab 07

^[**Objectives and Summary**][OS7]

> [OS7]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===
#Lab 08

^[**Objectives and Summary**][OS8]

> [OS8]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===
#Lab 09

^[**Objectives and Summary**][OS9]

> [OS9]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===
#Lab 10

^[**Objectives and Summary**][OS10]

> [OS10]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===
#Lab 11

^[**Objectives and Summary**][OS11]

> [OS11]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===
#Lab 12

^[**Objectives and Summary**][OS12]

> [OS12]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===
#Lab 13

^[**Objectives and Summary**][OS13]

> [OS13]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===
#Lab 14

^[**Objectives and Summary**][OS14]

> [OS14]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===

#Lab x

^[**Objectives and Summary**][OSx]

> [OSx]:
##Objectives
##Exercices
##Prerequisites   
##Variables 
##Estimated time to complete this lab  
##Scenario

===