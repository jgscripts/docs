# Local Storage

Local storage saves generated images inside the Vehicle Studio resource folder. It is useful for localhost testing, but it is not the recommended setup for live servers.

{% hint style="danger" %}
If your server is live and players connect from outside your own machine, use Qbox CDN, Fivemanage, Cloudflare R2, or AWS S3 instead. Local storage will not work out of the box on most live servers.
{% endhint %}

In `config/config.lua`:

```lua
Config.ImageStorageProvider = "local"
```

Images are saved to:

```
exported_images/
```

Vehicle Studio then serves those files through its built-in FiveM HTTP endpoint.

### When To Use Local Storage

Use local storage if:

* You are testing Vehicle Studio on localhost.
* You are developing locally.
* You are an advanced user and already have a proper HTTPS reverse proxy.

Do not use local storage if:

* Your server is live and you want the simplest reliable setup.
* You are trying to use a public IP with `http://`.
* You are trying to use a `users.cfx.re` proxy URL.
* You do not know what an HTTPS reverse proxy is.

For live servers, choose one of these instead:

```lua
Config.ImageStorageProvider = "qbox"
Config.ImageStorageProvider = "fivemanage"
Config.ImageStorageProvider = "r2"
Config.ImageStorageProvider = "s3"
```

### Why Local Storage Fails On Live Servers

Vehicle Studio runs inside the in-game browser. That browser loads the UI from an HTTPS page, for example:

```
https://cfx-nui-jg-vehiclestudio/web/dist/index.html
```

When local storage is enabled, the browser must upload image files to the Vehicle Studio HTTP endpoint, for example:

```
http://123.123.123.123:30120/jg-vehiclestudio/save
```

Browsers block HTTPS pages from sending requests to plain HTTP public IPs. This is called mixed content blocking.

That means this is not supported for live servers:

```lua
Config.HttpBaseUrl = "http://123.123.123.123:30120"
```

`localhost` is treated differently by browsers, which is why this can work for local testing:

```lua
Config.HttpBaseUrl = "http://localhost:30120"
```

### Basic Localhost Setup

For most local development setups, leave `Config.HttpBaseUrl` empty:

```lua
Config.ImageStorageProvider = "local"
Config.HttpBaseUrl = nil
```

Vehicle Studio will try to detect the local endpoint automatically.

If automatic detection fails while testing locally, set:

```lua
Config.HttpBaseUrl = "http://localhost:30120"
```

Replace `30120` if your server uses a different port.

### Do Not Use The Cfx Proxy

Do not set `Config.HttpBaseUrl` to a generated Cfx proxy URL:

```lua
Config.HttpBaseUrl = "https://something.users.cfx.re"
```

That URL may look useful because it uses HTTPS, but it is not suitable for image uploads. The Cfx proxy is rate limited and is not designed for large POST upload traffic.

### Advanced Live Server Setup

Only use local storage on a live server if you can provide your own HTTPS reverse proxy.

Example:

```lua
Config.ImageStorageProvider = "local"
Config.HttpBaseUrl = "https://images.example.com"
```

Do not include the resource name in `Config.HttpBaseUrl`. Vehicle Studio adds it automatically.

So this is correct:

```lua
Config.HttpBaseUrl = "https://images.example.com"
```

This is not correct:

```lua
Config.HttpBaseUrl = "https://images.example.com/jg-vehiclestudio"
```

Your proxy must forward requests like these to the FiveM resource HTTP API:

```
https://images.example.com/jg-vehiclestudio/health
https://images.example.com/jg-vehiclestudio/save
https://images.example.com/jg-vehiclestudio/image/adorned.webp
```

The proxy must:

* Use HTTPS with a valid certificate.
* Allow `GET`, `POST`, `DELETE`, and `OPTIONS`.
* Allow large enough POST bodies for image uploads.
* Avoid strict rate limits that would break batch photography.
* Forward headers such as `Content-Type`, `X-Vehicle-Model`, `X-Image-Format`, `X-Image-Id`, and `X-Preset-Id`.
* Return valid CORS headers.

If this list does not make sense, do not use local storage on a live server. Use a remote provider instead.

### Troubleshooting

**Vehicle Studio Says It Cannot Connect To The Server**

This comes from the local HTTP endpoint health check.

If you are on a live server, the simplest fix is to stop using local storage and switch to Qbox CDN, Fivemanage, R2, or S3.

If you are testing locally, check:

* `Config.ImageStorageProvider` is set to `"local"`.
* `Config.HttpBaseUrl` is empty or points to `http://localhost:30120`.
* The port matches your FiveM server port.
* The resource was restarted after changing config.

**Browser Console Shows Mixed Content**

You are using a plain HTTP public IP. The browser blocks this before Vehicle Studio receives the request.

Use a remote provider or put local storage behind a real HTTPS reverse proxy.

**Browser Console Shows A CORS Error**

For local storage, this usually means `Config.HttpBaseUrl` points to the wrong place or your reverse proxy is not handling CORS correctly.

Check that:

* The URL does not include `/jg-vehiclestudio`.
* The URL is reachable from the game client.
* The proxy allows upload POST requests.
* The proxy returns valid CORS headers.

**Local Images Do Not Load**

Check that:

* Files are being written to `exported_images/`.
* The configured URL can be reached from the game client.
* The server firewall allows access to the selected port or proxy.
* Any reverse proxy forwards requests to the FiveM HTTP server.
