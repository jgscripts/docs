# Public, Private & Impound Garages

### Public Garages

* You can configure the prices of transfers to other garages, and returning of vehicles back to the garage if they are left out (or make them free)
* There are 3 different types of garages - `car`, `sea` and `air`. For players that own planes, helicopters or boats, you will to add air and sea garages so that players can retrieve these vehicles, as they can't retrieve them from car garages _(hint: there are example air and sea garages already added in the default config!)_.
  * Vehicle types are decoded from the meta files included with a vehicle - if your addon plane or boat is showing in the wrong garage, check the `vehicles.meta` for that addon vehicle
* You can configure the distance at which the instruction overlays appear when you're near the garage

### Private Garages

* Admins or players with an enabled job (see `Config.PrivGarageCreateCommand`) can use `/createprivategarage` and a UI will appear guiding you through the process
* Only the player selected in the step above will be able to access the garage
* Deleting the garage has to be done through the database in the table `player_priv_garages`. Coming soon is a better way of handling this!

### Impound

* Players with an enabled job (see `Config.ImpoundJobRestriction`) can use `/iv` when next to or inside a car to send it to the impound
* You can set a reason and whether the owner can retrieve it themselves. If this is set to false, only players with a whitelisted job can remove it from the impound for them
* If self-retrieval is enabled, you can set a time before they can take it out and a price they must pay (or 0)
* The owner of the vehicle can see in their garage that it has been impounded, the reason why and by who to avoid any confusion
* You can add one or more impound locations in `config.lua`. Vehicles that are impounded are available at every impound location.
* You can also add impounds for `air` and `sea` vehicles, by setting their type in `config.lua`. If type is not provided, it will default to `car`, and only `car` vehicle types will show in the impound.

###
