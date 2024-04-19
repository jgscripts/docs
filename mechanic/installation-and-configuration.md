# Installation & Configuration

## Installation

1. Unzip the `jg-mechanic-bundle`&#x20;
2. Drag all 4 folders (`jg-mechanic`, `jg-mechanic-props`, `jg-vehiclemileage` and `jg-textui`) into a new folder called `[jg]` within your server's `resources` folder.

{% hint style="info" %}
If you don't plan on using our new default Text UI, you don't need to transfer it to your server, and you can choose a different Text UI script in the config!
{% endhint %}

3. Inside of your `server.cfg`, add a new line **after** all your other resources have started:

```
ensure [jg]
```

4. Now head into the `install` folder within `jg-mechanic`. In here were are going to modify your database & setup your inventory. Start by running the `database/run.sql` file within your database software (could be phpmyadmin or HeidiSQL).

{% hint style="danger" %}
Make sure you are running this SQL code within the correct database - triple check and cross reference the name of the database!
{% endhint %}

5. Now navigate into the `inventory` folder. You will need to follow a different guide depending on your chosen inventory. The item images can be found in the `images` folder.
   * ox\_inventory
   * qb-inventory
   * esx\_inventory

## Configuration

Now for the fun part! Let's get the script perfectly configured for your city. Inside of the `config` folder you will find 4 different configuration files.

The main one you want to worry about is the `config.lua` file. This is the **core configuration for the script.** In there you will to make sure you're targeting your framework (`QBCore` or `ESX`), your inventory script, text UI, notify and more. You can also customise core parts of how the script work, such as mechanic locations, core pricing, servicing, tuning etc.

If you want to dive deeper and really customise the script to your servers specific needs, we have 3 other config files for you to play with. It will allow you to fine tune how mods (tuning), servicing & tuning (engine swaps, brakes, tyres, etc) work in your city. These files are heavily commented to help you understand them. They can be a little complicated, so only modify them if you know what you're doing and somewhat comfortable with Lua.
