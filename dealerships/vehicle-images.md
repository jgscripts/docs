# Vehicle Images

{% hint style="warning" %}
Only available in Dealerships v1.2 or newer
{% endhint %}

You can now show little vehicle thumbnails inside of the Dealerships UI, as you can see below. This will automatically stay up to date with the latest DLC vehicles.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Vehicle thumbnails in showroom list</p></figcaption></figure>

To enable this feature, set `Config.ShowVehicleImages = true`

If you have addon vehicles, and are looking to add images for these vehicles too, it's super simple!&#x20;

1. There is a folder in the root of the script called `vehicle_images`
2. Get a **.png** (has to be a png) of the addon vehicle (recommended 140x100px)
3. Drag the .png into the `vehicle_images` folder
4. Rename the file to the spawn code/model name of the vehicle - for example `adder.png`
5. Go to the fxmanifest.lua and add `"vehicle_images/*",` into the files table like this:

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Note: If you are using custom vehicle images within JG Advanced Garages, you don't need to duplicate them! JG Dealerships will automatically pull those images, so long as the feature is enabled within both scripts :orange\_heart:
{% endhint %}
