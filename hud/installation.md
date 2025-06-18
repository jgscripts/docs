# Installation

## Installation

1. Unzip the `jg-hud-bundle` and open the `jg-hud-bundle` folder.
2. Drag the script folder within (`jg-hud`) into a new folder called `[jg]` within your server's `resources` folder.
3. Make sure you have the latest version of [ox\_lib ](https://github.com/overextended/ox_lib/releases/latest)installed on your server. This script does not use a database, so oxmysql is _not_ required.
4. Inside of your `server.cfg`, add a new line **after** all your other resources have started:

```
ensure [jg]
```

## Configuration

There are some basic options to adjust in `config/config.lua` - mainly integrations, guard rails and essential options to get things up and running for your server.

If you're looking to change some of our built in datasets, such as speed limits, GTA V HUD component hiding, train stations and more, you'll find these in `config/config.data.lua`

Nearest postal data can be found in `data/nearest-postal`. Full credits to BlockBa5her/DevBlocky for the integration - the original repo can be found here: [https://github.com/DevBlocky/nearest-postal](https://github.com/DevBlocky/nearest-postal)

### Editing the HUD

Most configuration will be done in-game, via the HUD settings. This is accessible, by default, via the `/settings` command. You can change this command in the config, via `Config.OpenSettingsCommand`

The settings panel should be fairly intuitive. Click on the different options on the left to adjust different parts of the HUD. The necessary parts of the HUD will show if hidden to allow for easy editing.

You can change the layout by clicking the button that's always in the top right of the settings panel.

By default, players can change the HUD for just their eyes. If you want to enforce a custom HUD layout & setup for all your players, see [default-settings.md](default-settings.md "mention")
