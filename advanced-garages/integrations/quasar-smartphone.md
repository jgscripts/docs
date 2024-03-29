# Quasar Smartphone

Buy it here: [https://buy.quasar-store.com/package/4861393](https://buy.quasar-store.com/package/4861393)

1. Navigate to `qs-smartphone/config/config.garage.lua`
2. Search (CTRL+F) for `qs-smartphone:server:GetGarageVehicles` - replace this whole Server callback with the following instead:

```lua
QSPhone.RegisterServerCallback('qs-smartphone:server:GetGarageVehicles', function(source, cb)
    if Config.Framework == 'ESX' then 
        local Player = GetPlayerFromIdFramework(source)
        local Vehicles = {}

        MySQL.Async.fetchAll("SELECT * FROM owned_vehicles WHERE owner = '"..Player.identifier.."'", {}, function(result)
            if result[1] ~= nil then
                for k, v in pairs(result) do

                    if v.garage_id == "OUT" then
                        VehicleState = "OUT"
                    else
                        VehicleState = v.garage_id
                    end

                    local vehdata = {}
                    local genData = json.decode(result[k].vehicle)
                    local vehicleInfo = { ['name'] = json.decode(result[k].vehicle).model }

                    vehdata = {
                        model = json.decode(result[k].vehicle).model,
                        plate = v.plate,
                        garage = VehicleState,
                        fuel = genData.fuel or 1000,
                        engine = genData.engine or 1000,
                        body = genData.body or 1000,
                        label = vehicleInfo["name"]
                    }
                    table.insert(Vehicles, vehdata)
                end
                cb(Vehicles)
            else
                cb(nil)
            end
        end)
    elseif Config.Framework == 'QBCore' then 
        local Player = GetPlayerFromIdFramework(source)
        local Vehicles = {}

        MySQL.Async.fetchAll("SELECT * FROM " .. Config.QBCoreFrameworkVehiclesTable .. " WHERE citizenid = '"..Player.identifier.."'", {}, function(result)
            if result[1] ~= nil then
                for k, v in pairs(result) do

                    if v.garage_id == "OUT" then
                        VehicleState = "OUT"
                    else
                        VehicleState = v.garage_id
                    end

                    local vehdata = {}
                    vehdata.props = v.mods
                    vehdata.plate = v.plate
                    vehdata.model = v.vehicle
                    vehdata.garage = VehicleState
                    vehdata.label = v.vehicle

                    table.insert(Vehicles, vehdata)
                end
                cb(Vehicles)
            else
                cb(nil)
            end
        end)
    end
end)
```

... and you're good to go!
