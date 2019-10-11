---

---
# Create VNet

#### Create Virtual Network

Use the command **New-AzVirtualNetwork**

    $vNet = New-AzVirtualNetwork -ResourceGroupName DemoAppRG01 -Location "South Central US" -Name DemoVnet01 -AddressPrefix 10.0.0.0/16

#### Create a Subnet

Using the command **Add-AzVirtualNetworkSubnetConfig**

    $subNetConfig = Add-AzVirtualNetworkSubnetConfig -Name DemoSubnet01 -VirtualNetwork $vNet -AddressPrefix 10.0.0.0/24

#### Associate Subnet to VNet

Use the command

     $vNet | Set-AzVirtualNetwork

[Back to main content](https://devexpresso.github.io/Azure/azureinfradeploy "Back to main content")