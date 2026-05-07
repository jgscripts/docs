# Debug & Event Logging

{% hint style="warning" %}
Available in v1.7.0 and newer.
{% endhint %}

JG Mechanic now uses a central logging system. You only need to edit:

* `config/config.lua` to choose where each logging category is sent.
* `config/config.webhooks.lua` to add Discord webhook URLs.

### Logging Categories

Logging is split into three core routes:

| Route      | Used for                                                                                       |
| ---------- | ---------------------------------------------------------------------------------------------- |
| `LOG`      | Debug logs (can keep this off unless running into issues or asked to by support)               |
| `EVENT`    | Normal audit logs, such as invoices, orders, tuning, shops, repairs, nitrous, and duty changes |
| `SECURITY` | Denied actions, invalid values, suspicious requests, and vehicle data warnings                 |

### Configure Destinations

Set destinations in `config/config.lua`:

```lua
Config.Logs = {
  LOG = {
    enabled = true,
    destinations = { "console" }
  },
  EVENT = {
    enabled = true,
    destinations = { "ox_lib" }
  },
  SECURITY = {
    enabled = true,
    destinations = { "console", "ox_lib" }
  }
}
```

Supported destinations are:

| Destination | Description                                                                                                                                                                                              |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `console`   | Prints to the server console                                                                                                                                                                             |
| `ox_lib`    | <p>Sends to <code>lib.logger</code><br><br>Learn more: <a href="https://overextended.dev/ox_lib/Modules/Logger/Server#liblogger">https://overextended.dev/ox_lib/Modules/Logger/Server#liblogger</a></p> |
| `discord`   | <p>Sends to Discord webhooks from <code>config.webhooks.lua</code></p><p></p><p>(Not recommended)</p>                                                                                                    |

To disable a route:

<pre class="language-lua"><code class="lang-lua">Config.Logs = {
  ...
  LOG = {
    enabled = <a data-footnote-ref href="#user-content-fn-1">false</a>,
    destinations = ...
  }
}
</code></pre>

### Common Examples

Use ox\_lib for normal events and security logs:

```lua
Config.Logs = {
  LOG = {
    enabled = true,
    destinations = { "console" }
  },
  EVENT = {
    enabled = true,
    destinations = { "ox_lib" }
  },
  SECURITY = {
    enabled = true,
    destinations = { "ox_lib" }
  }
}
```

Use Discord for events, but keep security warnings in console and ox\_lib:

```lua
Config.Logs = {
  LOG = {
    enabled = true,
    destinations = { "console" }
  },
  EVENT = {
    enabled = true,
    destinations = { "discord" }
  },
  SECURITY = {
    enabled = true,
    destinations = { "console", "ox_lib" }
  }
}
```

Send events to both Discord and ox\_lib:

```lua
Config.Logs = {
  LOG = {
    enabled = true,
    destinations = { "console" }
  },
  EVENT = {
    enabled = true,
    destinations = { "discord", "ox_lib" }
  },
  SECURITY = {
    enabled = true,
    destinations = { "console", "ox_lib" }
  }
}
```

Disable debug output:

```lua
Config.Logs = {
  LOG = {
    enabled = false,
    destinations = { "console" }
  },
  EVENT = {
    enabled = true,
    destinations = { "ox_lib" }
  },
  SECURITY = {
    enabled = true,
    destinations = { "console", "ox_lib" }
  }
}
```

Visual debug zone markers are separate from logging:

```lua
Config.DebugZones = false
```

### Discord Webhook Setup

Discord webhook URLs are configured in `config/config.webhooks.lua`.

Only add webhook URLs here. Do not put webhook URLs in `config/config.lua`, because `config.lua` is shared with the client.

```lua
Webhooks = {}
Webhooks.SelfService = ""
Webhooks.Orders = ""
Webhooks.TabletTuning = ""
Webhooks.Servicing = ""
Webhooks.Invoices = ""
Webhooks.Mechanic = ""
Webhooks.Admin = ""

Webhooks.Shop = ""
Webhooks.Nitrous = ""
Webhooks.Repair = ""
Webhooks.Duty = ""
Webhooks.VehicleData = ""
Webhooks.Security = ""
```

Discord logs are only sent when the route destination includes `"discord"`.

If a Discord URL is empty, that Discord log is skipped. Other destinations, such as `console` or `ox_lib`, will still work.

### Migrating Discord Webhooks From `server/sv-webhooks.lua`

1. Open your old `server/sv-webhooks.lua`.
2. Copy each old webhook URL into the matching entry in `config/config.webhooks.lua`.
3. Set your route destinations in `config/config.lua`.
4. Add `"discord"` to any route that should send Discord webhook logs.
5. Leave new webhook categories blank unless you want logs for those areas.

Old categories are still available:

| Old webhook             | New config entry        |
| ----------------------- | ----------------------- |
| `Webhooks.SelfService`  | `Webhooks.SelfService`  |
| `Webhooks.Orders`       | `Webhooks.Orders`       |
| `Webhooks.TabletTuning` | `Webhooks.TabletTuning` |
| `Webhooks.Servicing`    | `Webhooks.Servicing`    |
| `Webhooks.Invoices`     | `Webhooks.Invoices`     |
| `Webhooks.Mechanic`     | `Webhooks.Mechanic`     |
| `Webhooks.Admin`        | `Webhooks.Admin`        |

New optional categories are:

| New webhook            | Used for                            |
| ---------------------- | ----------------------------------- |
| `Webhooks.Shop`        | Mechanic shop item purchases        |
| `Webhooks.Nitrous`     | Nitrous bottle installs and refills |
| `Webhooks.Repair`      | Self-service repair purchases       |
| `Webhooks.Duty`        | Mechanic duty toggles               |
| `Webhooks.VehicleData` | Vehicle data and statebag warnings  |
| `Webhooks.Security`    | General security warnings           |

### Discord Appearance

You can also edit the Discord username, avatar, and embed colors in `config/config.webhooks.lua`:

```lua
Config.Logs.Discord = {
  username = "JG Mechanic Webhook",
  avatarUrl = "https://example.com/avatar.png",
  webhooks = Webhooks,
  colors = {
    success = 0x2ecc71,
    danger = 0xe74c3c,
    warning = 0xf1c40f,
    debug = 0x3498db,
    default = 0xff6700
  }
}
```

[^1]: Set enabled = false
