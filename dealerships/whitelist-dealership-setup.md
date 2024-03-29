---
description: A template to help configure a whitelisted dealership
---

# Whitelist Dealership Setup

Example is for Police Job!

<pre class="language-lua"><code class="lang-lua"><strong>-- Recommended specifying vehicles category as job name
</strong><strong>Config.Categories = {
</strong>  police = "Police",
}  

-- WHITELISTED DEALERSHIP
  ["Police"] = {
    type = "self-service", -- or "owned", "self-service"
    openShowroom = vector3(459.33, -1103.84, 29.2),
    purchaseSpawn = vector4(478.43, -1093.1, 29.2, 357.26),
    testDriveSpawn = vector4(478.43, -1093.1, 29.2, 357.26),
    --openManagement = vector3(472.26, -1114.73, 29.4),
    --sellVehicle = vector4(484.39, -1081.98, 29.2, 270.53),
    camera = {
      name = "Car",
      coords = vector4(445.81, -1097.49, 43.07, 170.91),
      positions = {5.0, 8.0, 12.0, 8.0}
    },
    categories = {"police"},
    enableTestDrive = false,
    enableFinance = false,
    hideBlip = false,
    blip = {
      id = 523,
      color = 2,
      scale = 0.6
    },
    hideMarkers = false,
    enableSellVehicle = false,
    sellVehiclePercent = 0.6,
    enableFinance = false,
    markers = { id = 21, size = { x = 0.3, y = 0.3, z = 0.3 }, color = { r = 255, g = 255, b = 255, a = 120 }, bobUpAndDown = 0, faceCamera = 0, rotate = 1, drawOnEnts = 0 },
    -- Job Name &#x26; Rank that can open the dealership
    showroomJobWhitelist = {
      police = {0, 1, 2, 3, 4}
    },
    showroomGangWhitelist = {},
    -- Job Name &#x26; Rank that use job society funds to purchase vehicles
    societyPurchaseJobWhitelist = {
      police = {0, 1, 2, 3, 4}
    },
    societyPurchaseGangWhitelist = {},
  },
</code></pre>
