---
description: Garage trigger events for custom integrations
---

# Events

{% hint style="warning" %}
Note: `GARAGE_ID` is the name of your garage in the `config.lua`
{% endhint %}

```lua
-- Open garage (personal garages)
TriggerEvent("jg-advancedgarages:client:ShowGarage", "GARAGE_ID")

-- Insert vehicle (personal garages)
-- If using this for houses set to true
TriggerEvent("jg-advancedgarages:client:InsertVehicle", "GARAGE_ID", false)

-- Open House Garage
TriggerEvent('jg-advancedgarages:client:ShowHouseGarage', "GARAGE_ID")

-- Open garage (job garages)
TriggerEvent("jg-advancedgarages:client:ShowJobGarage", "JOB_GARAGE_ID")

-- Insert vehicle (job garages)
TriggerEvent("jg-advancedgarages:client:JobGarageInsertVehicle", "JOB_GARAGE_ID")

-- Open garage (gang garages, QBCore only)
TriggerEvent("jg-advancedgarages:client:ShowGangGarage", "GANG_GARAGE_ID")

-- Insert vehicle (gang garages, QBCore only)
TriggerEvent("jg-advancedgarages:client:GangGarageInsertVehicle", "GANG_GARAGE_ID")

-- Open impound
TriggerEvent("jg-advancedgarages:client:ShowImpound", "IMPOUND_ID")

-- Impound vehicle
TriggerEvent("jg-advancedgarages:client:ImpoundVehicle")
```
