# Vehicle Duplication Prevention

Vehicle duplication prevention is designed to improve the roleplay experience by only allowing one vehicle to be taken out at a time, just like in real life. This feature works across public, private, job and gang garages.

To enable the feature, set the infinite vehicle spawn config options to false. You can enable and disable the feature for different garage types separately, through the 3 options in the [configurator](https://configurator.jgscripts.com/advanced-garages).

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

### Behaviour

* When taking out a vehicle, the script stores the vehicle ID on the server, so that all clients are aware the vehicle is out, so a client restart will not put a vehicle back in the garage.
* If a vehicle is completely destroyed or is de-spawned, the vehicle will be returned to the garage menu (for a fee, if configured).
* This works for job/gang garages too, as all clients are aware of a vehicle being out, it cannot be spawned by another job member if a vehicle has been taken out.

### Known Issues

A perfect vehicle duplication system is extremely difficult to get right, as many scripts have found. Since moving to using server-side vehicle spawning, there are only a couple of known issues which are due to FiveM limitations. Vehicles that are blocked in, are underwater or are upside down will NOT be respawnable, as they still technically have health.

**If this happens, players can contact an admin, who can use the command `/vreturn [plate]`, which will return the vehicle to the garage for the player.** If this is an issue, it may be best to allow infinite respawns in the config.
