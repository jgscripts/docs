---
description: >-
  Here are a list of our client sided events! This can help you integrate with
  other resources. Please note, the examples below are examples. You can use
  these events how you wish, these are just ideas!
---

# Events



## Client

### Open tuning menu

Can be used to open the tuning menu at a specific location

```lua
-- mechanicId: string         (The mechanic id from the config)
-- mechanicLabel: string      (The label which will show in the menu)
TriggerEvent("jg-mechanic:client:open-customisation-menu", mechanicId, mechanicLabel)
```

#### Example Usage

```lua
ReigsterCommand('openmechmenu', function() -- This creates a command called openmechmenu (maybe for admins?)
    TriggerEvent("jg-mechanic:client:open-customisation-menu", 'bennys', 'bennys') -- Trigger Actual event
end, true) -- Is it locked? (https://natives.avarian.dev/?native=0x5FA79B0F&game=gta5)
```

### Open tablet

Can be used to open the tablet

<pre class="language-lua"><code class="lang-lua"><strong>TriggerEvent("jg-mechanic:client:use-tablet")
</strong></code></pre>

#### Example usage

```lua
exports.ox_target:addBoxZone({
    coords = vector3(0.0, 0.0, 0.0), -- Vector of where you wanted the box zone
    size = vec3(1.0, 1.0, 2.0),
    rotation = 0,
    debug = false,
    options = {
        {
            label = "Use Tablet",
            icon = "fa-solid fa-tablet-screen-button",
            onSelect = function()
                TriggerEvent("jg-mechanic:client:use-tablet")
            end,
        }
    }
})
```

### Open admin menu

Can be used to open the admin menu

```lua
TriggerEvent("jg-mechanic:client:open-admin")
```

#### Example Usage

```lua
exports.ox_target:addBoxZone({
    coords = vector3(0.0, 0.0, 0.0), -- Vector of where you wanted the box zone
    size = vec3(1.0, 1.0, 2.0),
    rotation = 0,
    debug = false,
    options = {
        {
            label = "Open Admin",
            icon = "fa-solid fa-tablet-screen-button",
            onSelect = function()
                TriggerEvent("jg-mechanic:client:open-admin")
            end,
        }
    }
})
```

### Listeners

#### Listen to toggling of tablet being hidden

```lua
AddEventHandler("jg-mechanic:client:tablet-hidden-for-interaction", function()
end)
```

#### Example usage

```lua
AddEventHandler("jg-mechanic:client:tablet-hidden-for-interaction", function()
    -- Example: Print something random when tablet is hidden
    print('Scorpion is the best support member')
end)
```

#### Listen to toggling of tablet being shown

```lua
AddEventHandler("jg-mechanic:client:tablet-shown-after-interaction", function()
end)
```

```lua
AddEventHandler("jg-mechanic:client:tablet-shown-after-interaction", function()
    print('James is cool')
end)
```

