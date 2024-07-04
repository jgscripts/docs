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
    openShowroom = {
      coords = vector3(459.33, -1103.84, 29.2),
      size = 5
    },
    --[[
    openManagement = {
      coords = vector3(1184.45, -3179.27, 7.1),
      size = 5
    },
    sellVehicle = {
      coords = vector3(1196.75, -3205.31, 6.0),
      size = 5
    },
    ]]--
    purchaseSpawn = vector4(478.43, -1093.1, 29.2, 357.26),
    testDriveSpawn = vector4(478.43, -1093.1, 29.2, 357.26),
    camera = {
      name = "Car",
      coords = vector4(445.81, -1097.49, 43.07, 170.91),
      positions = {7.5, 12.0, 15.0, 12.0}
    },
    categories = {"police"},
    enableSellVehicle = false, -- Allow players to sell vehicles back to dealer
    sellVehiclePercent = 0.6,  -- 60% of current sale price
    enableTestDrive = true,
    enableFinance = true,
    hideBlip = false,
    blip = {
      id = 477,
      color = 2,
      scale = 0.6
    },
    hideMarkers = false,
    markers = { id = 21, size = { x = 0.3, y = 0.3, z = 0.3 }, color = { r = 255, g = 255, b = 255, a = 120 }, bobUpAndDown = 0, faceCamera = 0, rotate = 1, drawOnEnts = 0 },
    showroomJobWhitelist = {
      -- Job Name &#x26; Rank that can open the dealership
      police = {0, 1, 2, 3, 4}
    },
    showroomGangWhitelist = {},
    societyPurchaseJobWhitelist = {
      -- Job Name &#x26; Rank that use job society funds to purchase vehicles
      police = {0, 1, 2, 3, 4}
    },
    societyPurchaseGangWhitelist = {},
  },
</code></pre>
