---
description: license check for qbcore
---

# QBCORE

{% hint style="info" %}
Firstly you will need to add the following to each dealership in the config.lua
{% endhint %}

```lua
licenseCheck = false, -- false = no license required to open dealership
license = 'driver', -- this is the license name required 
```

{% hint style="info" %}
You will need to replace the following function in config-sv.lua\
&#x20;"jg-dealerships:server:showroom-pre-check"
{% endhint %}

```lua
Framework.Server.CreateCallback("jg-dealerships:server:showroom-pre-check", function(src, cb, dealershipId)
  local allowed = false
  
  -- QBCORE LICENSE CHECKS
  local Player = QBCore.Functions.GetPlayer(src)
  local licenseCheck = Config.DealershipLocations[dealershipId].licenseCheck
  local license = Player.PlayerData.metadata['licences'][Config.DealershipLocations[dealershipId].license]
  if licenseCheck then
    if not license then
      allowed = false

    elseif license  then
      allowed = true
    end
  else
    allowed = true
  end
  
  -- Write some server-sided code here. Again, update the "allowed" variable
  
  if not allowed then
    Framework.Server.Notify(src, "You require a ".. Config.DealershipLocations[dealershipId].license.. " license", "error")
    return cb({ error = true })
  end
  
  return cb()
end)
```

{% hint style="info" %}
Only want to setup individual dealerships and not all you can replace config-sv function with the following config-sv.lu&#x20;
{% endhint %}

```lua
Framework.Server.CreateCallback("jg-dealerships:server:showroom-pre-check", function(src, cb, dealershipId)
  local allowed = false

  -- QBCORE License Check
  -- dealershipId is the name of your dealership in the config.lua
  if dealershipId == "boat" then
    local Player = QBCore.Functions.GetPlayer(src)
    local license = Player.PlayerData.metadata['licences'][Config.DealershipLocations[dealershipId].license]
    if not license then
      allowed = false
    end
  end

  -- Write some server-sided code here. Again, update the "allowed" variable

  if not allowed then
    Framework.Server.Notify(src, "You require a ".. Config.DealershipLocations[dealershipId].license.. " license", "error")
    return cb({ error = true })
  end

  return cb()
end)
```
