---

---
# Create VNet

#### Create Virtual Network

Use the command **New-AzVirtualNetwork**

     $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName DemoAppRG01 -Location "South Central US" -Name DemoAppVNet01 -AddressPrefix 10.0.0.0/16

#### Create a Subnet

Using the command **Add-AzVirtualNetworkSubnetConfig**

    $SubNetConfig = Add-AzVirtualNetworkSubnetConfig -Name DemoAppSubNet01 -VirtualNetwork $virtualNetwork -AddressPrefix 10.0.0.0/24

#### Associate Subnet to VNet

Use the command 

     $virtualNetwork | Set-AzVirtualNetwork