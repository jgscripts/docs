# Exports

## Server Exports

***

### getAllGarages

Used to get information on all garages

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
local garages = exports['jg-advancedgarages']:getAllGarages()
</code></pre>

