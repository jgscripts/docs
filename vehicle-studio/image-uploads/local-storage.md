# Local Storage

Local storage is the default option and does not need any third-party account.

In `config/config.lua`:

```lua
Config.ImageStorageProvider = "local"
```

**With local storage, images are saved inside the resource to `/exported_images`**, and then served from that folder. No extra settings are required in `config/config.upload.lua`.

Use this provider if:

* You want the simplest setup.
* You are testing the resource locally.
* You are happy to have extra load on your server for serving vehicle images.

### Built-In HTTP Endpoint

Local storage uses Vehicle Studio's built-in FiveM HTTP endpoint to upload image bytes from the NUI and to serve local gallery images back to the browser.

Vehicle Studio tries to detect the endpoint automatically when the gallery opens. If detection fails, the gallery is blocked and shows an endpoint connection error instead of the normal gallery UI.

On hosted servers, set `Config.HttpBaseUrl` in `config/config.lua` if automatic detection cannot reach the raw server URL:

```lua
Config.HttpBaseUrl = "http://123.123.123.123:30120"
Config.HttpBaseUrl = "https://server.example.com:30120"
```

`Config.HttpBaseUrl` should point to the raw FiveM HTTP server or to your own POST-capable reverse proxy. It should include the scheme and port, but not the resource name.

Vehicle Studio automatically appends the resource path, for example `/jg-vehiclestudio`.

Rules:

* Include `http://` or `https://`.
* Include the port when your server or proxy requires it.
* Do not include `/jg-vehiclestudio`; the resource path is appended automatically.
* Do not use the generated `web_baseUrl` / `users.cfx.re` proxy.

The generated Cfx proxy is heavily rate limited and may block POST requests, so it is not suitable for local storage uploads.

### Troubleshooting

#### Vehicle Studio Says It Cannot Connect To The Server

This message comes from the local HTTP endpoint health check.

Check:

* `Config.ImageStorageProvider` is set to `"local"`.
* `Config.HttpBaseUrl` points to the raw server URL or your own POST-capable proxy.
* The URL includes the correct scheme and port.
* The URL does not include the resource path.
* You are not using the generated `users.cfx.re` `web_baseUrl` proxy.

#### Browser Console Shows A CORS Error

A local storage CORS or connection failure usually means `Config.HttpBaseUrl` is missing, points to the wrong host, or points to a proxy that does not allow POST requests.

#### Local Images Do Not Load

Check that:

* The resource was restarted after changing `Config.HttpBaseUrl`.
* The configured URL can be reached from the same machine running the game client.
* The server firewall allows access to the selected port.
* Any reverse proxy forwards requests to the FiveM HTTP server.
