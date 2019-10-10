---

---
# Azure Infrastructure and Deployment plan

### Create Azure Resource Group

Open **PowerShell**

Login to azure account using **Add-AzureAccount**

    PS> Add-AzureAccount

Select a subscription to be the current one using **Select-AzureSubscription**

    PS> Select-AzureSubscription -Current -SubscriptionName Pay-As-You-Go

Validate that the subscription selected is now the default and current one using **Get-AzureSubscription**

    PS> Get-AzureSubscription

Use **New-AzureRmResourceGroup** cmdlet to create a new Azure resource group

    PS> New-AzureRmResourceGroup -Name DemoAppRG01 -Location "South Central US"

Validate that the resource group has been created successfully using **Get-AzureRmResourceGroup**

    PS> Get-AzureRmResourceGroup