# Exports

## Server Exports

### Get all garages

Used to get information on all garages.

<pre class="language-lua"><code class="lang-lua">--[[ return: table
{
    {
        name: string,
        label: string,
        type: string,
<strong>        vehicle: string,
</strong>        blipName: string,
        blipColor: number,
        blipNumber: number
        showBlip: boolean,
        takeVehicle: vector3,
        putVehicle: vector3,
        spawnPoint: vector4,
    },
    ...
} 
]]
local garages = exports["jg-advancedgarages"]:getAllGarages()
</code></pre>

### Register vehicle outside

{% hint style="warning" %}
Available in v3.2.5 and later
{% endhint %}

Register a vehicle as outside the garage & currently spawned; so the vehicle cannot be respawned. This prevents vehicle duplication. Useful for integrating for scripts also spawning vehicles from the garage; such as a valet.

<pre class="language-lua"><code class="lang-lua">---@param plate string
---@param netId integer The network ID of the vehicle (not entity id!)
<strong>exports["jg-advancedgarages"]:registerVehicleOutside(plate, netId)
</strong></code></pre>

### Delete outside vehicle

{% hint style="warning" %}
Available in v3.2.5 and later
{% endhint %}

This allows you to delete a vehicle via it's plate if the vehicle is registered as outside within JG Advanced Garages; either via interacting with a garage directly or by using the `registerVehicleOutside` export.

```lua
---@param plate string
exports["jg-advancedgarages"]:deleteOutsideVehicle(plate)
```
