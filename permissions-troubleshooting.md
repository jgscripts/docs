# Permissions Troubleshooting

All admin commands out of the box in our scripts require `god` permissions in QBCore and `admin` permissions in ESX by default.

These required permissions can be adjusted inside of `framework/sv-functions.lua` -> `Functions.Server.IsAdmin()`

1.  To give someone access to the command they need to be a part of the correct group in your `server.cfg` file - see the following example:<br>

    ```bash
    add_principal identifier.fivem:XXXXXXX group.god # QBCore

    add_principal identifier.fivem:XXXXXXX group.admin # ESX
    ```


2. Restart your server<br>
3. :warning: ESX only: If you are still having issues, try running in-game -> `/setgroup [id] admin`
