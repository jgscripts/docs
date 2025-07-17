# Custom Street/Zone Names

{% hint style="success" %}
Available in v1.2 or newer
{% endhint %}

JG HUD allows you to rename any street or zone (area) name within the game. This is useful for creating a more immersive experience, if your server isn't based in Los Santos.

All of the following changes are done within `config/config.data.lua`&#x20;

### Adding Custom Street Names

1. Look for `Config.CustomStreetNames`
2. To add a new entry, you need the **hash** of the street you'd like to update. This is quite difficult to find, so we made a file to make things easier. You can find it here: [https://github.com/jgscripts/gtav-street-zone-hashes/blob/main/streets.txt](https://github.com/jgscripts/gtav-street-zone-hashes/blob/main/streets.txt)
3. Search in the file above for the street you'd like to update, for example, _Los Santos Freeway_**.** Copy the hash (starts with `0x`) to the left of the street name. In this case, it will be `0xAC9F694E`.
4. Add it to the table, with square brackets around the hash. Do NOT add quotes around the hash. Then set it equal to the name you'd like to change it to. This is what it should look like:

```lua
Config.CustomStreetNames = {
  [0xAC9F694E] = "Custom name for Los Santos Freeway",
}
```

### Adding Custom Zone Names

1. Look for `Config.CustomZoneNames`&#x20;
2. To add a new entry, you need the **hash** of the street you'd like to update. This is quite difficult to find, so we made a file to make things easier. You can find it here: [https://github.com/jgscripts/gtav-street-zone-hashes/blob/main/zones.txt](https://github.com/jgscripts/gtav-street-zone-hashes/blob/main/zones.txt)
3. Search in the file above for the zone/area you'd like to update, for example, _Los Santos International Airport_. Copy the hash (which is a shorter, capitalised version of the zone), to the left of the zone name. In this case, it will be `AIRP`.
4. Add it to the table, by setting the hash equal to the name you'd like to change it to. This is what it should look like:

```lua
Config.CustomZoneNames = {
  AIRP = "Custom name for LS International Airport",
}
```

### Using Your Changes Game-Wide

By default, these custom names will only apply to the compass within JG HUD. If you want your custom names to update across the entire game, so both the base game and other scripts will use your custom names, set `Config.CustomNamesShouldUpdateGameTextEntries = true`. JG HUD will automatically handle applying all of the text entries for you.
