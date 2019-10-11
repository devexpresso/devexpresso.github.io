---

---
# Create Availability Set

Use the command **New-AzAvailabilitySet**

    New-AzAvailabilitySet -Location "South Central US" -ResourceGroup DemoAppRG01 -Name DemoAvailabilitySet01 -Sku aligned -PlatformFaultDomainCount 2 -PlatformUpdateDomainCount 2