# Brazzers-FakePlate



{% hint style="info" %}
This code was provided to us by IgnoranceIsBliss. &#x20;

This code has been tested and works. It requires some code editing for garages and relies on **ox-lib**.
{% endhint %}

### Step 1.  Download the latest releases of ox-lib and Brazzers-FakePlate  if you do not have them!

{% embed url="https://github.com/Brazzers-Development/brazzers-fakeplates" %}

{% embed url="https://github.com/overextended/ox_lib/releases" %}

### Step 2.  Modifiy the shared\_scripts Section of the Garage's fxmanifest.lua

> Replace the section with the one below.

```lua
shared_scripts {'config.lua', 'locales/*.lua', '@ox_lib/init.lua', 'main.lua', 'framework/main.lua'}
```

### Step 3.  Edit The Framework.Client.GetPlate Function in 'Framework/cl-functions.lua'

> Replace the GetPlate Section with below.&#x20;

```lua
function Framework.Client.GetPlate(vehicle)
  if Config.Framework == "QBCore" then
    local plate = QBCore.Functions.GetPlate(vehicle)
    local originalPlate = lib.callback.await('brazzers-fakeplates:getPlateFromFakePlate', false, plate)
    if originalPlate then plate = originalPlate end
    return plate
  elseif Config.Framework == "ESX" then
    return ESX.Game.GetVehicleProperties(vehicle).plate
  end
end
```

### Step 4.  Modify the jg-advancedgarages:client:TakeOutVehicle:config Event in 'config-cl.lua'&#x20;

> Add the below code to the event

```lua
local fakePlate = lib.callback.await('brazzers-fakeplates:getFakePlateFromPlate', false, vehicleDbData.plate)
    if fakePlate then SetVehicleNumberPlateText(vehicle,fakePlate) end
```

### Step 5. Add The Following to config-sv.lua

```lua
lib.callback.register('brazzers-fakeplates:getPlateFromFakePlate', function(source, fakeplate)
    local result = MySQL.scalar.await('SELECT plate FROM player_vehicles WHERE fakeplate = ?', {fakeplate})
    if result then
        return result
    end 
end)

lib.callback.register('brazzers-fakeplates:getFakePlateFromPlate', function(source, plate)
    local result = MySQL.scalar.await('SELECT fakeplate FROM player_vehicles WHERE plate = ?', {plate})
    if result then
        return result
    end
end)
```

### Step 6.  Modify the Vehicle Key Functions to recognize the Fake Plate

> &#x20;Find  Framework.Client.VehicleGiveKeys(plate, vehicleEntity) and add the below code after if not DoesEntityExist(vehicleEntity) then return false end

```lua
 local fakePlate = lib.callback.await('brazzers-fakeplates:getFakePlateFromPlate', false, plate)
  if fakePlate then plate = fakePlate end
```

Do the Same for Framework.Client.VehicleRemoveKeys(plate, vehicleEntity)

```lua
local fakePlate = lib.callback.await('brazzers-fakeplates:getFakePlateFromPlate', false, plate)
  if fakePlate then plate = fakePlate end
```
