# Installation & Configuration

## Installation

1. Unzip the `jg-dealerships-bundle`&#x20;
2. Drag `jg-dealerships` into a new folder called `[jg]` within your server's `resources` folder.

{% hint style="info" %}
If you don't plan on using our new default Text UI, `jg-textui`, you don't need to transfer it to your server, and you can choose a different Text UI script in the config!
{% endhint %}

3. Inside of your `server.cfg`, add a new line **after** all your other resources have started:

```
ensure [jg]
```

{% hint style="danger" %}
You should probably disable `qb-vehicleshop` or `esx_vehicleshop` because there will be location clashes!
{% endhint %}

### Migrating from v1?

If you want to keep all of your existing data, stop here and follow this guide instead:

[migrating-from-v1.md](migrating-from-v1.md "mention")

### Setting Up Locations

Dealership locations are now created and managed entirely in-game. No configuration required!

1. Open the admin panel with `/dealeradmin`
2. Navigate to **Locations**
3. Click **Create Location**
4. Configure the location settings
5. Save the location

#### Location Types

<table><thead><tr><th width="226">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>self-service</code></td><td>Public dealership where anyone can browse and purchase vehicles (no stock limits)</td></tr><tr><td><code>owned</code></td><td>Player-owned dealership with stock management, employees, direct sales and finances</td></tr></tbody></table>

### Configuration

Further configuration options are available in the `config/config.lua` file. Most of the options are set to auto, meaning that in some cases, you may not need to edit this file at all!

The most important options that would need editing are **Localisation**, **Framework & Integrations** & **Interaction Methods**.

If you want to add any custom dealership categories, this would also need to be done in the config. You can find categories a little further down, under `Config.Categories`. Dealership categories dictate what categories are browsable and what vehicles will be available in the showroom UI.

### If you received an error saying \[SQL ERROR]

JG Dealerships tries to automatically make the required database changes. In some cases, this automatic installation fails and you need to make the changes manually.

Head into the `install` folder within `jg-dealerships`. Run either the `run-qb.sql` (QBCore & Qbox) or `run-esx.sql` (ESX) file within your database software (could be PhpMyAdmin or HeidiSQL).

{% hint style="info" %}
If you get the error along the lines of `cannot use the syntax IF NOT EXISTS` - then simply remove every instance of `IF NOT EXISTS` from the run.sql file, and re-run it. It will still run fine.
{% endhint %}

{% hint style="danger" %}
Make sure you are running this SQL code within the correct database - triple check and cross reference the name of the database!
{% endhint %}

### Integrations

Scripts are more fun when they work with others! See a full list of supported integrations and how to get them set up here: [integrations](integrations/ "mention")
