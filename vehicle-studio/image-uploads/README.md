# Image Uploads

Vehicle Studio can save generated vehicle images locally, or upload them directly to a hosted image provider.

The main provider switch lives in `config/config.lua`:

```lua
Config.ImageStorageProvider = "local"
```

API keys and private provider settings live in `config/config.upload.lua`, which is loaded on the server only. Keep that file private and do not commit it to a public repository.

### Provider Options

<table><thead><tr><th>Provider</th><th width="131">Config value</th><th width="100">Difficulty</th><th>Best for</th><th>Guide</th></tr></thead><tbody><tr><td>Local storage</td><td><code>"local"</code></td><td>Easiest</td><td>Default installs, private servers, testing</td><td><a href="local-storage.md">Setup guide</a></td></tr><tr><td>Fivemanage</td><td><code>"fivemanage"</code></td><td>Easy</td><td>FiveM-focused hosted image uploads</td><td><a href="fivemanage.md">Setup guide</a></td></tr><tr><td>Cloudflare R2</td><td><code>"r2"</code></td><td>Medium</td><td>S3-compatible uploads without AWS complexity</td><td><a href="cloudflare-r2.md">Setup guide</a></td></tr><tr><td>AWS S3</td><td><code>"s3"</code></td><td>Hardest</td><td>Existing AWS setups, custom CDN pipelines</td><td><a href="aws-s3.md">Setup guide</a></td></tr></tbody></table>

### Setup Order

1. Pick a provider from the table above.
2. Set `Config.ImageStorageProvider` in `config/config.lua`.
3. Fill in the matching provider block in `config/config.upload.lua` if the provider needs private settings.
4. Restart the resource after changing upload settings.

### Remote Batch Upload Queue

Batch photography can upload remote images concurrently for efficiency.

The queue settings live in `config/config.lua`:

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

### General Troubleshooting

#### The Resource Says The Provider Is Missing

Check `Config.ImageStorageProvider` in `config/config.lua`. It must match one of:

```txt
local
fivemanage
r2
s3
```

#### Browser Console Shows A CORS Error

For S3 or R2, the bucket CORS rules are missing or the allowed origin does not match your resource. Check the CORS section in the provider guide.

For local storage, check the local storage guide because the built-in FiveM HTTP endpoint is part of that provider's setup.

#### :warning: Keep Secrets Private

Never put real API keys in Git commits, support tickets, discord screenshots or client-side Lua files.

Only `config/config.upload.lua` should contain upload credentials.
