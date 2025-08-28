# Custom Framework

Using a custom framework with JG Vehicle Mileage is fairly straightforward. All your framework needs to have is some sort of owned vehicles database table, and that table needs to have a `plate` column in it. Then all you need to do is:

1. Go to `jg-vehiclemileage/main.lua`
2. Add another conditional for your framework, and point to the name of your vehicles table
3. In `jg-vehiclemileage/config.lua`, set `Config.Framework = [your framework name]`

Example:

```lua
if Config.Framework == "QBCore" then
  Framework.VehiclesTable = "player_vehicles"
elseif Config.Framework == "ESX" then
  Framework.VehiclesTable = "owned_vehicles"
elseif Config.Framework == "MyFrameworkName" then
  Framework.VehiclesTable = "vehicles_table_name"
else
  error("You haven't set a valid framework. Valid options can be found in main.lua!")
end
```
