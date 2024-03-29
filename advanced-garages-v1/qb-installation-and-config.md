# QB Installation & Config

{% hint style="danger" %}
Do **NOT** change the name of the resource. Changing the folder name or any other names in any way will break the script.
{% endhint %}

{% hint style="warning" %}
If you still have `qb-garages` or another garage script enabled, you should probably disable it so it doesn't conflict!
{% endhint %}

### Installation

1. Move the resource inside of the `resources/[qb]` folder in your server
2. Run the code inside `run.sql` in your MySQL database
3. Configure the `config.lua` file as necessary for your needs!

### Configuration

You can configure the script using `config.lua` for general personalisation for your server, and through `config-client.lua` to add custom code for certain script events such as when a vehicle is inserted into the garage. There are instructions throughout the comments in each file.

Both configuration files are heavily commented, to help you understand what various configuration options do.
