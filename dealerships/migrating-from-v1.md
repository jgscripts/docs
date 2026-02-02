# Migrating from v1

{% hint style="info" %}
This guide is only useful if you are looking to retain all the data from your v1 installation. If you want to start from scratch with v2, follow the installation guide to install it from scratch.
{% endhint %}

This guide walks you through upgrading from JG Dealerships v1 to v2 while preserving your existing data.

### Before You Begin

⚠️ **Important:** Back up your existing `config.lua` file before proceeding. You will need it later.

### Step 1: Replace Script Files

1. Delete all existing files in your `jg-dealerships` folder
2. Extract the new v2 files downloaded from Portal into the folder
3. Copy your backed-up `config.lua` into `config/config.lua`, replacing the new one

### Step 2: Add New Configuration Options

Add the following new v2 options to the bottom of your `config.lua`:

```lua
Config.UseFrameworkJobs = true
Config.InteractionMethod = "textui" -- or "target", "3dtextui", "radial"
Config.Target = "auto" -- or "ox_target"
Config.DrawText3d = "auto" -- or "sleepless_interact"
Config.RadialMenu = "auto" -- or "ox_lib"
Config.BlipNameFormat = "Dealership: %s"
Config.EntityStreamingDistance = 20.0 -- Distance in meters at which entities despawn/respawn
Config.TruckingMissionForOrderDeliveries = true
Config.DealershipMaxActiveTestDrives = 5 -- Maximum number of active test drives per dealership
```

### Step 3: Restart and Migrate Database

1. Fully restart your server
2. Run the migration command using **one** of these methods:
   * Type `/migratev2` in the in-game chat
   * Run `migratev2` in the txAdmin server console

### Step 4: Import Your Locations

1. Navigate to `/dealeradmin` in-game
2. Click the **Import** button
3. Select **"Existing Config"** (described as: _Import locations from your config.lua (for v1 migration)_)
4. Follow the import wizard to complete the process

### Done!

Your v1 data should now be migrated to v2. Verify your dealership locations & and check all functionality is working as expected.
