---
description: Sam's ultimate guide
---

# Why isn't /admincar working?!

`/admincar` is a commonly used command in QBCore that allows admins to add a spawned in vehicle to their owned vehicles. What is even more common though is this error:

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

There is a few mistakes that can make it so you can't own vehicles in QBCore. This guide will explain everything that is potentially wrong, and help you fix it.

### Issues with the vehicles meta file

First we will have a look at the vehicle itself. Browse to your vehicles files and go to the "Data" section of these files (this is where the vehicles .meta files are located). Each vehicle will have the following .meta files:

* `carvariations.meta`
* `handling.meta`
* `vehicles.meta` Some vehicles may have more, but these are the main ones you will need to look at.

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption><p>Typical meta files found with an addon vehicle</p></figcaption></figure>

Now to start off we will go over some fundementals. Your cars model name is the name of the model in the "Stream" section of your vehicles files. This will also be the vehicles in-game spawn code when it has been added. The name can be changed but this is not recommended as it needs to be changed in all "Data" files for it to work properly.

{% hint style="danger" %}
**IMPORTANT: From our research we have found that model names that don't follow these guidelines break /admincar:**

* NEVER put any capital letters in this name
* NEVER put any symbols in this name.
* NEVER make the name longer than **12 characters** this can be unreliable.
{% endhint %}

`carvariations.meta`: Here the only thing you should double check or change is the "modelName".

```xml
<modelName>pgt3</modelName> <!-- This should be set to the model name. -->
```

`handling.meta`: Here the only thing you should double check or change is the "handlingName".

```xml
<handlingName>pgt3</handlingName> <!-- This should be set to the model name for simplicity's sake. -->
```

`vehicles.meta`: Here you will have to double check a lot of things. Bellow you will find an example of what you need to look out for or change.

```xml
  <modelName>pgt3</modelName> <!-- This should be set to the model name. -->
  <txdName>pgt3</txdName> <!-- This should be set to the model name. -->
  <handlingId>pgt3</handlingId> <!-- If you have been following allong should be set as the model name for simplicity's sake. -->
  <gameName>pgt3</gameName> <!-- This should be set to the model name. -->
  <audioNameHash>gt3flat6</audioNameHash> <!-- This defines what audio file your car will use and is changeable to custom ones. -->
  <vehicleClass>VC_SUPER</vehicleClass> <!-- This defines and should be set as the vehicle class. -->
```

If you have followed these steps your vehicle model itself should be working. **Restart the server fully and try doing /admincar again**.

{% hint style="danger" %}
You could restart only the resource holding the addon vehicle but this crashes the game if **ANY** vehicle in this resource is still out in the world so this is not recommended.
{% endhint %}

If this still gives you the pop-up saying "You can't store this vehicle in your garage.." you will have to go to the next step.

### Mistakes QB-Core/shared/vehicles.lua file

The second thing we will be having a look at is the shared vehicles file. This file is located in `[qb]/qb-core/shared/vehicles.lua`. This should contain every vehicle that is able to be stored / owned in the garage system. If the vehicle you are trying to `/admincar` is not in this file or is not formatted correctly it will not work.

The fundamentals from the first part still count for this part so if you are unsure, go back and check those before you continue.

Bellow you will find an example of a properly formatted shared vehicle:

```lua
['pgt3'] = {
    	['name'] = '911 GT3 RS',
    	['brand'] = 'Porsche',
    	['model'] = 'pgt3',
    	['price'] = 195000,
    	['category'] = 'super',
    	['hash'] = `pgt3`,
    	['shop'] = 'pdm',
},
```

Bellow you will find how to properly share your vehicle and what you need to put where:

`['pgt3'] = {` This should be set to the model name. (check fundementals to see what is not allowed and where to find this.)

`['name'] = '911 GT3 RS',` This should be set to the vehicles name and will be displayed in the garage system. You are allowed to use capital letters here. You are allowed to use symbols here.

`['brand'] = 'Porsche',` This should be set to the vehicles brand and will be displayed in the garage system. You are allowed to use capital letters here. You are allowed to use symbols here.

`['model'] = 'pgt3',` This should be set to the model name. (check fundementals to see what is not allowed and where to find this.)

`['price'] = 195000,` This sets the price for QB-Vehicleshop and should reflect how much the vehicle is worth. This is also used in some replacement vehicle shops to set the price of the vehicle. This can also be used in some replacement vehicleshops as they have a setting to import vehicles and add them to the store using the data stored in the `shared/vehicles.lua` file.

`['category'] = 'super',` This should be set as the vehicle category. In some scripts this is used to determine howmuch inventory capacity the vehicle has. It can also determine what vehicle store category it gets added to for some scripts.

`['hash'] = pgt3,` This should be set to the model name. (check fundamentals to see what is not allowed and where to find this.)

`['shop'] = 'pdm',` This is set to pdm by default however as QB-Vehicleshop and most other replacements have the option to have multiple dealerships, be sure to set this to the dealership you wish this vehicle to be sold in. This can also be used in some replacement vehicleshops as they have a setting to import vehicles and add them to the store using the data stored in the `shared/vehicles.lua` file.
