# Custom Key System

If we don't support your key system out of the box, it's super easy to add your own!

1. Go to the `framework` folder, and open `cl-functions.lua`
2. Find the `VehicleGiveKeys` and `VehicleRemoveKeys` functions, which should look something like this:

```lua
function Framework.Client.VehicleGiveKeys(plate, vehicleEntity)
  if not DoesEntityExist(vehicleEntity) then return false end

  if Config.VehicleKeys == "qb-vehiclekeys" then
    TriggerEvent("vehiclekeys:client:SetOwner", plate)
  elseif Config.VehicleKeys == "jaksam-vehicles-keys" then
    TriggerServerEvent("vehicles_keys:selfGiveVehicleKeys", plate)
  elseif Config.VehicleKeys == "mk_vehiclekeys" then
    exports["mk_vehiclekeys"]:AddKey(vehicleEntity)
  elseif Config.VehicleKeys == "qs-vehiclekeys" then
    local model = GetDisplayNameFromVehicleModel(GetEntityModel(vehicleEntity))
    exports['qs-vehiclekeys']:GiveKeys(plate, model)
  elseif Config.VehicleKeys == "wasabi_carlock" then
    exports.wasabi_carlock:GiveKey(plate)
  elseif Config.VehicleKeys == "cd_garage" then
    TriggerEvent('cd_garage:AddKeys', plate)
  elseif Config.VehicleKeys == "okokGarage" then
    TriggerServerEvent("okokGarage:GiveKeys", plate)
  elseif Config.VehicleKeys == "t1ger_keys" then
    TriggerServerEvent('t1ger_keys:updateOwnedKeys', plate, true)
  else
    -- Setup custom key system here...
  end
end

function Framework.Client.VehicleRemoveKeys(plate, vehicleEntity)
  if not DoesEntityExist(vehicleEntity) then return false end
  
  if Config.VehicleKeys == "qs-vehiclekeys" then
    local model = GetDisplayNameFromVehicleModel(GetEntityModel(vehicleEntity))
    exports['qs-vehiclekeys']:RemoveKeys(plate, model)
  elseif Config.VehicleKeys == "wasabi_carlock" then
    exports.wasabi_carlock:RemoveKey(plate)
  elseif Config.VehicleKeys == "t1ger_keys" then
    TriggerServerEvent('t1ger_keys:updateOwnedKeys', plate, false)
  else
    -- Setup custom key system here...
  end
end
```

3. Inside the `VehicleGiveKeys` function, within the `else` block where it says\
   \
   `return 65 -- Setup custom key system here...`\
   \
   replacing it with your custom **give keys** export. Also make sure you are providing the correct variables to the export based on the ones available:\
   \
   `plate` - Vehicle plate\
   `vehicleEntity` - Physical vehicle entity\
   \
   If you need to get the vehicle model/spawn code, you can use:\
   \
   `local model = GetDisplayNameFromVehicleModel(GetEntityModel(vehicleEntity))`\

4. If your key script has the ability to remove keys, you can do the same inside the `VehicleRemoveKeys` function replacing the line that says\
   \
   `-- Setup custom key system here...`\
   \
   with the **remove keys** export of your key script. Again, ensure you are using variables from the ones available, `plate` and `vehicleEntity`\

5. Make sure that in the Configurator or `config.lua` you have set the VehicleKeys to `none`.
