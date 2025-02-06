# Manual Transmissions & Smooth First Gear

{% hint style="danger" %}
Requires **game build 3258** or newer. You can enforce this on your server by adding `sv_enforceGameBuild 3258` to your server.cfg.
{% endhint %}

{% hint style="warning" %}
Available in Mechanic v1.5 and newer. These settings will have no effect/not appear on electric vehicles.
{% endhint %}

### Manual Transmission

You can now add a _real_ manual transmission, in the tablet's tuning parts menu. This is a special item that will change the vehicle's strAdvancedFlags to prevent automatic shifting and require the player to do it themselves. If gear shifting is performed badly, the engine will take damage. The experience is fairly realistic, without being too difficult to use for the average player. The car will not move if you try to move it in 4th gear, for example.

To make it as easy as possible to use & actually desirable on your server, we detect if a vehicle has been stuck at a gear at high RPM for an extended time, and show a prompt with the key bindings to upshift/downshift.

Please note that this functionality is only available in **game build 3258 or newer**. It's super simple to enforce a game build on your server; simply add `sv_enforceGameBuild 3258` to your server.cfg.

### Smooth First Gear

This is a global config option, that will apply server-wide to all vehicles, including vehicles just spawned in/not owned. It will also update the vehicle's strAdvancedFlags to slow the RPM curve of the first gear to a rate much more realistic. This allows a console-type effect on keyboard, with reduced wheelspin and more time to upshift without being in redline when using a manual transmission.

You can enable it by setting:

```lua
Config.SmoothFirstGear = true
```

### Troubleshooting

Either of the above not working? This is likely due to the vehicle not have `strAdvancedFlags` present in it's handling.meta file. All base GTA vehicles have this unless you have re-streamed all the handling files. If it is missing, you will need to add it to the vehicle(s) manually in order for this functionality to work.

\[add guide on how to add strAdvancedFlags]

