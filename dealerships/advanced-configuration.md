---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Advanced Configuration

Please note that Society Purchase is not available if you do not own our Garages!

{% hint style="danger" %}
Gang configuration is not available for ESX
{% endhint %}

## Information about these sections

1. showroomJobWhitelist = restrict specific jobs to open a dealership
2. showroomGangWhitelist = restrict specific gangs to open a dealership
3. societyPurchaseJobWhitelist = use job society funds to purchase
4. societyPurchaseGangWhitelist = use gang society funds to purchase

## Configure dealership access and society purchases

To setup job or gang restriced dealerships & society purchases please follow the template below

```lua
-- Config.DealershipLocations you can find these config options
-- From this configuration
    
    --showroomJobWhitelist = {},
    --showroomGangWhitelist = {},
    --societyPurchaseJobWhitelist = {},
    --societyPurchaseGangWhitelist = {},    

    -- To this
    showroomJobWhitelist = { -- job name and rank
      -- jobname = {ranknumbers}
      police = {0, 1, 2, 3, 4},
    },
    showroomGangWhitelist = {
      -- gangname = {ranknumbers}
      thelostmc = {0, 1, 2, 3, 4},
    },
    societyPurchaseJobWhitelist = {
      -- jobname = {ranknumbers}
      police = {0, 1, 2, 3, 4},
    },
    societyPurchaseGangWhitelist = {
      -- gangname = {ranknumbers}
      thelostmc = {0, 1, 2, 3, 4},
    },
```
