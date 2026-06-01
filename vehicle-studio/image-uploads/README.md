# Image Uploads

Vehicle Studio creates vehicle images in the in-game browser. Those images need to be stored somewhere before they can be shown in the gallery or used by other scripts.

**For live servers, use a remote upload provider.** The local storage option is mainly for localhost testing or advanced users who have their own HTTPS reverse proxy.

{% hint style="danger" %}
Do not use local storage as your live server setup unless you already know how to run a proper HTTPS reverse proxy. A plain public server IP such as `http://123.123.123.123:30120` will be blocked by the browser.
{% endhint %}

The main provider switch lives in `config/config.lua`:

```lua
Config.ImageStorageProvider = "qbox"
```

API keys and private provider settings live in `config/config.upload.lua`, which is loaded on the server only. Keep that file private and do not commit it to a public repository.

### Which Option Should I Use?

If your server is live, choose one of these:

```lua
Config.ImageStorageProvider = "qbox"
Config.ImageStorageProvider = "fivemanage"
Config.ImageStorageProvider = "r2"
Config.ImageStorageProvider = "s3"
```

If you are only testing on your own machine, you can use:

```lua
Config.ImageStorageProvider = "local"
```

### Provider Options

| Provider      | Config value   | Live server?                           | Best for                                     | Guide                           |
| ------------- | -------------- | -------------------------------------- | -------------------------------------------- | ------------------------------- |
| Qbox CDN      | `"qbox"`       | ✅ Yes                                  | Simple hosted uploads through Qbox           | [Setup guide](qbox-cdn.md)      |
| Fivemanage    | `"fivemanage"` | ✅ Yes                                  | FiveM-focused hosted image uploads           | [Setup guide](fivemanage.md)    |
| Cloudflare R2 | `"r2"`         | ✅ Yes                                  | Cloudflare object storage and custom domains | [Setup guide](cloudflare-r2.md) |
| AWS S3        | `"s3"`         | ✅ Yes                                  | Existing AWS setups and custom CDN pipelines | [Setup guide](aws-s3.md)        |
| Local storage | `"local"`      | ⚠️ No, except with HTTPS reverse proxy | Local-only image generation                  | [Setup guide](local-storage.md) |

### Setup Order

1. Pick a provider from the table above.
2. Set `Config.ImageStorageProvider` in `config/config.lua`.
3. Fill in the matching provider block in `config/config.upload.lua` if the provider needs private settings.
4. Restart the resource after changing upload settings.
5. Open Vehicle Studio and take one test photo before running bulk photography.

### Why Local Storage Is Not Recommended For Live Servers

Local storage uses Vehicle Studio's built-in FiveM HTTP endpoint. The in-game browser must upload image files to that endpoint and load gallery images back from it.

On live servers, this usually fails because the Vehicle Studio UI is loaded from an HTTPS page such as:

```
https://cfx-nui-jg-vehiclestudio/web/dist/index.html
```

Browsers block that HTTPS page from sending uploads to a plain HTTP public IP such as:

```
http://123.123.123.123:30120/jg-vehiclestudio/save
```

That browser rule is called mixed content blocking. It happens before the request reaches Vehicle Studio.

`localhost` is different. Browsers treat localhost as a trusted local address, so local storage can work for development:

```lua
Config.HttpBaseUrl = "http://localhost:30120"
```

The generated Cfx proxy URL, such as `https://something.users.cfx.re`, is also not a good local storage fix. It is rate limited and not designed for large image upload POST requests.

### Remote Batch Upload Queue

Batch photography can upload remote images concurrently for efficiency.

The queue settings live in `config/config.upload.lua`:

```lua
Config.RemoteImageUploadQueue = {
  enabled = true,
  concurrency = 3,
  maxPendingUploads = 6,
  maxAttempts = 5,
  retryBaseDelayMs = 500,
  retryMaxDelayMs = 8000,
}
```

`concurrency` controls how many remote uploads can run at the same time. `maxPendingUploads` controls how many captured images can be held by the upload queue before the batch runner pauses and waits for uploads to catch up. `maxAttempts` is the total number of tries for each upload phase, so `1` means no retry.

If you are experiencing issues, set the following to make remote batch uploads happen one at a time.

```lua
Config.RemoteImageUploadQueue = {
  enabled = true,
  concurrency = 1,
  maxPendingUploads = 1,
  maxAttempts = 1,
  retryBaseDelayMs = 1,
  retryMaxDelayMs = 1,
}
```

### ⚠️ Keep Secrets Private

Never put real API keys in Git commits, support tickets, Discord screenshots, or client-side Lua files.

Only `config/config.upload.lua` should contain upload credentials.

### General Troubleshooting

#### Vehicle Studio Says It Cannot Connect To The Server

This usually means you are using `Config.ImageStorageProvider = "local"` and the browser cannot reach the local HTTP endpoint.

For live servers, switch to Qbox CDN, Fivemanage, R2, or S3.

For localhost testing, check the local storage guide.

#### Browser Console Shows Mixed Content

You are trying to use local storage through a plain HTTP public IP. This is blocked by the browser. Use a remote provider, or use local storage only behind a real HTTPS reverse proxy.

#### Browser Console Shows A CORS Error

For S3 or R2, the bucket CORS rules are missing or the allowed origin does not match your resource. Check the CORS section in the provider guide.

For local storage, check the local storage guide.

#### The Resource Says The Provider Is Missing

Check `Config.ImageStorageProvider` in `config/config.lua`. It must match one of:

```txt
local
qbox
fivemanage
r2
s3
```
