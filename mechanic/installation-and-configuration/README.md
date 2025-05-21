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

4. Now navigate into the `inventory` folder. You will need to follow a different guide depending on your chosen inventory. The item images can be found in the `images` folder.
   * ox\_inventory
   * qb-inventory
   * esx\_inventory

## Configuration

Now for the fun part! Let's get the script perfectly configured for your city. Inside of the `config` folder you will find 4 different configuration files.

The main one you want to worry about is the `config.lua` file. This is the **core configuration for the script.** Most integrations (such as framework, text UI, notifications and more) are automatically detected.

In this file, you will mainly want to customise mechanic locations, core pricing, servicing and tuning options.

If you want to dive deeper and really customise the script to your servers specific needs, we have 3 other config files for you to play with. It will allow you to fine tune how cosmetics, servicing & tuning (engine swaps, brakes, tyres, etc) work in your city. These files are heavily commented to help you understand them. They can be a little complicated, so only modify them if you know what you're doing and somewhat comfortable with Lua.

## If you received an error saying \[SQL ERROR]

JG Mechanic tries to automatically make the required database changes. In some cases, this automatic installation fails and you need to make the changes manually.

Head into the `install` folder within `jg-mechanic`. Run the `database/run.sql` file within your database software (could be PhpMyAdmin or HeidiSQL).

{% hint style="info" %}
If you get the error along the lines of `cannot use the syntax IF NOT EXISTS` - then simply remove every instance of `IF NOT EXISTS` from the run.sql file, and re-run it. It will still run fine.
{% endhint %}

{% hint style="danger" %}
Make sure you are running this SQL code within the correct database - triple check and cross reference the name of the database!
{% endhint %}

## Use our get/setVehicleProperties exports (optional)

{% hint style="success" %}
Using [JG Advanced Garages](https://jgscripts.com/scripts/advanced-garages)? Skip this section, you're good to go, all vehicle modifications will be saved automatically with no configuration required if using **v2.2.8 or newer**!
{% endhint %}

JG Mechanic uses your "vehicle properties" to save changes made your vehicle while using the script. Vehicle properties are typically stored in your player owned vehicles database table table, under a column such as `mods` or `vehicle`.

Our exports are designed for JG Mechanic, and offer many improvements on the typical functions included with your framework. We recommend, if you feel comfortable modifying your framework's code, to change to using our exports. &#x20;

To make things as simple as possible, we have created **2 replacement exports** for getting and setting these vehicle properties. If using QBCore or ESX, we can replace them in your core to keep things simple. There's also a guide for replacing the ox\_lib functions, however ox\_lib's functions are well written and will have no conflicts with JG Mechanicc. So that is _very_ optional.

<details>

<summary><strong>QBCore</strong></summary>

1. Navigate to `[qb]/qb-core/client/functions.lua` and locate the 2 functions:

```lua
QBCore.Functions.GetVehicleProperties(vehicle)
```

```lua
QBCore.Functions.SetVehicleProperties(vehicle, props)
```

2. These are two very large functions - but we are going to _entirely_ replace them. A life hack is on VSCode, you can collapse them with the little arrow near the line numbers to make it easy to delete them.

<img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt="" data-size="original">

3. Replace these 2 functions with the following new code:

```lua
function QBCore.Functions.GetVehicleProperties(vehicle)
    return exports["jg-mechanic"]:getVehicleProperties(vehicle)
end

function QBCore.Functions.SetVehicleProperties(vehicle, props)
    exports["jg-mechanic"]:setVehicleProperties(vehicle, props)
end
```

You may be concerned about how much code you have removed. No need to worry: you can re-get this code from the QBCore GitHub at any time, and all the functionality has been replaced for you directly inside JG Mechanic. Now do a full server restart and you're ready to go!

</details>

<details>

<summary><strong>ESX</strong></summary>

1. Navigate to `[core]/es_extended/client/functions.lua` and locate the 2 functions:

```lua
function ESX.Game.GetVehicleProperties(vehicle)
```

```lua
function ESX.Game.SetVehicleProperties(vehicle, props)
```

2. These are two very large functions - but we are going to _entirely_ replace them. A life hack is on VSCode, you can collapse them with the little arrow near the line numbers to make it easy to delete them.

<img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt="" data-size="original">

3. Replace these 2 functions with the following new code:

```lua
function ESX.Game.GetVehicleProperties(vehicle)
    return exports["jg-mechanic"]:getVehicleProperties(vehicle)
end

function ESX.Game.SetVehicleProperties(vehicle, props)
    exports["jg-mechanic"]:setVehicleProperties(vehicle, props)
end
```

You may be concerned about how much code you have removed. No need to worry: you can re-get this code from the ESX GitHub at any time, and all the functionality has been replaced for you directly inside JG Mechanic. Now do a full server restart and you're ready to go!

</details>

<details>

<summary>ox_lib (very very optional)</summary>

1.  Navigate to `ox_lib\resource\vehicleProperties\client.lua` and locate the 2 functions:\


    ```lua
    function lib.getVehicleProperties(vehicle)
    ```



    ```lua
    function lib.setVehicleProperties(vehicle, props, fixVehicle)
    ```


2. These are two very large functions - but we are going to _entirely_ replace them. A life hack is on VSCode, you can collapse them with the little arrow near the line numbers to make it easy to delete them.\
   \
   ![](<../../.gitbook/assets/image (24).png>)\

3.  Replace these 2 functions with the following new code:\


    ```lua
    ---@param vehicle number
    ---@return VehicleProperties?
    function lib.getVehicleProperties(vehicle)
        return exports["jg-mechanic"]:getVehicleProperties(vehicle)
    end

    ---@param vehicle number
    ---@param props VehicleProperties
    ---@param fixVehicle? boolean Fix the vehicle after props have been set. Usually required when adding extras.
    ---@return boolean isEntityOwner True if the entity is networked and the client is the current entity owner.
    function lib.setVehicleProperties(vehicle, props, fixVehicle)
        exports["jg-mechanic"]:setVehicleProperties(vehicle, props)
        return not NetworkGetEntityIsNetworked(vehicle) or NetworkGetEntityOwner(vehicle) == cache.playerId
    end
    ```

    \
    You may be concerned about how much code you have removed. No need to worry: you can re-get this code from the Overextended GitHub at any time, and all the functionality has been replaced for you directly inside JG Mechanic. Now do a full server restart and you're ready to go!
</details>

## Need Extra Support?

If you’re stuck or want a visual walkthrough of how to get JG Mechanic set up properly, check out our step-by-step video guide:

[🎥 Watch the Installation & Setup Tutorial](https://youtu.be/W-yAeUgB3tg?si=pje4oke6vEjK4Rs9)

This video covers installation, configuration, and common troubleshooting tips — perfect if you're more of a visual learner or want to double-check your setup.


