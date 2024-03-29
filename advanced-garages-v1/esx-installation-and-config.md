# ESX Installation & Config

{% hint style="danger" %}
Do **NOT** change the name of the resource. Not even taking the `-esx` off the end of the resource name. Changing the folder name or any other names in any way will break the script.
{% endhint %}

{% hint style="danger" %}
This script **requires**`oxmysql` for making database requests. If you aren't already using it, you can [download it here](https://github.com/overextended/oxmysql) and add it into `resources/[standalone]`&#x20;
{% endhint %}

{% hint style="warning" %}
If you still have `esx-garage` or another garage script enabled, you should probably disable it so it doesn't conflict!
{% endhint %}

### Installation

1. Move the resource inside of the `resources/[esx_addons]` folder in your server
2. Run the code inside run.sql in your MySQL database
3. Configure the `config.lua` file as necessary for your needs!

### Configuration

You can configure the script using `config.lua` for general personalisation for your server, and through `config-client.lua` to add custom code for certain script events such as when a vehicle is inserted into the garage. There are instructions throughout the comments in each file.

Both configuration files are heavily commented, to help you understand what various configuration options do.
