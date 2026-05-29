# Fivemanage

Fivemanage is the simplest hosted option because it is designed around FiveM resources. Vehicle Studio asks Fivemanage for a presigned upload URL, then the NUI uploads the image directly to Fivemanage.

### 1. Create A Fivemanage API Token

1. Open the [Fivemanage dashboard](https://fivemanage.com/).
2. Sign in or create an account.
3. Go to your API token or developer settings page.
4. Create a new API token.
5. Copy the token into `config/config.upload.lua`.

The exact dashboard labels may change over time, but you are looking for an API token that allows file uploads.

### 2. Configure Vehicle Studio

In `config/config.lua`:

```lua
Config.ImageStorageProvider = "fivemanage"
```

In `config/config.upload.lua`:

```lua
Config.ImageStorageProviders = Config.ImageStorageProviders or {}

Config.ImageStorageProviders.fivemanage = {
  apiKey = "YOUR_FIVEMANAGE_API_TOKEN",
  presignedEndpoint = "https://api.fivemanage.com/api/v3/file/presigned-url",
  path = "vehicle-studio",
  metadata = "",
  retentionExempt = false,
  presignExpires = 900
}
```

### 3. Test The Upload

After restarting the resource, generate a vehicle image. Fivemanage returns the final public URL, and Vehicle Studio saves that URL directly.

If uploads fail, check:

* The API token was copied correctly.
* The token has upload access.
* The `path` value is valid for your Fivemanage account.
* The server can make outbound HTTPS requests to create the presigned URL.

### Config Fields

| Field               | Required | Description                                                                                |
| ------------------- | -------- | ------------------------------------------------------------------------------------------ |
| `apiKey`            | Yes      | Fivemanage API token used to create presigned upload URLs.                                 |
| `presignedEndpoint` | No       | Defaults to `"https://api.fivemanage.com/api/v3/file/presigned-url"`.                      |
| `path`              | No       | Folder path to upload into. Defaults to no folder if omitted.                              |
| `metadata`          | No       | Optional metadata JSON string sent with the upload.                                        |
| `retentionExempt`   | No       | Optional Fivemanage retention setting.                                                     |
| `presignExpires`    | No       | Presigned upload URL lifetime in seconds. Defaults to Fivemanage's API default if omitted. |

### Troubleshooting

#### Fivemanage Returns Unauthorized

Check that `Config.ImageStorageProviders.fivemanage.apiKey` is correct and still active.

#### Upload Prepare Fails

Check that:

* `Config.ImageStorageProvider` is set to `"fivemanage"`.
* `config/config.upload.lua` is loaded on the server.
* The server can make outbound HTTPS requests to Fivemanage.
* The Fivemanage API token has not expired or been revoked.
