# Default Settings

{% hint style="warning" %}
It's best to do this **before** you launch JG HUD within your server; as if you have allowed users to edit their own settings in the past, this will take precedent over your new default settings profile.
{% endhint %}

JG HUD allows you to set default settings for **all users.** These default settings can include anything that users are able to adjust usually, including colours, visibility of components, size, position and more.

### 1. Creating & exporting a default settings profile

To create a default settings profile, simply go into `/settings` in game as you usually would to adjust your own HUD.

{% hint style="warning" %}
**HUGE WARNING:** If you decide to edit the layout of the HUD as part of your default settings profile, keep in mind these are **anchored** to the corner of their default position. So if you change the speedometer from the bottom right to the bottom left, this **WILL NOT** appear correctly on screen resolutions different from yours.\
\
In general, we suggest that you stick to setting default colours, component visibility and micro adjustments for components in their default corner to ensure it displays as you would expect on all screen resolutions.
{% endhint %}

Once you're happy with the settings you've adjusted, you need to **export them to JSON.** You can do this by clicking the "Import & Export" tab on the left hand side.

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Once in this tab, you need to click "Copy to Clipboard" to copy the configuration JSON to your clipboard.

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### 2. Making the settings profile your server default

Now that you've got the JSON copied to your clipboard, we are going to need to add this to a file within the resource's code. We've created one by default for you, inside of the `data` folder, called `default-settings.json`  (full path: `jg-hud/data/default-settings.json`).

You can also make your own file wherever you'd like (within the jg-hud resource folder), and point to it's location using the config option `Config.DefaultSettingsData`.

Once you've got the correct file, open it in a text editor and paste the JSON in your clipboard. Like this:

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### 3. Important additional info and settings

This will now be the default profile for all **new users** of JG HUD in your server. For existing users, their current settings/layout will take precedence.

It's therefore best to set this up before players join your server and use JG HUD for the first time.

If you want players to always use the exact configuration you've made for them, simply set both the following config options to false, which will mean players will always see whatever is in the default settings JSON file. Please note that preventing users from changing their settings, especially the layout of the HUD, is **not recommended.**

```lua
Config.AllowPlayersToEditSettings = false
Config.AllowUsersToEditLayout = false
```

If you are allowing users to change their settings/layout, and they want to set their HUD to the new default settings you've created, they can go to the "Reset to Default" tab on the left of the settings panel, and reset their settings/layout to default. The defaults will come from the file you've set up.

You're good to go! Have fun configuring! :orange\_heart:
