# RoadPhone

{% hint style="info" %}
This code was provided to us by the developer of RoadPhone. Please notify RoadPhone so we can work together to get this documentation updated.
{% endhint %}

Buy it here: [https://fivem.roadshop.org/](https://fivem.roadshop.org/)

This integration is to ensure the Valet functionality works with JG Advanced Garages.

***

Replace the contents of `roadphone/server/serverAPI/valet.lua` with the following (make sure you select the correct framework)

{% tabs %}
{% tab title="QBCore" %}
```lua
QBCore.Functions.CreateCallback("roadphone:valet:getCars", function(source, cb)

    local xPlayer = QBCore.Functions.GetPlayer(source)

    if not xPlayer then
        return;
    end

    MySQL.Async.fetchAll("SELECT * FROM " .. Config.OwnedVehiclesTable .. " WHERE `citizenid` = @citizenid and type = @type and `impound` = @impound", {
        ['@impound'] = 0,
        ['@citizenid'] = xPlayer.PlayerData.citizenid,
        ['@type'] = "car"
    }, function(result)
        local cachedvehicles = {}

        for i = 1, #result do
            table.insert(cachedvehicles, {
                plate = result[i].plate,
                vehicle = result[i].vehicle, -- result[i].vehicle,
                type = 'car',
                hash = result[i].hash,
                garage = result[i].garage_id,
                stored = result[i].in_garage
            })

        end

        cb(cachedvehicles)

    end)

end)

QBCore.Functions.CreateCallback('roadphone:valet:loadVehicle', function(source, cb, plate)

    local valetCheck = valetServerSideCheck(plate)

    if valetCheck ~= false then
        cb(false, valetCheck)
        return;
    end

    MySQL.Async.fetchAll('SELECT * FROM ' .. Config.OwnedVehiclesTable .. ' WHERE `plate` = @plate', {
        ['@plate'] = plate
    }, function(vehicle)
        cb(vehicle)
    end)
end)

QBCore.Functions.CreateCallback('roadphone:valet:checkMoney', function(source, cb)
    local xPlayer = QBCore.Functions.GetPlayer(source)

    if not xPlayer then
        return;
    end

    if xPlayer.Functions.GetMoney('bank') > Config.ValetDeliveryPrice then
        xPlayer.Functions.RemoveMoney('bank', Config.ValetDeliveryPrice, 'Car delivered')

        local number = getNumberFromIdentifier(xPlayer.PlayerData.citizenid)

        TriggerEvent("roadphone:addBankTransfer", number, 0,  Lang:t('info.valet_car_delivered'), Config.ValetDeliveryPrice)

        TriggerClientEvent("roadphone:sendNotification", source, {
            apptitle = "APP_VALET_NAME",
            title = "APP_VALET_CAR_ONTHEWAY",
            img = "/public/img/Apps/valet.jpg"
        })

        discordLog("9807270", "Valet", xPlayer.PlayerData.name .. ' ' .. Lang:t('info.valet_car_delivered_2', { value = Config.ValetDeliveryPrice }), 'RoadPhone - Valet', nil, Cfg.ValetWebhook)
        cb(true)
        return;
    else
        TriggerClientEvent("roadphone:sendNotification", source, {
            apptitle = "APP_VALET_NAME",
            title = "APP_VALET_NOTENOUGHMONEY",
            img = "/public/img/Apps/valet.jpg"
        })

        cb(false)
        return;
    end

end)

RegisterServerEvent("roadphone:valetCarSetOutside")
AddEventHandler("roadphone:valetCarSetOutside", function(plate)
    MySQL.Async.execute('UPDATE '..Config.OwnedVehiclesTable..' SET `in_garage` = @in_garage WHERE `plate` = @plate', {
        ['@plate'] = plate,
        ['@in_garage'] = 0,
    })
end)
```
{% endtab %}

{% tab title="ESX" %}
<pre class="language-lua"><code class="lang-lua"><strong>ESX.RegisterServerCallback("roadphone:valet:getCars", function(source, cb)
</strong>
    local xPlayer = ESX.GetPlayerFromId(source)

    if not xPlayer then
        return;
    end

    MySQL.Async.fetchAll("SELECT * FROM " .. Config.OwnedVehiclesTable .. " WHERE `owner` = @identifier and type = @type and `impound` = @impound", {
        ['@impound'] = 0,
        ['@identifier'] = xPlayer.identifier,
        ['@type'] = "car"
    }, function(result)
        local cachedvehicles = {}

        for i = 1, #result do

            local Garage = result[i].garage_id
            local State = _U('valet_state_default')

            table.insert(cachedvehicles, {
                plate = result[i].plate,
                vehicle = json.decode(result[i].vehicle),
                garage = Garage,
                state = State
            })

        end

        cb(cachedvehicles)

    end)

end)

ESX.RegisterServerCallback('roadphone:valet:loadVehicle', function(source, cb, plate)

    local valetCheck = valetServerSideCheck(plate)

    if valetCheck ~= false then
        cb(false, valetCheck)
        return;
    end

    MySQL.Async.fetchAll('SELECT * FROM ' .. Config.OwnedVehiclesTable .. ' WHERE `plate` = @plate', {
        ['@plate'] = plate
    }, function(vehicle)
        cb(vehicle)
    end)
end)

ESX.RegisterServerCallback('roadphone:valet:checkMoney', function(source, cb)
    local xPlayer = ESX.GetPlayerFromId(source)

    if not xPlayer then
        return;
    end

    if xPlayer.getAccount('bank').money > Config.ValetDeliveryPrice then
        xPlayer.removeAccountMoney('bank', Config.ValetDeliveryPrice)

        local number = getNumberFromIdentifier(xPlayer.identifier)

        TriggerEvent("roadphone:addBankTransfer", number, 0,  _U('valet_car_delivered'), Config.ValetDeliveryPrice)

        TriggerClientEvent("roadphone:sendNotification", source, {
            apptitle = "APP_VALET_NAME",
            title = "APP_VALET_CAR_ONTHEWAY",
            img = "/public/img/Apps/valet.jpg"
        })

        discordLog("9807270", "Valet", xPlayer.getName() .. ' ' .. _U('valet_car_delivered_2', Config.ValetDeliveryPrice), 'RoadPhone - Valet', nil, Cfg.ValetWebhook)
        cb(true)
        return;
    else
        TriggerClientEvent("roadphone:sendNotification", source, {
            apptitle = "APP_VALET_NAME",
            title = "APP_VALET_NOTENOUGHMONEY",
            img = "/public/img/Apps/valet.jpg"
        })

        cb(false)
        return;
    end

end)

RegisterServerEvent("roadphone:valetCarSetOutside")
AddEventHandler("roadphone:valetCarSetOutside", function(plate)
    MySQL.Async.execute('UPDATE '..Config.OwnedVehiclesTable..' SET `in_garage` = @in_garage WHERE `plate` = @plate', {
      ['@plate'] = plate,
      ['@in_garage'] = 0,
    })
    return
end)
</code></pre>
{% endtab %}
{% endtabs %}

