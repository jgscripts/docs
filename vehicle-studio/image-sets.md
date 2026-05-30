# Image Sets

Image sets are named photo slots for a vehicle. They are useful if you want vehicles to be able to have multiple images for different use cases. A transparent PNG for lists, and a cool stylised pic with a background for showcase images, for example.

They are controlled by the `imageId` value used when a photo is saved or requested through exports. The default image set is called `default`.

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

### Why image sets exist

A single vehicle can need more than one image. For example:

<table><thead><tr><th width="228">Example image set ID</th><th>Example use</th></tr></thead><tbody><tr><td><code>default</code></td><td>Main gallery image.</td></tr><tr><td><code>showroom</code></td><td>Dealership UI image.</td></tr><tr><td><code>loading_banner</code></td><td>Loading screen image.</td></tr><tr><td><code>transparent</code></td><td>Transparent-background image for custom UI layouts.</td></tr></tbody></table>

Each vehicle stores images by image set. That means `adder/default` and `adder/showroom` are separate images.

### Image sets are shared names, not folders

An image set is just an ID. It does not create a separate folder or global collection.

If you photograph `adder`, `banshee`, and `zentorno` with the image set `showroom`, each vehicle gets its own `showroom` image. Other scripts can then ask for the `showroom` image by passing the same image ID to the exports.

```lua
local showroomImage = exports["jg-vehiclestudio"]:getImage("adder", "showroom")
```

### Naming rules

In the UI, custom image set IDs:

<table><thead><tr><th width="293">Rule</th><th>Detail</th></tr></thead><tbody><tr><td>Allowed characters</td><td>Letters, numbers, and underscores.</td></tr><tr><td>Length</td><td>Up to 25 characters.</td></tr><tr><td>Default set</td><td>Use the <strong>Use default image set</strong> switch to save as <code>default</code>.</td></tr></tbody></table>

Use lowercase underscore names for consistency, such as `showroom`, `website_banner`, or `transparent_bg`.

Internally, image IDs are sanitized before storage. Existing server-side paths also support hyphens, but the UI is designed around underscore IDs.

### Saving and replacing images

When a vehicle is photographed with a new image set, Vehicle Studio adds that image set to the vehicle.

When a vehicle is photographed with an image set it already has, the existing image for that vehicle and image set is replaced.

For example:

<table><thead><tr><th width="293">Action</th><th>Result</th></tr></thead><tbody><tr><td>Save <code>adder</code> with <code>default</code></td><td>Creates or replaces the default image for <code>adder</code>.</td></tr><tr><td>Save <code>adder</code> with <code>showroom</code></td><td>Creates or replaces the showroom image for <code>adder</code>.</td></tr><tr><td>Save <code>banshee</code> with <code>showroom</code></td><td>Creates or replaces the showroom image for <code>banshee</code>. It does not affect <code>adder</code>.</td></tr></tbody></table>

{% hint style="warning" %}
Retaking an image or bulk photographing with **All vehicles** can replace existing images in the selected image set.
{% endhint %}

### Bulk photography and image sets

Bulk Photograph uses the selected image set to decide what to process.

| Scope        | Behavior                                                                          |
| ------------ | --------------------------------------------------------------------------------- |
| Only missing | Photographs vehicles that do not already have the selected image set.             |
| All vehicles | Photographs every vehicle and replaces existing images in the selected image set. |

This is useful when you add a new image set later. For example, if every vehicle already has `default`, you can create a new `website_banner` set by bulk photographing **Only missing** for `website_banner`.

### File names and storage

Local image files are stored in `exported_images/`.

| Image set        | Local filename format            |
| ---------------- | -------------------------------- |
| `default`        | `<spawnCode>.<format>`           |
| Custom image set | `<spawnCode>_<imageId>.<format>` |

Examples:

```
exported_images/adder.webp
exported_images/adder_showroom.webp
exported_images/adder_website_banner.png
```

When database storage is enabled, metadata is stored by both `spawn_code` and `image_id`.

### Image fallbacks

If an image does not exist, Vehicle Studio can return a configured fallback image instead. Fallbacks are controlled in the Settings tab.

By default, Vehicle Studio tries the FiveM vehicle image URL and then the local no-image placeholder. If you need exports to return `nil` for missing images, disable image fallbacks in Settings.

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

### Using image sets from other scripts

Pass the image set ID as the second argument to `getImage` or `getImages`.

```lua
local defaultImage = exports["jg-vehiclestudio"]:getImage("adder")
local showroomImage = exports["jg-vehiclestudio"]:getImage("adder", "showroom")

local images = exports["jg-vehiclestudio"]:getImages({
  "adder",
  "banshee",
  "zentorno",
}, "website_banner")
```

See [Exports API](api.md) for full API details.
