# Exports

## Server Exports

***

### vehiclePlateUpdated

You need to run this export in all scripts that updates plates, otherwise you will lose your vehicle's data.

<pre class="language-lua"><code class="lang-lua"><strong>-- plate: string
</strong><strong>-- newPlate: string
</strong><strong>
</strong><strong>exports["jg-mechanic"]:vehiclePlateUpdated(plate, newPlate)
</strong></code></pre>

### doesVehicleNeedServicing

Returns if a vehicle needs servicing.

```lua
-- plate: string
-- returns: boolean
local needService = exports["jg-mechanic"]:doesVehicleNeedServicing(plate)
```

### getVehicleServiceHistory

Returns the service history from a specific vehicle.

<pre class="language-lua"><code class="lang-lua"><strong>-- plate: string
</strong>--[[ return: table
{
    {
        id: number,
        date: number,
        identifier: string,
        plate: string,
        mechanic_label: string,
        serviced_paort: string,
        mechanic: string
        mileage_km: number,
    },
    ...
} 
]]
<strong>
</strong><strong>local history = exports["jg-mechanic"]:getVehicleServiceHistory(plate)
</strong></code></pre>
