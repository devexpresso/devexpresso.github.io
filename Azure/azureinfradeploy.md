---

---
# Azure Infrastructure and Deployment plan

### Create Azure Resource Group

Login to azure account using Powershell

Use **New-AzureRmResourceGroup** cmdlet to create a new Azure resource group

    PS> New-AzureRmResourceGroup 
    		-Name DemoAppRG01
            -Location "South Central US"