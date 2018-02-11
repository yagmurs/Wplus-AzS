#WorkshopPLUS – Architecting Hybrid Cloud Solutions – Azure Stack
#Introduction
> [!ALERT] Introduction Information will be completed

===
#Lab 01 - Deploy Azure Stack using Azure Active Directory

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
>>> [!ALERT]Azure Pass account has to be registered and ready to use.
>
> ###Variables
>>
>
> ###Estimated time to complete this lab
>> **6 Hours**
>
> ###Scenario
>> Deploy Azure Stack Development Kit on an Azure VM and register to Azure on **connected** mode. 

##Exercises
###Exercise #1 - Deploy Azure Stack host using ARM Template
- [] Logon [Azure Portal][azure-portal]
- [] [*Register Azure Pass*][azure-pass]
- [] On main menu click **More services** > **Azure Active Directory**
- [] On Quick tasks click **Add a user**.

Specify the following settings.
> - Name: **@lab.UserFirstName**
> - User name: **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com**
> - Profile: **Not configured**
> - Directory Role: **Global administrator**
> - Password: check **Show Password**
> - **Copy** the password to **clipboard**

- [] Click **Create**

> [!KNOWLEDGE] Tenant user now created, needs to be the **Owner** for the Azure subscription.

- [] On main menu click **More services** > **Subscriptions**
- [] On the subscriptions blade click on your **Azure Pass** Subscription
- [] On the Azure Pass blade click **Access control (IAM)** > **+Add**

On the Add permissions blade, Specify the following settings.
> - Role: **Owner**
> - Assign Access to: **Azure AD user,group or application**
> - Select: **@lab.UserFirstName**

- [] User should be listed. Click on user to add user to Selected members
- [] Click **Save**

- [] Sign out from current session.
- [] Logon [Azure Portal][azure-portal] with **@lab.UserFirstName@<\tenant_name>.onmicrosoft.com** and use the temporary password previously saved.
- [] You will be prompted to change the password.
- [] Enter previous password and **new password** twice. Click **OK**

    > [!ALERT] This will make sure you have changed the temporary password

> [!KNOWLEDGE] We now have the necessary Azure AD tenant directory to use on Azure Stack installation.

> [!KNOWLEDGE] Until this point we will start deploying Azure VM to host Azure Stack Development Kit.

- [] Open up new browser and go to https://github.com/yagmurs/AzureStack-VM-PoC
- [] Click **Deploy to Azure** button to deploy Azure Stack host template to Azure.
- [] Browser will be redirected to your <\tenant name>.onmicrosoft.com portal with ARM template settings to be completed. 
- [] On "Customized template" blade > "Resource Group"
- [] Click **Create New**

On the Custom deployment blade, Specify the following settings.
> - Virtual Machine Name: **AzS-Host1**
> - Public DNS Name: **Any name that does not conflicts**
> - Region: **<\Any region that is closer to your location>**
> - Resource Group: **AzureStackPremierWorkshop**
> - Admin Password: **<\Local Administrator password to be used to logon to Azure VM>**

- [] Check TERMS AND CONDITIONS and click on **Purchase**.

> [!NOTE] VM installation takes about 10 to 20 minutes depends on the region and load.

[Reference Document][lab01-e1-rl]

- [] *Exercise #2*

[Reference Document][lab01-e2-rl]

- [] *Exercise #3*

[Reference Document][lab01-e3-rl]

- [] *Exercise #4*

[Reference Document][lab01-e4-rl]

- [] *Exercise #5*

[Reference Document][lab01-e5-rl]

- [] *Exercise #6*

[Reference Document][lab01-e6-rl]

- [] *Exercise #7*

[Reference Document][lab01-e7-rl]


===
## References
#### Lab References
[lab01-e1-rl]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-deploy
[lab01-e2-rl]:https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-deploy

#### variables


#### Images


#### Links
[Azure Portal][azure-portal]

[Azure Stack Admin Portal][azurestack-adminportal]

[Azure Stack Tenant Portal][azurestack-tenantportal]

[Register Azure Pass][azure-pass]

[azure-portal]:https://portal.azure.com "https://portal.azure.com/"
[azurestack-adminportal]:https://adminportal.local.azurestack.external "https://adminportal.local.azurestack.external"
[azurestack-tenantportal]:https://portal.local.azurestack.external "https://portal.local.azurestack.external"
[azure-pass]:https://www.microsoftazurepass.com/SubmitPromoCode "https://www.microsoftazurepass.com/"

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

##Exercises
###Exercise #1

[Reference Document][labxx-e1-rl]

###Exercise #2

[Reference Document][labxx-e2-rl]

###Exercise #3

[Reference Document][labxx-e3-rl]

###Exercise #4

[Reference Document][labxx-e4-rl]

###Exercise #5

[Reference Document][labxx-e5-rl]

###Exercise #6

[Reference Document][labxx-e6-rl]

###Exercise #7

[Reference Document][labxx-e7-rl]

===


===
