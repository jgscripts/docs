# Qbox CDN

Qbox CDN is a hosted CDN option for FiveM media. Vehicle Studio asks Qbox for a presigned upload URL, then the NUI uploads the image directly to Qbox without exposing your API key in the browser.

### 1. Create A Qbox CDN API Token

1. Open the [Qbox dashboard](https://dashboard.qbox.re/).
2. Sign in with Discord.
3. Go to the CDN area.
4. Generate a new API token.
5. Copy the token into `config/config.upload.lua`.

The exact dashboard labels may change over time, but you are looking for a CDN API token that can upload files. If Qbox shows token permissions, make sure uploads are allowed. If you want Vehicle Studio to delete remote images when you delete gallery entries, make sure deletes are allowed too.

### 2. Configure Vehicle Studio

In `config/config.lua`:

```lua
Config.ImageStorageProvider = "qbox"
```

In `config/config.upload.lua`:

```lua
Config.ImageStorageProviders = Config.ImageStorageProviders or {}

Config.ImageStorageProviders.qbox = {
  apiKey = "YOUR_QBOX_CDN_API_TOKEN",
  presignedEndpoint = "https://api.qbox.re/v1/file/presigned-url",
  deleteEndpoint = "https://api.qbox.re/v1/file",
  presignExpires = 300
}
```

Qbox accepts either a raw API key or a `Bearer ...` value in `apiKey`. Use the exact value Qbox gives you unless their dashboard tells you to include the `Bearer` prefix.

### 3. Test The Upload

After restarting the resource, generate a vehicle image. Qbox returns the final public URL, and Vehicle Studio saves that URL directly.

If uploads fail, check:

* The API token was copied correctly.
* The token has upload access.
* Your Qbox CDN storage limit has not been reached.
* The server can make outbound HTTPS requests to Qbox.
* The NUI can make outbound HTTPS requests to the presigned Qbox upload URL.

### Config Fields

| Field               | Required | Description                                                                              |
| ------------------- | -------- | ---------------------------------------------------------------------------------------- |
| `apiKey`            | Yes      | Qbox CDN API token used to create presigned upload URLs.                                 |
| `presignedEndpoint` | No       | Defaults to `"https://api.qbox.re/v1/file/presigned-url"`.                               |
| `deleteEndpoint`    | No       | Defaults to `"https://api.qbox.re/v1/file"`. Used when deleting gallery images.          |
| `presignExpires`    | No       | Local pending-upload lifetime in seconds. Qbox currently returns 300 seconds by default. |

### Troubleshooting

#### Qbox Returns Unauthorized

Check that `Config.ImageStorageProviders.qbox.apiKey` is correct and still active. If you pasted only the token value and Qbox requires a bearer value for your token, set it as:

```lua
apiKey = "Bearer YOUR_QBOX_CDN_API_TOKEN"
```

#### Upload Prepare Fails

Check that:

* `Config.ImageStorageProvider` is set to `"qbox"`.
* `config/config.upload.lua` is loaded on the server.
* The server can make outbound HTTPS requests to `https://api.qbox.re`.
* The Qbox CDN API token has not expired or been revoked.

#### Upload Completes But Delete Fails

Vehicle Studio deletes Qbox files through the Qbox API. Check that:

* Your Qbox API token allows deletes.
* `deleteEndpoint` is still set to `https://api.qbox.re/v1/file`, unless Qbox has given you a different endpoint.
* The saved image URL is a Qbox CDN URL so Vehicle Studio can resolve the storage path.
