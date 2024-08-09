# Vehicle Labels

If you have added addon vehicles/imports to your server, you may see `NULL` or strange capitalised vehicle labels in the garage interface. ESX does not come with a built-in "shared" file to index all vehicle models like QBCore, so this has been built into the script for your convience!

To add a pretty vehicle label to your addon/import vehicles, navigate to `config.lua`, and at the bottom of the file there is a section `Config.VehicleLabels`. There are two example rows added as an example, but these can be deleted if you are not using those vehicles.

For example, if you have added an RS6 with the spawn code `rs6` and want to add a label for it, the code would look like the following:

```lua
Config.VehicleLabels = {
  ["rs6"] = "Audi RS6"
}
```
