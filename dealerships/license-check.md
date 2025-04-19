# License Check

{% hint style="info" %}
Firstly you will need to add the following to each dealership in the config.lua
{% endhint %}

```lua
licenseCheck = false, -- false = no license required to open dealership
license = 'driver', -- this is the license name required 
```

{% hint style="info" %}
You will need to replace the following function in config-cl.lua\
function ShowroomPreCheck(dealershipId)
{% endhint %}

{% tabs %}
{% tab title="QBCore" %}
```lua
function ShowroomPreCheck(dealershipId)
  local Player = Framework.Client.GetPlayerData()
  local licenseCheck = Config.DealershipLocations[dealershipId].licenseCheck
  local license = Player.metadata['licences'][Config.DealershipLocations[dealershipId].license]

  if licenseCheck then
    if not license then
      allowed = false

    elseif license  then
      print(license)
      allowed = true
    end
  else
    allowed = true
  end

  if not allowed then
    msg = "You require a ".. Config.DealershipLocations[dealershipId].license
    Framework.Client.Notify(msg, "error", 1000 )
    return false
  end

  return true

end
```
{% endtab %}

{% tab title="ESX" %}
```lua
function ShowroomPreCheck(dealershipId)
    local allowed = true
    local licenseCheck = Config.DealershipLocations[dealershipId].licenseCheck
    local licenseType = Config.DealershipLocations[dealershipId].license

    if licenseCheck and licenseType then
        local hasLicense = lib.callback.await('jg-dealerships:server:check-license', false, licenseType)

        if not hasLicense then
            allowed = false
        end
    end

    if not allowed then
        TriggerServerEvent("jg-dealerships:showroom-notify", "You are not allowed to access the showroom", "error")
        return false
    end

    return true
end

-- the code block below is pasted inside the config-sv.lua

lib.callback.register('jg-dealerships:server:check-license', function(src, licenseType)
    local identifier = Framework.Server.GetPlayerIdentifier(src)

    local result = MySQL.scalar.await(
        'SELECT type FROM user_licenses WHERE type = ? AND owner = ?',
        {licenseType, identifier}
    )

    return result ~= nil
end)

```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Do you only want license check for individual dealerships? Then replace the config-cl function with the following
{% endhint %}

{% tabs %}
{% tab title="QBCore" %}
```lua
function ShowroomPreCheck(dealershipId)
  -- Only check licenses for "boat" dealership
  if dealershipId ~= "boat" then
      return true
  end

  local allowed = true
  local licenseCheck = Config.DealershipLocations[dealershipId].licenseCheck
  local licenseType = Config.DealershipLocations[dealershipId].license

  if licenseCheck and licenseType then
      -- Ask the server if this player has the required license
      local hasLicense = lib.callback.await('jg-dealerships:server:check-license', false, licenseType)

      if not hasLicense then
          allowed = false
      end
  end

    if not allowed then
        local msg = "You require a " .. licenseType .. " license to access this showroom."
        Framework.Client.Notify(msg, "error", 1000)
        return false
  end

  return true
end

--
```
{% endtab %}

{% tab title="ESX" %}
<pre class="language-lua"><code class="lang-lua">function ShowroomPreCheck(dealershipId)
  -- Only check licenses for "boat" dealership
  if dealershipId ~= "boat" then
      return true
  end

  local allowed = true
  local licenseCheck = Config.DealershipLocations[dealershipId].licenseCheck
  local licenseType = Config.DealershipLocations[dealershipId].license

  if licenseCheck and licenseType then
      -- Ask the server if this player has the required license
      local hasLicense = lib.callback.await('jg-dealerships:server:check-license', false, licenseType)

      if not hasLicense then
          allowed = false
      end
  end

    if not allowed then
        local msg = "You require a " .. licenseType .. " license to access this showroom."
        Framework.Client.Notify(msg, "error", 1000)
        return false
  end

  return true
end
<strong>
</strong><strong>-- the code block below is pasted inside the config-sv.lua
</strong>
lib.callback.register('jg-dealerships:server:check-license', function(src, licenseType)
    local identifier = Framework.Server.GetPlayerIdentifier(src)

    local result = MySQL.scalar.await(
        'SELECT type FROM user_licenses WHERE type = ? AND owner = ?',
        {licenseType, identifier}
    )

    return result ~= nil
end)
</code></pre>
{% endtab %}
{% endtabs %}
