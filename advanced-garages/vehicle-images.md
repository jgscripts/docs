# Vehicle Images

{% hint style="warning" %}
Only available in v2.2.0 or newer
{% endhint %}

You can now show little vehicle thumbnails inside of the garage UI, as you can see below. This will automatically stay up to date with the latest DLC vehicles.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Vehicle thumbnails</p></figcaption></figure>

To enable this feature, set `Config.ShowVehicleImages = true`

If you have addon vehicles, and are looking to add images for these vehicles too, it's super simple!&#x20;

1. There is a folder in the root of the script called `vehicle_images`
2. Get a **.png** (has to be a png) of the addon vehicle (recommended 140x100px)
3. Drag the .png into the `vehicle_images` folder
4. Rename the file to the spawn code/model name of the vehicle - for example `adder.png`

{% hint style="info" %}
Note: If you are using vehicle images within JG Dealerships, you don't need to duplicate them! JG Advanced Garages will automatically pull those images, so long as the feature is enabled within both scripts :)
{% endhint %}
