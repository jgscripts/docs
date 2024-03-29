---
description: license check for qbcore
---

# QBCORE

{% hint style="info" %}
Firstly you will need to add the following to each dealership in the config.lua
{% endhint %}

```lua
license = false, -- this will mean the dealership doesnt require a license to open
license = 'boat', -- this is how to make it require a license name/type to open 
```

{% hint style="info" %}
You will need to replace the following function in config-sv.lua "jg-dealerships:server:showroom-pre-check"
{% endhint %}

```lua
Framework.Server.CreateCallback("jg-dealerships:server:showroom-pre-check", function(src, cb, dealershipId)
  local allowed = false

  -- QBCORE LICENSE CHECKS
  local Player = QBCore.Functions.GetPlayer(src)
  local license = Player.PlayerData.metadata['licences'][Config.DealershipLocations[dealershipId].license]
  if Config.DealershipLocations[dealershipId].license then
    allowed = true
    if not license then
      allowed = false
    end
  elseif not Config.DealershipLocations[dealershipId].license then
    allowed = true
  end

  -- Write some server-sided code here. Again, update the "allowed" variable

  if not allowed then
    Framework.Server.Notify(src, "You are not allowed to access the showroom", "error")
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
    Framework.Server.Notify(src, "You are not allowed to access the showroom", "error")
    return cb({ error = true })
  end

  return cb()
end)
```
