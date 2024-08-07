---
description: license check for esx
---

# ESX

{% hint style="info" %}
Firstly you will need to add the following to each dealership in the config.lua
{% endhint %}

```lua
licenseCheck = false, -- false = no license required to open dealership
license = 'driver', -- this is the license name required 
```

{% hint style="info" %}
You will need to replace the following function in config-sv.lua  "jg-dealerships:server:showroom-pre-check"
{% endhint %}

<pre class="language-lua"><code class="lang-lua">lib.callback.register("jg-dealerships:server:showroom-pre-check", function(src, dealershipId)
<strong>  local allowed = false
</strong>  
  -- ESX LICENSE CHECKS
  local licenseCheck = Config.DealershipLocations[dealershipId].licenseCheck
  local license = MySQL.scalar.await('SELECT type FROM user_licenses WHERE type = ? AND owner = ?', {Config.DealershipLocations[dealershipId].license, Framework.Server.GetPlayerIdentifier(src)})
  if licenseCheck then
    if not license then
      allowed = false
    elseif license then
      allowed = true
    end
  else
    allowed = true
  end

  
  if not allowed then
    Framework.Server.Notify(src, "You are not allowed to access the showroom", "error")
    return false
  end

  return true
end)
</code></pre>

{% hint style="info" %}
If you only want to setup individual dealerships and not all you can replace config-sv function with the following config-sv.lua
{% endhint %}

```lua
lib.callback.register("jg-dealerships:server:showroom-pre-check", function(src, dealershipId)
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
    Framework.Server.Notify(src, "You are not allowed to access the showroom", "error")
    return false
  end

  return true
end)
```
