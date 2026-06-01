# Installation

### 1. Installation

1. Unzip the `jg-vehiclestudio-bundle`.
2. Drag the script folder (`jg-vehiclestudio`) into a new folder called `[jg]` within your server's `resources` folder.
3. Make sure you have the latest version of [ox\_lib](https://github.com/overextended/ox_lib/releases/latest) installed.
4. Optional: If you are planning to use a database for data storage, see Data Storage and make sure [oxmysql](https://github.com/overextended/oxmysql/releases/latest) is installed on your server.
5. Inside your `server.cfg`, add a new line after your framework, dependencies, and other required resources have started:

```
ensure [jg]
```

### 2. Configuration

Inside the `config` folder you will find the main configuration files.

The main file is:

```
config/config.lua
```

Private upload credentials live in:

```
config/config.upload.lua
```

Do not put real upload API keys in public GitHub repositories, support tickets, or screenshots.

### 3. Pick Image Upload Storage

Before using Vehicle Studio on a live server, choose where generated images should be stored.

For live servers, we strongly recommend a remote upload provider:

```lua
Config.ImageStorageProvider = "qbox"
Config.ImageStorageProvider = "fivemanage"
Config.ImageStorageProvider = "r2"
Config.ImageStorageProvider = "s3"
```

Local storage is mainly for localhost testing:

```lua
Config.ImageStorageProvider = "local"
```

**⚠️ Local storage does not work out of the box on most live servers because the in-game browser blocks uploads to plain HTTP public IPs.** Only use local storage on a live server if you have set up your own HTTPS reverse proxy.

Read the image upload guide before taking lots of photos:

* [Image Uploads Docs](image-uploads/)
* [Local Storage Guide](image-uploads/local-storage.md)

### 4. Data Storage

Vehicle Studio can store its data in local files or in a database.

If you want to change where data is stored, follow the [Data Storage Docs](data-storage.md).

### 5. Quick Start

Our quick start guide makes it easy to get up and running with JG Vehicle Studio:

{% content-ref url="quick-start.md" %}
[quick-start.md](quick-start.md)
{% endcontent-ref %}

