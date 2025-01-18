# Installation

## Installation

1. Unzip the `jg-handling-bundle`&#x20;
2. Drag the script folder (`jg-handling`) into a new folder called `[jg]` within your server's `resources` folder.
3. Make sure you have the latest versions of [ox\_lib ](https://github.com/overextended/ox_lib/releases/latest)& [oxmysql ](https://github.com/overextended/oxmysql/releases/latest)installed on your server.
4. Inside of your `server.cfg`, add a new line **after** all your other resources have started:

```
ensure [jg]
```

## Configuration

Now for the fun part! Let's get the script perfectly configured for your server. Inside of the `config` folder you will find 2 different configuration files.

The main one you want to worry about is the `config.lua` file. This is the **core configuration for the script.** Most integrations (such as framework (optional), notifications and more) are automatically detected.

In this file, you will mainly want to customise the functionality of the script, such as adjusting the job lock, timing tool features etc. Enjoy!

## If you received an error saying \[SQL ERROR]

JG Handling tries to automatically make the required database changes. In some cases, this automatic installation fails and you need to make the changes manually.

Head into the `install` folder within `jg-handling`. Run the `database.sql` file within your database software (could be PhpMyAdmin or HeidiSQL).

{% hint style="info" %}
If you get the error along the lines of `cannot use the syntax IF NOT EXISTS` - then simply remove every instance of `IF NOT EXISTS` from the run.sql file, and re-run it. It will still run fine.
{% endhint %}

{% hint style="danger" %}
Make sure you are running this SQL code within the correct database - triple check and cross reference the name of the database!
{% endhint %}

