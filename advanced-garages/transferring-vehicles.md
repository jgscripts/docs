# Transferring Vehicles

### :people\_holding\_hands: Between Players

By default, players can transfer their vehicle to any player that's online, from a dropdown list. The player does not have to accept the transfer.

To hide names to prevent metagaming: `Config.TransferHidePlayerNames = false`

To disable the feature, see [#disabling-transfers](transferring-vehicles.md#disabling-transfers "mention")

### :parking: Between Garages

Vehicles are available from the garage they were last stored in. By default though, players can transfer vehicles between garages for their convenience for a configurable fee.

Adjust the fee: `Config.GarageVehicleTransferCost`

To disable the feature, see [#disabling-transfers](transferring-vehicles.md#disabling-transfers "mention")

### :octagonal\_sign: Blacklist Transfers Between Players

You may want to allow transfers between players, but prevent this for certain vehicles (such as donator or other sensitive vehicles). This is super simple to do - simply add the spawn code of the vehicle to `Config.PlayerTransferBlacklist`

```lua
Config.PlayerTransferBlacklist = {
  "adder",
  "thrax"
}
```

### :no\_entry: Disabling Transfers

```lua
Config.EnableTransfers = {
  betweenGarages = false, -- disables transfers between garages
  betweenPlayers = false -- disables transfers between players
}
```
