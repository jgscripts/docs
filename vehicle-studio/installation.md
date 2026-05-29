# Installation

## Installation

1. Unzip the `jg-vehiclestudio-bundle`&#x20;
2. Drag the script folder (`jg-vehiclestudio`) into a new folder called `[jg]` within your server's `resources` folder.
3. Make sure you have the latest version of [ox\_lib](https://github.com/overextended/ox_lib/releases/latest) installed.
4. \[Optional] If you are planning to use a database for data storage (see [Data Storage](data-storage.md)), ensure [oxmysql ](https://github.com/overextended/oxmysql/releases/latest)installed on your server.
5. Inside of your `server.cfg`, add a new line **after** all your other resources have started:

```
ensure [jg]
```

## Configuration

Now for the fun part! Let's get the script perfectly configured for your server. Inside of the `config` folder you will find 2 different configuration files.

The main one is the `config.lua` file; and you don't have to touch this if you want to use the default setup of storing images & data in local files within the resource folder. If you want to change where images or data is stored, follow these guides:

* [Image Uploads Docs](image-uploads/)
* [Data Storage Docs](data-storage.md)

Enjoy!
