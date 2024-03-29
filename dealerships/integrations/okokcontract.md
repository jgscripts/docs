---
description: to integrate finance check into okokcontracts
---

# okokContract

{% hint style="danger" %}
Please note you will need to change the table name depending on your framework\
\
If ESX then table name is "owned\_vehicles"\
\
If QBCORE then table name is "player\_vehicles"
{% endhint %}

```lua
function canVehicleBeSold(source, plate)
    local canBeSold = true
    local financed = MySQL.scalar.await('SELECT `financed` FROM `owned_vehicles` WHERE `plate` = ? LIMIT 1', { plate })
 
    if financed then
        TriggerClientEvent('okokNotify:Alert', source, "NO TRANSFER", "You cannot transfer a vehicle that is financed", 5000, 'error')
    elseif not financed then
       return canBeSold
    end
end
```
