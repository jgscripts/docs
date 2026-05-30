# API

JG Vehicle Studio has two exports for getting vehicle image URLs from other scripts.

These exports work on **both** client and server.

```lua
exports["jg-vehiclestudio"]:getImage(...)
exports["jg-vehiclestudio"]:getImages(...)
```

### getImage

Returns the image URL for a single vehicle, or `nil` if no image exists.

```lua
-- Get the default image
local image = exports["jg-vehiclestudio"]:getImage("adder")

-- Get a specific image set
local showroomImage = exports["jg-vehiclestudio"]:getImage("adder", "showroom")
```

| Parameter   | Type     | Required | Description                                |
| ----------- | -------- | -------- | ------------------------------------------ |
| `spawnCode` | `string` | Yes      | Vehicle spawn code, for example `"adder"`. |
| `imageId`   | `string` | No       | Image set ID. Defaults to `"default"`.     |

### getImages

Returns a table of spawn codes mapped to image URLs.

Vehicles without an image will have a `nil` value.

```lua
-- Get default images for multiple vehicles
local images = exports["jg-vehiclestudio"]:getImages({
  "adder",
  "banshee",
  "zentorno",
})

-- Get a specific image set for multiple vehicles
local showroomImages = exports["jg-vehiclestudio"]:getImages({
  "adder",
  "banshee",
  "zentorno",
}, "showroom")
```

Example return value:

```lua
{
  adder = "https://...",
  banshee = "https://...",
  zentorno = nil,
}
```

| Parameter    | Type     | Required | Description                            |
| ------------ | -------- | -------- | -------------------------------------- |
| `spawnCodes` | `table`  | Yes      | Array of vehicle spawn codes.          |
| `imageId`    | `string` | No       | Image set ID. Defaults to `"default"`. |

### Image IDs

`imageId` is the image set ID.

If you do not pass an `imageId`, Vehicle Studio uses `"default"`.

```lua
local defaultImage = exports["jg-vehiclestudio"]:getImage("adder")
local orangeImage = exports["jg-vehiclestudio"]:getImage("adder", "orange_bg")
```

Use the same image ID you selected when photographing the vehicle in Vehicle Studio.
