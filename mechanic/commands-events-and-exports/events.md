# Events



### Client

#### Open tuning menu

Can be used to open the tuning menu at a specific location

```lua
-- mechanicId: string         (The mechanic id from the config)
-- mechanicLabel: string      (The label which will show in the menu)
TriggerEvent("jg-mechanic:client:open-customisation-menu", mechanicId, mechanicLabel)
```

#### Open tablet

Can be used to open the tablet

```lua
TriggerEvent("jg-mechanic:client:use-tablet")
```

#### Open admin menu

Can be used to open the admin menu

```lua
TriggerEvent("jg-mechanic:client:open-admin")
```

#### Listen to toggling of tablet visibility for interactions

```lua
AddEventHandler("jg-mechanic:client:tablet-hidden-for-interaction", function() end)
AddEventHandler("jg-mechanic:client:tablet-shown-after-interaction", function() end)
```

