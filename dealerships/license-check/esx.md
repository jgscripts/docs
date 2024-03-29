---
description: license check for esx
---

# ESX

{% hint style="info" %}
Firstly you will need to add the following to each dealership in the config.lua
{% endhint %}

```lua
license = false, -- this will mean the dealership doesnt require a license to open
license = 'boat', -- this is how to make it require a license name/type to open 
```

{% hint style="info" %}
You will need to replace the following function in config-sv.lua  "jg-dealerships:server:showroom-pre-check"
{% endhint %}

```lua

Framework.Server.CreateCallback("jg-dealerships:server:showroom-pre-check", function(src, cb, dealershipId)
  local allowed = true
  
  -- ESX LICENSE CHECKS
  local license = MySQL.scalar.await('SELECT type FROM user_licenses WHERE type = ? AND owner = ?', {Config.DealershipLocations[dealershipId].license, Framework.Server.GetPlayerIdentifier(src)})
  if not license then
    allowed = false
  elseif license == false then
    allowed = true
  end
  
  if not allowed then
    Framework.Server.Notify(src, "You are not allowed to access the showroom", "error")
    return cb({ error = true })
  end

  return cb()
end)

```

{% hint style="info" %}
If you only want to setup individual dealerships and not all you can replace config-sv function with the following config-sv.lua
{% endhint %}

```lua
Framework.Server.CreateCallback("jg-dealerships:server:showroom-pre-check", function(src, cb, dealershipId)
  local allowed = true
  
  -- ESX License Check
  -- dealershipId is the name of your dealership in the config.lua
  if dealershipId == "boat" then
    local license = MySQL.scalar.await('SELECT type FROM user_licenses WHERE type = ? AND owner = ?', {Config.DealershipLocations[dealershipId].license, Framework.Server.GetPlayerIdentifier(src)})
    if not license then
      allowed = false
    end
  end
  
  if not allowed then
    Framework.Server.Notify(src, "You are not allowed to access the showroom (server-side)", "error")
    return cb({ error = true })
  end

  return cb()
end)
```
