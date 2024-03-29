# Config & Customisation

{% hint style="info" %}
You can configure the script inside `config.lua`. Most of the config options are self-explanatory, but this guide should explain most options anyway.
{% endhint %}

### Highlighted Jobs

* The color of each highlighted job must be a hex code
* `countOnDutyOnly` will display the total players in that whitelisted job that are currently set as on duty (:warning: QBCore only)
* Icons are from [https://icons.getbootstrap.com/](https://icons.getbootstrap.com/) - paste the name of the icon (without the `bi-` prefix) to change it
* `ShowAdminBadges` will either hide or show orange stars next to players who have admin or god status
* `AdminBadgeIcon` uses the same bootstrap icons as above
* You can set multiple jobs in one highlighted job counter: for example, if you have multiple mechanic jobs you can do: `job = {"mechanic", "mechanic2"}`

### Name & Logo

* The name can be changed by customising `Config.ServerName`.
* You can change the icon displayed in the top left by replacing the png at `/html/my-logo.png`.

{% hint style="danger" %}
The image that you change to **MUST** be a `.png` and it must be a square aspect ratio.
{% endhint %}

### Key Bind

* `Config.KeyBind` is the default key bind for all players. Individual players can change it to their liking by going into `Settings > Key Bindings > FiveM > (jg-scoreboard) Open Scoreboard` in-game.
