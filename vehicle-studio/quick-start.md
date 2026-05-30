# Quick Start

This guide covers the basic workflow for using JG Vehicle Studio after the resource has been installed and configured.

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

### Before you start

Make sure the resource is started after `ox_lib`.

In `config/config.lua`, check:

| Setting                       | What it controls                                                                             |
| ----------------------------- | -------------------------------------------------------------------------------------------- |
| `Config.VehicleStudioCommand` | The command used to open Vehicle Studio. Default: `vehiclestudio`.                           |
| `Config.ImageStorageProvider` | Where finished images are stored. Use `local`, `s3`, `r2`, or `fivemanage`.                  |
| `Config.HttpBaseUrl`          | Required for local image storage when the automatic endpoint cannot be reached from the NUI. |
| `Config.DataStorage`          | Stores vehicle, image, preset, and settings data in `local_data/` or the database.           |

If `Config.DataStorage` is set to `database`, make sure `oxmysql` is available. The SQL schema in `sql/jg_vehiclestudio.sql` is used for database storage.

### Open Vehicle Studio

Run the configured command in-game:

```
/vehiclestudio
```

The main window has three sections:

<table><thead><tr><th width="222">Section</th><th>Purpose</th></tr></thead><tbody><tr><td>Gallery</td><td>Add vehicles, import vehicles, photograph vehicles, retake images, preview images, and delete saved images.</td></tr><tr><td>Presets</td><td>Rename or delete saved photography presets.</td></tr><tr><td>Settings</td><td>Check upload configuration and configure image fallbacks.</td></tr></tbody></table>

### Add vehicles

Open the Gallery tab, then use **Add Vehicles**.

You can add vehicles in two ways:

| Option       | Use it when                                                                                        |
| ------------ | -------------------------------------------------------------------------------------------------- |
| Add manually | You already know the spawn codes you want to photograph.                                           |
| Import       | You want to import vehicles from the configured framework, JG Dealerships, or an external dataset. |

Manual spawn codes are validated before they are added. Invalid models are skipped, and vehicles already in the gallery are not duplicated.

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

### Photograph one vehicle

1. Select a vehicle in the Gallery.
2. Click **Take Photo** in the side panel.
3. Choose a size or a preset.
4. Choose the image set.
5. Click **Next**.
6. Position and modify the vehicle.
7. Click the capture button.
8. Edit the final image.
9. Click **Save**.

When saving, Vehicle Studio asks whether you want to save the current setup as a preset. Saving a preset is optional, but it is useful when you want the same camera, vehicle setup, and image edits for future photos.

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

### Bulk photograph vehicles

Use **Bulk Photograph** from the Gallery when you want to process many vehicles with the same setup.

Bulk photographing can process:

| Scope        | What happens                                                                                       |
| ------------ | -------------------------------------------------------------------------------------------------- |
| Only missing | Only vehicles without the selected image set are photographed.                                     |
| All vehicles | Every gallery vehicle is photographed, and existing images in the selected image set are replaced. |

For the fastest repeatable workflow, create a preset first, then use that preset in Bulk Photograph. If you do not enable **Review setup before photographing**, Vehicle Studio skips the manual positioning and editor steps and starts processing with the preset settings.

<figure><img src="../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

### Manage saved images

Selecting a vehicle opens its side panel. From there, you can:

| Action         | What it does                                     |
| -------------- | ------------------------------------------------ |
| Preview        | Opens the saved image at a larger size.          |
| Retake         | Starts photography again for the same image set. |
| Delete image   | Deletes one image set for that vehicle.          |
| Delete vehicle | Deletes the vehicle and all saved images for it. |

Retaking an image with the same image set replaces the existing image.

{% hint style="warning" %}
Deleting saved images removes them from Vehicle Studio storage. If you use remote storage, make sure the provider credentials still allow deletion.
{% endhint %}

### Manage presets

Open the Presets tab to rename or delete saved presets. Presets are listed with their thumbnail and output size where available.

[Learn more about presets](presets.md)

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

### Use images in other scripts

Other resources can use the `getImage` and `getImages` exports to display saved images in their own UI.

```lua
local imageUrl = exports["jg-vehiclestudio"]:getImage("adder")
```

See [Exports API](api.md) for full examples.
