# License Check

{% hint style="info" %}
Firstly you will need to add the following to each dealership in the config.lua
{% endhint %}

```lua
licenseCheck = false, -- false = no license required to open dealership
license = 'driver', -- this is the license name required 
```

{% hint style="info" %}
You will need to replace the following callback in config-sv.lua\
&#x20;"jg-dealerships:server:showroom-pre-check"
{% endhint %}

{% tabs %}
{% tab title="QBCore" %}
```lua
lib.callback.register("jg-dealerships:server:showroom-pre-check", function(src, dealershipId)
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
    return false
  end

  return true
end)
```
{% endtab %}

{% tab title="ESX" %}
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
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Do you only want license check for individual dealerships? Then replace the config-sv callback with the following
{% endhint %}

{% tabs %}
{% tab title="QBCore" %}
```lua
lib.callback.register("jg-dealerships:server:showroom-pre-check", function(src, dealershipId)
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
    return false
  end

  return true
end)
```
{% endtab %}

{% tab title="ESX" %}
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
{% endtab %}
{% endtabs %}
