---

---
# Azure Infrastructure and Deployment plan

### Login to Azure Account

Open **PowerShell**

Install Azure PowerShell using **Install-Module** command

    Install-module -Name Az -AllowClobber -Scope AllUsers

Login to azure account using **Add-AzureAccount**

    PS> Add-AzureAccount

Get all Subscriptions available using **Get-AzSubscription**

    PS> Get-AzSubscription

Set default context using **Set-AzContext**

    PS> Set-AzContext -SubscriptId "Subscription Id"

Verify that the default Subscription is selected using **Get-AzSubscription**

    PS> Get-AzSubscription

**Step 1:** [Create Resource Group](https://devexpresso.github.io/Azure/azureinfradeploy/createresourcegroup "Create Resource Group")

**Step 2:** [Create Virtual Network](https://devexpresso.github.io/Azure/azureinfradeploy/createvnet "Create Virtual Network")