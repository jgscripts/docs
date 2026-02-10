# License Check

{% hint style="success" %}
This guide currently only works with JG Dealerships V2
{% endhint %}

{% hint style="info" %}
Firstly you will need to add the following to the config.lua
{% endhint %}

```lua
-- License Requirements
-- Configure license requirements for dealerships
-- Note: In V2, dealership locations are stored in the database
-- Use the dealership ID (UUID) from the dealership_locations to configure license requirements
Config.DealershipLicenses = {
   ["b4dbbcca-a7ba-462a-b772-3c3e6434612c"] = { -- Replace with your dealership ID
     enabled = true,
     licenseType = "driver" -- The license type required (e.g., "boat", "pilot", "driver")
   },
}
```

{% hint style="info" %}
You will need to replace the following function in config-cl.lua\
function ShowroomPreCheck(dealershipId)
{% endhint %}

{% tabs %}
{% tab title="QBCore" %}
```lua
---@param dealershipId string
---@return boolean allowed
function ShowroomPreCheck(dealershipId)
  local licenseConfig = lib.callback.await("jg-dealerships:server:get-dealership-license-config", false, dealershipId)
  
  if not licenseConfig then
    return true
  end

  if not licenseConfig.enabled then
    return true
  end

  local Player = Framework.Client.GetPlayerData()
  local hasLicense = Player.metadata and Player.metadata['licences'] and Player.metadata['licences'][licenseConfig.licenseType]

  if not hasLicense then
    local msg = "You require a " .. licenseConfig.licenseType .. " license to access this showroom."
    Framework.Client.Notify(msg, "error", 5000)
    return false
  end

  return true
end
```
{% endtab %}

{% tab title="ESX" %}
```lua
---@param dealershipId string
---@return boolean allowed
function ShowroomPreCheck(dealershipId)
  local licenseConfig = lib.callback.await("jg-dealerships:server:get-dealership-license-config", false, dealershipId)
  
  if not licenseConfig then
    return true
  end

  if not licenseConfig.enabled then
    return true
  end

  local hasLicense = lib.callback.await('jg-dealerships:server:check-player-license', false, licenseConfig.licenseType)

  if not hasLicense then
    local msg = "You require a " .. licenseConfig.licenseType .. " license to access this showroom."
    Framework.Client.Notify(msg, "error", 5000)
    return false
  end

  return true
end
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
You will need to add the code below inside the config-sv.lua for this integration to work
{% endhint %}

{% tabs %}
{% tab title="QBCore" %}
```lua
---@param src integer
---@param dealershipId string
---@return table|nil configuration { enabled: boolean, licenseType: string }
lib.callback.register("jg-dealerships:server:get-dealership-license-config", function(src, dealershipId)
  if not dealershipId then
    return nil
  end

  local licenseConfig = Config.DealershipLicenses[dealershipId]
  
  if not licenseConfig then
    return nil
  end

  return licenseConfig
end)
```
{% endtab %}

{% tab title="ESX" %}
```lua
---Get dealership license configuration
---@param src integer
---@param dealershipId string
---@return table|nil configuration { enabled: boolean, licenseType: string }
lib.callback.register("jg-dealerships:server:get-dealership-license-config", function(src, dealershipId)
  if not dealershipId then
    return nil
  end

  local licenseConfig = Config.DealershipLicenses[dealershipId]
  
  if not licenseConfig then
    return nil
  end

  return licenseConfig
end)

---Check if player has a specific license (supports both ESX and QB/Qbox)
---@param src integer
---@param licenseType string
---@return boolean hasLicense
lib.callback.register('jg-dealerships:server:check-player-license', function(src, licenseType)
  if not licenseType then
    return false
  end

  -- ESX: Check user_licenses table in database
  if Config.Framework == "ESX" then
    local identifier = Framework.Server.GetPlayerIdentifier(src)
    
    local result = MySQL.scalar.await(
      'SELECT type FROM user_licenses WHERE type = ? AND owner = ?',
      {licenseType, identifier}
    )
    
    return result ~= nil
  end

  -- QB/Qbox: Check player metadata
  if Config.Framework == "QBCore" or Config.Framework == "Qbox" then
    local Player = Framework.Server.GetPlayer(src)
    
    if not Player then
      return false
    end
    
    return Player.PlayerData.metadata and 
           Player.PlayerData.metadata['licences'] and 
           Player.PlayerData.metadata['licences'][licenseType] or false
  end

  return false
end)
```
{% endtab %}
{% endtabs %}
