# Events

## Client

### Open garage

This event can be used to open a specific garage, such as via a radial menu.  If the `garageId` is a registered garage (i.e. specified in the config), it will perform a location check automatically for security.

```lua
-- garageId: string
-- vehicleType?: "car" | "air" | "sea"
-- spawnCoords?: vector4 | Only needed for unknown/house garage integrations
TriggerEvent("jg-advancedgarages:client:open-garage", garageId, vehicleType, spawnCoords)
```

### Store current vehicle

This event can be used to store the vehicle into a garage. If the `garageId` is a registered garage (i.e. specified in the config), it will perform a location check automatically for security.

```lua
--garageId: string
--garageVehicleType: "car" | "sea" | "air"
--return: boolean success
TriggerEvent("jg-advancedgarages:client:store-vehicle", garageId, garageVehicleType)
```

### Show impound form

Show the impound form (if you have permissions) - useful for showing via a radial menu, item, or key bind.

```lua
TriggerEvent("jg-advancedgarages:client:show-impound-form")
```

### Show private garages manager

This event can be used to open the private garages dashboard so a private garage can be created, edited or deleted.

```lua
TriggerEvent("jg-advancedgarages:client:show-private-garages-dashboard")
```

### Show change plate form

This event can be used to open the change plate UI.

```lua
-- vehicle?: integer
TriggerEvent("jg-advancedgarages:client:show-vplate-form", vehicle)
```

## Server

### Register vehicle outside \[deprecated]

{% hint style="danger" %}
This event has been deprecated and will be removed in a future release.
{% endhint %}

```lua
--plate: string
--netId: integer
TriggerEvent("jg-advancedgarages:server:register-vehicle-outside", plate, netId)
```

\
