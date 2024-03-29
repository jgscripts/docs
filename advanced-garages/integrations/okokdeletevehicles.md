# okokDeleteVehicles

Buy it here: [https://okok.tebex.io/package/5126470](https://okok.tebex.io/package/5126470)

Because JG Advanced Garages uses server-side vehicle spawning, a minor change has to be made to the script in order to delete vehicles.

1. Navigate to `client.lua`&#x20;
2. Replace **lines 92-95** with the following single line:

```lua
TriggerServerEvent('jg-advancedgarages:server:DeleteVehicleEntity', NetworkGetNetworkIdFromEntity(vehicle))
```
