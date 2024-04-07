# Pickle Mods Documents

For registering your vehicle after purchase :-)

Buy it here: [https://store.picklemods.com/package/5447777](https://store.picklemods.com/package/5447777)

{% hint style="warning" %}
#### Replace the  jg-dealerships:client:purchase-vehicle:config event located in the config-cl.lua with this below
{% endhint %}

```lua
RegisterNetEvent("jg-dealerships:client:purchase-vehicle:config", function(vehicle, plate, purchaseType, amount, paymentMethod, financed)
  TriggerServerEvent("jg-dealerships:server:AddVehicleRegistration", plate)
end)
```

{% hint style="warning" %}
#### Add event below into config-sv.lua
{% endhint %}

```lua
RegisterNetEvent("jg-dealerships:server:AddVehicleRegistration", function(plate)
  local src = source
  local matches = MySQL.query.await("SELECT * FROM `dealership_sales` WHERE `plate` = :plate LIMIT 1", {
    plate = plate,
  })
  
  local data = matches[1]
  dealer = data['dealership']
  
  exports.pickle_documents:GiveDocument(src, "VehicleRegistration", {
    plate = plate,
    dealer = dealer,
  }, { takePhoto = true })
end)
```

