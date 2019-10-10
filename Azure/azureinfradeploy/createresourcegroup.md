---

---
# Create Azure Resource Group

Use **New-AzureRmResourceGroup** cmdlet to create a new Azure resource group

    PS> New-AzureRmResourceGroup -Name DemoAppRG01 -Location "South Central US"

Validate that the resource group has been created successfully using **Get-AzureRmResourceGroup**

    PS> Get-AzureRmResourceGroup