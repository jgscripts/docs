# Installation

## Installation

1. Make sure you have the latest versions of [ox\_lib ](https://github.com/overextended/ox_lib/releases/latest)& [oxmysql](https://github.com/overextended/oxmysql/releases/latest) installed on your server.
2. Download from GitHub, by going to the [latest release](https://github.com/jgscripts/jg-vehiclemileage/releases/latest) page & clicking "Source code (zip)".
3. Unzip the downloaded file & rename the folder to `jg-vehiclemileage`.
4. Drag the script folder (`jg-vehiclemileage`) into a new folder called `[jg]` within your server's `resources` folder.
5. Inside of your `server.cfg`, add a new line **after** all your other resources have started:

```
ensure [jg]
```

## If you received an error saying \[SQL ERROR]

JG Vehicle Mileage tries to automatically make the required database changes. In some cases, this automatic installation fails and you need to make the changes manually.

Head into the `install` folder within `jg-vehiclemileage`. Run the `run-qb.sql` (QBCore/Qbox) or `run-esx.sql` (ESX Legacy) file within your database software (could be PhpMyAdmin or HeidiSQL).

{% hint style="info" %}
If you get the error along the lines of `cannot use the syntax IF NOT EXISTS` - then simply remove every instance of `IF NOT EXISTS` from the run-qb.sql/run-esx.sql file, and re-run it. It will still run fine.
{% endhint %}

{% hint style="danger" %}
Make sure you are running this SQL code within the correct database - triple check and cross reference the name of the database!
{% endhint %}
