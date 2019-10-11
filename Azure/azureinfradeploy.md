---

---
# Azure Infrastructure and Deployment plan

### Login to Azure Account

Open **PowerShell**

Install Azure PowerShell using **Install-Module** command

    Install-module -Name Az -AllowClobber -Scope AllUsers

Login to azure account using **Add-AzureAccount**

    Add-AzureAccount

Get all Subscriptions available using **Get-AzSubscription**

    Get-AzSubscription

Set default context using **Set-AzContext**

    Set-AzContext -SubscriptionId "Subscription Id"

Verify that the default Subscription is selected using **Get-AzSubscription**

    Get-AzSubscription

**Step 1:** [Create Resource Group](https://devexpresso.github.io/Azure/azureinfradeploy/createresourcegroup "Create Resource Group")

**Step 2:** [Create Virtual Network](https://devexpresso.github.io/Azure/azureinfradeploy/createvnet "Create Virtual Network")

**Step 3:** [Create Availability Set](https://devexpresso.github.io/Azure/azureinfradeploy/availabilityset "Create Availability Set")