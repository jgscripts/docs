# Server Exports

### getMileageByEntity

Returns the mileage, in kilometers, of a specified entity. Will return false if the entity does not exist.

```lua
---@param vehicle integer
---@return number|false mileageKm
exports["jg-vehiclemileage"]:getMileageByEntity(vehicle)
```

### getMileageByPlate

Returns the mileage, in kilometers, of a specified vehicle plate. Will return false if the plate could not be found in the database.

{% hint style="warning" %}
This export makes a database call, so use sparingly.
{% endhint %}

```lua
---@param plate string
---@return number|false mileageKm
exports["jg-vehiclemileage"]:getMileageByPlate(plate)
```

### getUnit

Returns the config option of either `kilometers` or `miles` set in the jg-vehiclemileage config.

```lua
---@return "miles"|"kilometers"
exports["jg-vehiclemileage"]:getUnit()
```

