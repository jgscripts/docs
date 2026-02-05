# Installation & Configuration

### Installation

1. Unzip the `jg-mechanic-bundle`
2. Drag all 4 folders (`jg-mechanic`, `jg-mechanic-props`, `jg-vehiclemileage` and `jg-textui`) into a new folder called `[jg]` within your server's `resources` folder.

{% hint style="info" %}
If you don't plan on using our new default Text UI, you don't need to transfer it to your server, and you can choose a different Text UI script in the config!
{% endhint %}

3. Inside of your `server.cfg`, add a new line **after** all your other resources have started:

```
ensure [jg]
```

4. Now navigate into the `install` folder, and then into the `inventory` folder. You will need to follow a different guide depending on your chosen inventory. The item images can be found within the `images` folder.
   * ox\_inventory
   * qb-inventory
   * esx\_inventory

### Configuration

Now for the fun part! Let's get the script perfectly configured for your city. Inside of the `config` folder you will find 4 different configuration files.

The main one you want to worry about is the `config.lua` file. This is the **core configuration for the script.** Most integrations (such as framework, text UI, notifications and more) are automatically detected.

In this file, you will mainly want to customise mechanic locations, core pricing, servicing and tuning options.

If you want to dive deeper and really customise the script to your servers specific needs, we have 3 other config files for you to play with. It will allow you to fine tune how cosmetics, servicing & tuning (engine swaps, brakes, tyres, etc) work in your city. These files are heavily commented to help you understand them. They can be a little complicated, so only modify them if you know what you're doing and somewhat comfortable with Lua.

### Video Installation Guide

Scorpion from the JG Scripts community has created a detailed installation guide, including configuration & inventory setup for QB, Qbox & ESX.

{% embed url="https://www.youtube.com/watch?v=W-yAeUgB3tg" %}

### If you received an error saying \[SQL ERROR]

JG Mechanic tries to automatically make the required database changes. In rare cases, this automatic installation fails and you need to make the changes manually.

Head into the `install` folder within `jg-mechanic`. Run the `database/run.sql` file within your database software (could be PhpMyAdmin or HeidiSQL).

{% hint style="info" %}
If you get the error along the lines of `cannot use the syntax IF NOT EXISTS` - then simply remove every instance of `IF NOT EXISTS` from the run.sql file, and re-run it. It will still run fine.
{% endhint %}

{% hint style="danger" %}
Make sure you are running this SQL code within the correct database - triple check and cross reference the name of the database!
{% endhint %}

