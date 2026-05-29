# Cloudflare R2

Cloudflare R2 is a good hosted option if you want S3-compatible storage without setting up AWS (as well as a pretty awesome free tier :eyes:).

Vehicle Studio signs a short-lived R2 PUT URL, then the NUI uploads the image directly to R2.

You will need:

* An R2 bucket.
* An R2 access key ID.
* An R2 secret access key.
* Your Cloudflare account ID.
* A public URL for reading uploaded files.
* CORS enabled for browser PUT uploads.

### 1. Create An R2 Bucket

1. Open the [Cloudflare dashboard](https://dash.cloudflare.com/).
2. Go to **R2 Object Storage**.
3. Create a bucket, for example `vehicle-studio`.
4. Save the bucket name.

### 2. Create R2 API Tokens

1. In the Cloudflare dashboard, open **R2 Object Storage**.
2. Go to **Manage R2 API Tokens**.
3. Create an API token with object read and write access for the bucket.
4. Copy the access key ID and secret access key.

Cloudflare documents this flow here: [R2 API tokens](https://developers.cloudflare.com/r2/api/tokens/).

### 3. Make The Bucket Public

Uploaded images must be reachable by the browser and by other scripts that use the saved image URL.

You can use either:

* An R2 public bucket URL.
* A custom domain connected to the R2 bucket.

Cloudflare's public bucket and custom domain options are documented here: [Public buckets](https://developers.cloudflare.com/r2/buckets/public-buckets/).

### 4. Enable CORS For Direct Uploads

Because the NUI uploads directly to R2, the bucket must allow browser `PUT` requests from your resource.

Use this as a starting point:

```json
[
  {
    "AllowedOrigins": ["https://cfx-nui-jg-vehiclestudio"],
    "AllowedMethods": ["PUT"],
    "AllowedHeaders": ["*"],
    "ExposeHeaders": ["ETag"],
    "MaxAgeSeconds": 3000
  }
]
```

For quick testing, you can temporarily use `"*"` as the allowed origin, then tighten it afterward.

### 5. Configure Vehicle Studio

In `config/config.lua`:

```lua
Config.ImageStorageProvider = "r2"
```

In `config/config.upload.lua`:

```lua
Config.ImageStorageProviders = Config.ImageStorageProviders or {}

Config.ImageStorageProviders.r2 = {
  accountId = "YOUR_CLOUDFLARE_ACCOUNT_ID",
  bucket = "vehicle-studio",
  accessKeyId = "YOUR_R2_ACCESS_KEY_ID",
  secretAccessKey = "YOUR_R2_SECRET_ACCESS_KEY",
  publicUrl = "https://pub-xxxxxxxxxxxxxxxx.r2.dev",
  prefix = "vehicle-studio",
  region = "auto",
  forcePathStyle = true,
  acl = nil,
  presignExpires = 900
}
```

Use your custom domain in `publicUrl` if you configured one (Cloudflare recommends this):

```lua
publicUrl = "https://images.example.com"
```

Cloudflare notes that presigned uploads use the R2 S3 API domain, not a custom domain. `publicUrl` is still the final public URL saved after the upload.

### Config Fields

| Field             | Required | Description                                                                |
| ----------------- | -------- | -------------------------------------------------------------------------- |
| `accountId`       | Yes      | Cloudflare account ID.                                                     |
| `bucket`          | Yes      | R2 bucket name.                                                            |
| `accessKeyId`     | Yes      | R2 access key ID.                                                          |
| `secretAccessKey` | Yes      | R2 secret access key.                                                      |
| `publicUrl`       | Yes      | Public read URL used to build the final saved image URL.                   |
| `prefix`          | No       | Folder-style prefix for uploaded images, for example `"vehicle-studio"`.   |
| `region`          | No       | R2 uses `"auto"`.                                                          |
| `forcePathStyle`  | No       | Required for R2-compatible signing. Defaults should normally stay enabled. |
| `acl`             | No       | Optional canned ACL. Leave `nil` unless you know the provider requires it. |
| `presignExpires`  | No       | Presigned upload URL lifetime in seconds. Defaults to `900`.               |

### Troubleshooting

#### R2 Uploads Fail With Signature Errors

Check:

* The Cloudflare account ID.
* The bucket name.
* The access key and secret key.
* The system clock on the server.
* Whether the NUI is sending the exact `Content-Type` from the presigned upload plan.

#### Upload Succeeds But Images Do Not Display

Check `publicUrl`. It must be the public read URL that can load the uploaded image directly in a browser.

Examples:

```lua
publicUrl = "https://pub-xxxxxxxxxxxxxxxx.r2.dev"
publicUrl = "https://images.example.com"
```

Do not include a trailing slash.

Also check the bucket's public access or custom domain configuration.
