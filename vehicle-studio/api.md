# API

JG Vehicle Studio has two exports for getting vehicle image URLs from other scripts.

These exports work on **both** client and server.

```lua
exports["jg-vehiclestudio"]:getImage(...)
exports["jg-vehiclestudio"]:getImages(...)
```

### getImage

Returns the stored image URL for a vehicle and an ordered list of computed fallback image URLs.

```lua
---@param spawnCode string|number
---@param imageId? string Defaults to "default"
---@return string|nil image
---@return string[] fallbacks
local image, fallbacks = exports["jg-vehiclestudio"]:getImage(spawnCode, imageId)
```

**Return values**

* `image`: The stored Vehicle Studio image URL for the requested vehicle and image set. This is `nil` when Vehicle Studio does not have a saved image for that vehicle/image set.
* `fallbacks`: Ordered fallback image URLs with `{MODEL}`, `{model}`, `{HASH}` and `{hash}` tokens resolved for the requested vehicle.

Fallback URLs are returned as candidates only. The export does not check whether remote fallback images exist. Browser/NUI integrations should try the returned URLs in order and use the first one that loads.

When a fallback URL has no variable tokens, it is treated as a final static fallback. Any configured fallbacks after that static URL are not returned.

**Example**

```lua
local image, fallbacks = exports["jg-vehiclestudio"]:getImage("adder")
```

### getImages

Returns stored image URLs and computed fallback image URLs for multiple vehicles.

```lua
---@param spawnCodes string[]|number[]
---@param imageId? string Defaults to "default"
---@return table<string|number, { image: string|nil, fallbacks: string[] }>
local images = exports["jg-vehiclestudio"]:getImages(spawnCodes, imageId)
```

Each entry in the returned table contains:

* `image`: The stored Vehicle Studio image URL for that vehicle, or `nil`.
* `fallbacks`: Ordered computed fallback image URLs for that vehicle.

**Example**

```lua
local images = exports["jg-vehiclestudio"]:getImages({
  "adder",
  "zentorno",
  "t20",
})
```

**Example result**

```lua
{
  adder = {
    image = "https://images.example.com/jg-vehiclestudio/image/adder.webp?v=1710000000",
    fallbacks = {
      "https://docs.fivem.net/vehicles/adder.webp",
      "https://cfx-nui-jg-vehiclestudio/web/dist/no-vehicle-image.png",
    },
  },
  zentorno = {
    image = nil,
    fallbacks = {
      "https://docs.fivem.net/vehicles/zentorno.webp",
      "https://cfx-nui-jg-vehiclestudio/web/dist/no-vehicle-image.png",
    },
  },
}
```

### Image IDs

`imageId` is the image set ID.

If you do not pass an `imageId`, Vehicle Studio uses `"default"`.

```lua
local defaultImage = exports["jg-vehiclestudio"]:getImage("adder")
local orangeImage = exports["jg-vehiclestudio"]:getImage("adder", "orange_bg")
```

Use the same image ID you selected when photographing the vehicle in Vehicle Studio.

### Browser/NUI fallback handling

NUI integrations should try the primary `image` first, then each fallback URL in order. Use the first URL that successfully loads.

```ts
interface VehicleImageCandidates {
  image?: string | null;
  fallbacks?: string[];
}

const imageProbeCache = new Map<string, boolean>();

function probeImage(url: string): Promise<boolean> {
  const cached = imageProbeCache.get(url);
  if (cached !== undefined) return Promise.resolve(cached);

  return new Promise((resolve) => {
    const img = new Image();

    img.onload = () => {
      imageProbeCache.set(url, true);
      resolve(true);
    };

    img.onerror = () => {
      imageProbeCache.set(url, false);
      resolve(false);
    };

    img.src = url;
  });
}

export async function resolveVehicleImage(
  candidates: VehicleImageCandidates,
): Promise<string | null> {
  const urls = [candidates.image, ...(candidates.fallbacks ?? [])].filter(
    (url): url is string => typeof url === "string" && url.length > 0,
  );

  for (const url of urls) {
    if (await probeImage(url)) return url;
  }

  return null;
}
```

