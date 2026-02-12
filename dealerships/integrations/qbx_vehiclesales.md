---
description: Make qbx_vehiclesales compatible with our Dealerships V2
---

# qbx\_vehiclesales

### Go to file qbx\_vehiclesales/server/main.lua`.lua`

&#x20;replace line 38-48 with the code block below

```lua
lib.callback.register('jg-advancedgarages:server:checkVehicleOwner', function(source, plate)
    local player = exports.qbx_core:GetPlayer(source)
    local result = MySQL.single.await('SELECT * FROM player_vehicles WHERE plate = ? AND citizenid = ?', {plate, player.PlayerData.citizenid})

    if result and result.id then
        local financeRow = MySQL.single.await('SELECT * FROM vehicle_financing WHERE vehicleId = ?', {result.id})
        if financeRow and financeRow.balance > 0 then
            exports.qbx_core:Notify(source, "You cannot sell a vehicle that is under finance", 'error')
            return false
        end
        return true, financeRow?.balance or 0
    end

    return false
end)
```

### Go to file `qbx_vehiclesales/client/main.lua`

Replace`qbx_vehiclesales:server:checkVehicleOwner`with`jg-advancedgarages:server:checkVehicleOwner`(there is 2 occurences of this you will need to change or you could do a find and replace in your text editor!), like in the screenshot below:&#x20;

<figure><img src="https://r2.fivemanage.com/image/Cs4lMQ2oKcNX.png" alt=""><figcaption></figcaption></figure>
