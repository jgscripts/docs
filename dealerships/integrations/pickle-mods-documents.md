# Pickle Mods Documents

For registering your vehicle after purchase :-)

Buy it here: [https://store.picklemods.com/package/5447777](https://store.picklemods.com/package/5447777)



In `config-sv.lua` update the `jg-dealerships:server:purchase-vehicle:config` event to include the following code:

```lua
RegisterNetEvent("jg-dealerships:server:purchase-vehicle:config", function(playerSrc, vehNetId, plate, purchaseType, amountToPay)
  local vehicle = NetworkGetEntityFromNetworkId(vehNetId)

  local matches = MySQL.query.await("SELECT * FROM `dealership_sales` WHERE `plate` = :plate LIMIT 1", {
    plate = plate,
  })

  local data = matches[1]
  dealer = data['dealership']

  exports.pickle_documents:GiveDocument(playerSrc, "VehicleRegistration", {
    plate = plate,
    dealer = dealer,
}, { takePhoto = false })
end)
```

