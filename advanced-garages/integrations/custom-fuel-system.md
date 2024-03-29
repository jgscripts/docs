# Custom Fuel System

If we don't support your fuel system out of the box, it's super easy to add your own!

1. Go to the `framework` folder, and open `cl-functions.lua`
2. Find the `VehicleGetFuel` and `VehicleSetFuel` functions, which should look like this:

```lua
function Framework.Client.VehicleGetFuel(vehicle)
  if (Config.FuelSystem == "LegacyFuel" or Config.FuelSystem == "ps-fuel" or Config.FuelSystem == "lj-fuel" or Config.FuelSystem == "cdn-fuel" or Config.FuelSystem == "hyon_gas_station") then
    return exports[Config.FuelSystem]:GetFuel(vehicle)
  elseif Config.FuelSystem == "ox_fuel" then
    return GetVehicleFuelLevel(vehicle)
  else
    return 65 -- or set up custom fuel system here...
  end
end

function Framework.Client.VehicleSetFuel(vehicle, fuel)
  if (Config.FuelSystem == "LegacyFuel" or Config.FuelSystem == "ps-fuel" or Config.FuelSystem == "lj-fuel" or Config.FuelSystem == "cdn-fuel" or Config.FuelSystem == "hyon_gas_station") then
    exports[Config.FuelSystem]:SetFuel(vehicle, fuel)
  elseif Config.FuelSystem == "ox_fuel" then
    Entity(vehicle).state.fuel = fuel
  else
    -- Setup custom fuel system here
  end
end
```

3. Inside the `VehicleGetFuel` function, within the `else` block where it says\
   \
   `return 65 -- or set up custom fuel system here...`\
   \
   replacing it with your custom **get fuel level** code. Remember to rename the vehicle entity variable to `vehicle` in case your code snippet has it named differently.\

4. Do the same inside the `VehicleSetFuel` function replacing the line that says\
   \
   `-- Setup custom fuel system here`\
   \
   with the **set fuel level** function of your fuel script. The vehicle entity variable is named `vehicle` and the fuel level is named `fuel` - again make sure to rename these if they are named differently in the snippet you are using! \

5. Make sure that in the Configurator or `config.lua` you have set the Fuel system to **None**.
