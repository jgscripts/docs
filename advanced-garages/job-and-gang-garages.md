# Job & Gang Garages

{% hint style="danger" %}
Gang garages are available in the QBCore version only, as QBCore has gang functionality built in. For ESX, you can set up a job garage that can be used for gang use.
{% endhint %}

Job/gang garages are shared between players that have been assigned to a particular job or gang, usually using `/setjob` or `/setgang`.

There are 3 distinct garage types:

| 1. Vehicle Spawner                                                                                                                                                                                                                                                                                                                                                  | 2. Owned Vehicles                                                                                                                                                                                                                                                                                                                                                                                                                             | 3. Personal Vehicles                                                                                                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>A vehicle spawner garage is the most basic garage type. You can define a list of vehicles that members of the job/gang should be able to choose from, a minimum job rank for each vehicle if applicable, and a plate that would you like each vehicle to spawn with.<br><br>All vehicles can be spawned as many times as a player wants without restriction.</p> | <p>An owned vehicles job garage is where all the vehicles available in the garage to job/gang members are <strong>stored in the database, just like personal vehicles</strong>.<br><br>Job/gang members can customise the cars but are responsible for damaging them or losing them while out. If a vehicle has been taken out, it cannot be taken out again if <code>Config.JobGaragesAllowInfiniteVehicleSpawns</code> is set to false.</p> | <p>A personal vehicles garage lets you store personally owned vehicles like a standard public garage.<br><br>The difference to a public garage is that only members of the job/gang can see or interact with the garage.</p> |

### Setting up a garage

From within the [configurator](https://configurator.jgscripts.com/advanced-garages), it's extremely intuitive to create a new job or gang garage. Simply scroll down to the job or gang garages sections, and click the add garage button.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>New Job Garage</p></figcaption></figure>

From within the form, you can give the garage a **unique name,** the in-game job name as it is referred to in the QB or ESX framework (such as `police`), as well as location and spawn coordinates. You can even add multiple spawn locations, in different parking spaces for example.

Next, select the garage type and it's accessible radius.

### Personal Vehicle Garages

These work just like public garages. Set the job/gang garage to store personal vehicles, and players can store their personal vehicles as usual. Players who are not part of the job/gang will not be able to see or interact with the garage.

### Owned Vehicle Garages

If you're setting up an owned vehicle garage, leave the "Owned Vehicles" option selected in the form. Once you have created the garage and are ready to test it out in-game, here is how you can add a vehicle to the owned vehicle job garage:

1. **As an admin in-game**, acquire a car into your personal garage by either purchasing it from a vehicle store, or by spawning it in with `/car`, and then adding it to your garage with `/admincar`. Or, you can get in an owned car from another player.
2. Use the command `/setjobvehicle [job_name] [min_job_grade]` or \
   `/setgangvehicle [gang_name] [min_gang_grade]` to add this owned vehicle to the shared job/gang garage respectively.
3. Admins can use `/removejobvehicle [new_player_owner_id]` while in the car to give it back to a player and make it a personal vehicle again

### Vehicle Spawner Garages

If you select the vehicle spawner option, you can give the spawn names of the vehicles directly in the form, as well as a custom plate and minimum rank restriction:

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption><p>Vehicle Spawner</p></figcaption></figure>
