---
description: Make qb-vehiclesales compatible with our Dealerships
---

# qb-vehiclesales

### Go to file `qb-vehiclesales/server/main.lua`

&#x20;add this snippet at the bottom of the script

```lua
QBCore.Functions.CreateCallback("jg-advancedgarages:server:checkVehicleOwner", function(source, cb, plate)
    local src = source
    local pData = QBCore.Functions.GetPlayer(src)
    local financed = MySQL.scalar.await('SELECT `financed` FROM `player_vehicles` WHERE `plate` = ? LIMIT 1', { plate })
    if not financed then
        MySQL.query('SELECT * FROM player_vehicles WHERE plate = ? AND citizenid = ?',{plate, pData.PlayerData.citizenid}, function(result)
            if result[1] then
                cb(true, result[1].balance)
            else
                cb(false)
            end
        end)
    elseif financed then
        TriggerClientEvent('QBCore:Notify', src, "you cannot sell a vehicle that is under finance", 'error')
    end
end)
```

### Go to file `qb-vehiclesales/client/main.lua`

Replace`qb-garage:server:checkVehicleOwner`with`jg-advancedgarages:server:checkVehicleOwner`(there is 2 occurences of this you will need to change or you could do a find and replace in your text editor!), like in the screenshot below:&#x20;

<figure><img src="../../.gitbook/assets/qb-vehiclesales-snippet (1).png" alt=""><figcaption></figcaption></figure>
