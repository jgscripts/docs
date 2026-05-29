# Data Storage

Vehicle Studio can store its saved data in local resource files or in a MySQL database through `oxmysql`.

The data storage switch lives in `config/config.lua`:

```lua
Config.DataStorage = "local"
```

Valid values are:

| Value        | Requires `oxmysql` | Best for                                      |
| ------------ | ------------------ | --------------------------------------------- |
| `"local"`    | No                 | Simple installs, testing, single-server usage |
| `"database"` | Yes                | Shared data, easier backups, production usage |

{% hint style="info" %}
Image uploads are configured separately with `Config.ImageStorageProvider`. Data storage decides where Vehicle Studio stores metadata such as vehicle image references, presets, and settings. It does not make preset thumbnails or generated image files database blobs.
{% endhint %}

### What Gets Stored

Vehicle Studio stores:

* Vehicle gallery index data.
* Image references and public image URLs.
* Presets.
* Preset thumbnail references.
* Gallery settings.

Generated image files and preset thumbnail files use the configured image upload provider:

* `"local"` saves files inside the resource.
* `"s3"`, `"r2"`, and `"fivemanage"` save files remotely and store the returned URL as a reference.

### Local Storage

Local storage is the default option and does not require any database setup.

In `config/config.lua`:

```lua
Config.DataStorage = "local"
```

With local data storage, Vehicle Studio writes JSON data inside the resource.

Typical local data files include:

* `exported_images/index.json`
* `presets/index.json`
* `presets/<preset-id>.json`
* `data/settings.json`

Use local storage if:

* You want the simplest setup.
* You do not want `oxmysql` as a dependency.
* You are testing the resource locally.
* You are only running one server instance.

#### Local Storage Notes

Do not commit your live `config/` folder or generated data files. The resource includes `config_example/` for defaults, while `config/` is intended for your server-specific settings.

If you use local image uploads too, make sure the local image upload guide is also configured correctly.

### Database Storage

Database storage stores Vehicle Studio data in MySQL through `oxmysql`.

In `config/config.lua`:

```lua
Config.DataStorage = "database"
```

You must also start `oxmysql` before Vehicle Studio in `server.cfg`:

```cfg
ensure oxmysql
ensure jg-vehiclestudio
```

Vehicle Studio uses `oxmysql` exports, so `oxmysql` is only required when `Config.DataStorage` is set to `"database"`.

### Database Tables

The database schema lives in:

```txt
sql/jg_vehiclestudio.sql
```

That SQL file is the source of truth for both automatic setup and manual setup.

It creates these tables:

| Table                       | Purpose                              |
| --------------------------- | ------------------------------------ |
| `jg_vehiclestudio_vehicles` | Vehicle entries in the gallery index |
| `jg_vehiclestudio_images`   | Image references for each vehicle    |
| `jg_vehiclestudio_presets`  | Saved preset data and summary fields |
| `jg_vehiclestudio_settings` | Resource settings saved from the UI  |

### Automatic Setup

When database storage is enabled, Vehicle Studio tries to run `sql/jg_vehiclestudio.sql` automatically on startup.

For automatic setup to work:

1. `oxmysql` must be started.
2. The MySQL connection must be valid.
3. The configured database user must have permission to create tables.
4. `sql/jg_vehiclestudio.sql` must be included with the resource.

### Manual Setup

If automatic setup fails, manually run:

```txt
sql/jg_vehiclestudio.sql
```

Run it in your MySQL database using your preferred database tool, then restart Vehicle Studio.

### Troubleshooting

#### Database Storage Says `oxmysql` Is Missing

Check that `oxmysql` is installed and started before Vehicle Studio:

```cfg
ensure oxmysql
ensure jg-vehiclestudio
```

#### Tables Are Not Created Automatically

Check:

* The MySQL connection string used by `oxmysql`.
* The database user has `CREATE TABLE` permission.
* `sql/jg_vehiclestudio.sql` exists in the resource.
* The server console for SQL errors.

You can manually run `sql/jg_vehiclestudio.sql` if needed.

#### Images Or Preset Thumbnails Are Not In The Database

This is expected. Vehicle Studio stores image files through `Config.ImageStorageProvider`, then saves references to those files in local data or database data.

Use the image upload provider docs if generated images or preset thumbnails are not uploading correctly.
