# Quasar Garages

Inside of `config-cl.lua` Replace the existing top event (`jg-dealerships:client:purchase-vehicle:config`) with the following code:

```lua
RegisterNetEvent("jg-dealerships:client:purchase-vehicle:config", function(vehicle, plate, purchaseType, amount, paymentMethod, financed)
  local vhClass = GetVehicleClass(vehicle)
  
  local vhType = "car"
  if vhClass == 14 then vhType = "boat" end
  if vhClass == 15 or vhClass == 16 then vhType = "airplane" end

  TriggerServerEvent("jg-dealerships:server:qsgarages-type-update", vhType, plate)
end)
```

and then inside of `config-sv.lua` add this new code, don't replace anything:

```lua
RegisterNetEvent("jg-dealerships:server:qsgarages-type-update", function(vhType, plate)
  MySQL.update.await("UPDATE owned_vehicles SET type = ? WHERE plate = ?", {vhType, plate})
end)
```
