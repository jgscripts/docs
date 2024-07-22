# Migration from v1

{% hint style="warning" %}
Ignore this guide unless you are migrating from **v1** of the resource!&#x20;
{% endhint %}

Thank you so much for continuing to use Advanced Garages. Your ongoing support is why we keep developing this script [🧡](https://emojipedia.org/apple/ios-14.5/orange-heart/)

Migrating is really simple, you only need to make a few SQL changes stated below. Advanced Garages still pulls from the default vehicles table, so you won't lose any of your data if you're migrating from Advanced Garages v1.

<details>

<summary>QBCore Users</summary>

Run the following SQL for the new job/gang ownership check

```sql
UPDATE `player_vehicles` SET `citizenid` = `license` WHERE `license` NOT LIKE 'license:%';
UPDATE `player_vehicles` SET `damage` = '';
```

</details>

<details>

<summary>ESX Users</summary>

Run the following SQL to add the columns that used to be QB only. Since the script is now universal, you need these for it to work.

```sql
ALTER TABLE `owned_vehicles` ADD COLUMN IF NOT EXISTS `gang_vehicle` TINYINT(1) DEFAULT '0';
ALTER TABLE `owned_vehicles` ADD COLUMN IF NOT EXISTS `gang_vehicle_rank` INT(10) DEFAULT '0';
UPDATE `owned_vehicles` SET `damage` = '';
```

_Just to note, gang garages functionality still isn't included, as gangs aren't part of ESX by default._

</details>

We don't recommend that you try to migrate your `config.lua` file. Just use our new Configurator tool linked above, and you will have the script configured to your servers needs again in just minutes :smile:
