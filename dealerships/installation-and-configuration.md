# Installation & Configuration

## Installation

1. Unzip the `jg-dealerships-bundle`&#x20;
2. Drag both `jg-dealerships` and `jg-textui` into either your `[qb]` or `[esx_addons]` folder within your server. _If you don't plan on using our new default Text UI, you don't need to transfer it to your server, and you can choose a different Text UI script in the config!_
3. Run either `run-qb.sql` pr `run-esx.sql` from within your MySQL database depending on the framework you are using.

{% hint style="warning" %}
You should probably disable `qb-vehicleshop` or `esx_vehicleshop` because there will be location clashes ;)
{% endhint %}

## Configuration

Now comes the fun part, configuring the script to your servers needs. No need to mess with any code - we have an awesome new configuration tool, that makes it a breeze:

{% embed url="https://configurator.jgscripts.com/dealerships" %}
Dealerships Configurator
{% endembed %}

To use the tool, simply change the settings to your preference, then scroll to the bottom of the page and click the big blue "Generate config.lua" button.

You can then move the downloaded `config.lua` file into the root of the `jg-dealerships`resource, replacing the existing one. To modify your `config.lua` in the future or after an update, you can import it by clicking "Import existing config" at the top of the page. You can then make changes to it and re-generate when you're finished.

## Integrations

Scripts are more fun when they work with others! See a full list of supported integrations and how to get them set up here: [integrations](integrations/ "mention")
