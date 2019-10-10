---

---
# Azure Infrastructure and Deployment plan

### Login to Azure Account

Open **PowerShell**

Login to azure account using **Add-AzureAccount**

    PS> Add-AzureAccount

Select a subscription to be the current one using **Select-AzureSubscription**

    PS> Select-AzureSubscription -Current -SubscriptionName Pay-As-You-Go

Validate that the subscription selected is now the default and current one using **Get-AzureSubscription**

    PS> Get-AzureSubscription

**Step 1:** [Create Resource Group](https://devexpresso.github.io/Azure/azureinfradeploy/createresourcegroup "Create Resource Group")