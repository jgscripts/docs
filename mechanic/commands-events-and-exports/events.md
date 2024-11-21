# Events



## Client

### Open Tuning Menu

Can be used to open the tuning menu at a specific location

```lua
-- mechanicId: string         (The mechanic id from the config)
-- mechanicLabel: string      (The label which will show in the menu)
TriggerEvent("jg-mechanic:client:open-customisation-menu", mechanicId, mechanicLabel)
```

### Open Tablet

Can be used to open the tablet

```lua
TriggerEvent("jg-mechanic:client:use-tablet")
```

### Open Admin menu

Can be used to open the admin menu

```lua
TriggerEvent("jg-mechanic:client:open-admin")
```
