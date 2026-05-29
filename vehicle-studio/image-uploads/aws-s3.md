# AWS S3

S3 from Amazon Web Services (AWS) is the most flexible option, but it is also the easiest to misconfigure. Vehicle Studio signs a short-lived S3 PUT URL, then the NUI uploads the image directly to S3.

You need four things to work at the same time:

| Step | What you are setting up   | Why it matters                                |
| ---- | ------------------------- | --------------------------------------------- |
| 1    | Bucket                    | The place images are stored                   |
| 2    | IAM access key            | Lets the server create presigned upload URLs  |
| 3    | CORS                      | Lets the NUI upload directly from the browser |
| 4    | Public read access or CDN | Lets browsers load the saved image URL        |

### The Two URLs To Understand

S3 has an upload URL and a public URL.

The upload URL is a short-lived presigned URL. Vehicle Studio creates it on the server, then the NUI uses it once to upload the image directly to S3.

The public URL is the final URL saved for the image. This is what players' browsers and other resources load later. In most setups it looks like one of these:

```txt
https://my-vehicle-images.s3.eu-west-2.amazonaws.com
https://cdn.example.com
```

If you are using CloudFront or another CDN, put the CDN domain in `publicUrl`.

Vehicle Studio appends a small `v=` query parameter to saved image URLs each time an image is retaken. The object path stays the same, but browsers are forced to fetch the newest version.

### 1. Create An S3 Bucket

1. Open the [AWS S3 console](https://s3.console.aws.amazon.com/s3/).
2. Create a bucket, for example `my-vehicle-images`.
3. Pick the AWS region closest to your server, for example `eu-west-2`.
4. Save the bucket name and region.

Bucket names are globally unique, so your bucket name must be different from everyone else's AWS bucket names.

### 2. Create An IAM User Or Access Key

Vehicle Studio needs an access key ID and secret access key that can create presigned PUT URLs for the bucket.

AWS documentation:

* [Create access keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)
* [IAM policies for S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-iam.html)

This example allows uploads and reads inside the `vehicle-studio/` folder only:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:PutObject", "s3:GetObject"],
      "Resource": "arn:aws:s3:::my-vehicle-images/vehicle-studio/*"
    }
  ]
}
```

Replace `my-vehicle-images` with your bucket name. Replace `vehicle-studio/` if you use a different `prefix`.

### 3. Enable CORS For Direct Uploads

Because the NUI uploads directly to S3, the bucket must allow browser `PUT` requests from your resource.

In the S3 bucket, open **Permissions** > **Cross-origin resource sharing (CORS)** and use this as a starting point:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["PUT"],
    "AllowedOrigins": ["https://cfx-nui-jg-vehiclestudio"],
    "ExposeHeaders": ["ETag"],
    "MaxAgeSeconds": 3000
  }
]
```

If you renamed the resource, replace `jg-vehiclestudio` in the origin. For quick testing, you can temporarily use `"*"` as the allowed origin, then tighten it afterward.

### 4. Make Uploaded Images Public

This step controls whether the final image URL can actually be loaded in a browser.

For a simple public bucket setup, add a bucket policy like this:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadVehicleStudioImages",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-vehicle-images/vehicle-studio/*"
    }
  ]
}
```

If AWS blocks this policy, check the bucket's **Block Public Access** settings. AWS blocks public bucket policies by default in many setups.

If you do not want a public bucket, use CloudFront or another CDN in front of the bucket instead. In that case, set `publicUrl` to the CDN domain.

AWS documentation:

* [Bucket policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html)
* [Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html)
* [CloudFront with S3](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/DownloadDistS3AndCustomOrigins.html)

### 5. Configure Vehicle Studio

In `config/config.lua`:

```lua
Config.ImageStorageProvider = "s3"
```

In `config/config.upload.lua`:

```lua
Config.ImageStorageProviders = Config.ImageStorageProviders or {}

Config.ImageStorageProviders.s3 = {
  bucket = "my-vehicle-images",
  region = "eu-west-2",
  accessKeyId = "YOUR_AWS_ACCESS_KEY_ID",
  secretAccessKey = "YOUR_AWS_SECRET_ACCESS_KEY",
  publicUrl = "https://my-vehicle-images.s3.eu-west-2.amazonaws.com",
  prefix = "vehicle-studio",
  acl = nil,
  endpoint = nil,
  forcePathStyle = false,
  presignExpires = 900
}
```

If you use CloudFront:

```lua
publicUrl = "https://cdn.example.com"
```

If you use a custom S3-compatible endpoint, set `endpoint`. Normal AWS S3 does not need it.

### 6. Test The Final URL

After restarting the resource, generate one image and open the saved image URL in a browser.

For example:

```txt
https://my-vehicle-images.s3.eu-west-2.amazonaws.com/vehicle-studio/adder.webp
```

If that URL does not open publicly, the issue is usually the bucket policy, Block Public Access settings, CDN setup, or an incorrect `publicUrl`.

### Config Fields

| Field             | Required | Description                                                                |
| ----------------- | -------- | -------------------------------------------------------------------------- |
| `bucket`          | Yes      | S3 bucket name.                                                            |
| `region`          | Yes      | AWS region, for example `"eu-west-2"`.                                     |
| `accessKeyId`     | Yes      | AWS access key ID.                                                         |
| `secretAccessKey` | Yes      | AWS secret access key.                                                     |
| `publicUrl`       | Yes      | Public read URL used to build the final saved image URL.                   |
| `prefix`          | No       | Folder-style prefix for uploaded images, for example `"vehicle-studio"`.   |
| `acl`             | No       | Optional canned ACL. Leave `nil` unless you know the provider requires it. |
| `endpoint`        | No       | Optional custom S3-compatible endpoint. Normal AWS S3 does not need this.  |
| `forcePathStyle`  | No       | Optional for S3-compatible providers.                                      |
| `presignExpires`  | No       | Presigned upload URL lifetime in seconds. Defaults to `900`.               |

### Troubleshooting

#### S3 Uploads Fail With Signature Errors

Check:

* The bucket region.
* The endpoint, if you configured one.
* The access key and secret key.
* The system clock on the server.
* Whether the browser is sending the exact `Content-Type` from the presigned upload plan.

#### Upload Succeeds But Images Do Not Display

Check `publicUrl`. It must be the public read URL that can load the uploaded image directly in a browser.

Examples:

```lua
publicUrl = "https://my-vehicle-images.s3.eu-west-2.amazonaws.com"
publicUrl = "https://cdn.example.com"
```

Do not include a trailing slash.

Also check the bucket policy, Block Public Access settings, or CDN origin configuration.
