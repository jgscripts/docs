# Garage Events

## Client

### jg-advancedgarages:client:open-garage

This event can be used to open a specific garage anywhere

```lua
-- garageId: string
-- vehicleType?: "car" | "air" | "sea"
-- spawnCoords?: vector4 | Only needed for unknown/house garage integrations
TriggerEvent("jg-advancedgarages:client:open-garage", garageId, vehicleType, spawnCoords)
```

### jg-advancedgarages:client:store-vehicle

This event can be used to store a vehicle into a specific garage anywhere

```lua
--garageId: string
--garageVehicleType: "car" | "sea" | "air"
--return: boolean success
TriggerEvent("jg-advancedgarages:client:store-vehicle", garageId, garageVehicleType)
```

### jg-advancedgarages:client:show-impound-form

This event can for example be used in a radial menu to show the impound form

```lua
TriggerEvent("jg-advancedgarages:client:show-impound-form")
```

### jg-advancedgarages:client:show-private-garages-dashboard

This event can be used to open the private garages dashboard so a new garage can be created

```lua
TriggerEvent("jg-advancedgarages:client:show-private-garages-dashboard")
```

### jg-advancedgarages:client:show-vplate-form

This event can be used to open the change plate ui

```lua
-- vehicle?: integer
TriggerEvent("jg-advancedgarages:client:show-vplate-form", vehicle)
```

## Server

### jg-advancedgarages:server:register-vehicle-outside

This event can be used to register an owned vehicle if it has been spawned with a thirdparty script (eg. phone valet)

```lua
--plate: string
--netId: integer
TriggerEvent("jg-advancedgarages:server:register-vehicle-outside", plate, netId)
```

\
