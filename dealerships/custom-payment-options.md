# Custom Payment Options

{% hint style="warning" %}
Available in Dealerships v1.4 and newer.
{% endhint %}

You can now add custom payment options to Dealerships, so no longer will players only have the option to use either cash or their bank balance. You could add black money, VIP coins, cryptocurrencies, or whatever! You can add code for whatever you'd like. Let your imagination run wild! This tutorial is split into two parts; the first part is how to update the config files, and the second part explains how you can add code for fetching and deducting from your custom payment option.

We will add an option called `water`in this example with the use of Ox Inventory.

### Part 1: Configuration

1. Find the dealership you want to add another payment option to. \

2. Add this into the dealership

```lua
paymentOptions = {"cash", "bank", "NewOption"},
```

3. In this example we'll add `water`. So we change `NewOption` to `water`

```lua
paymentOptions = {"cash", "bank", "water"},
```

4. Then we need to go into our language file and add a translation for `water`

```lua
water = "Water",
```

5. It should now show in the dealership's payment options!

![](<../.gitbook/assets/image (35).png>)



### Part 2: Adding get/removal code for the payment option

Now we need to add some code to our framework files to tell JG Dealerships where to **fetch** the value of water and how to **deduct** from it.

1. Find the `Framework.Client.GetBalance` function located in `framework/cl-functions.lua` \

2. In the first line we need to change "custom" to "water". Then we need to add our functionality to retrieve and return the amount of water which the client has

```lua
if type == "water" then
    return exports.ox_inventory:GetItemCount("water")
    -- Add your own custom balance system here insted (eg: return 0)
elseif Config.Framework == "QBCore" then
```

3. Now go to `sv-functions.lua` and find `Framework.Server.GetPlayerBalance` \

4. Then we need to do the exact same as we did in the cl-functions.lua. change "custom" to "water" and return the correct amount of water

```lua
if type == "water" then
    return exports.ox_inventory:GetItemCount(src, "water")
    -- Add your own custom balance system here instead (eg: return 0)
elseif Config.Framework == "QBCore" or Config.Framework == "Qbox" then
```

5. Go down to `Framework.Server.PlayerRemoveMoney`&#x20;
6. Change "custom" to "water" again and now we need to create code for the removal of water

```lua
if account == "water" then
    exports.ox_inventory:RemoveItem(src, "water", amount)
    -- Add your own custom balance system here instead
elseif Config.Framework == "QBCore" or Config.Framework == "Qbox" then
```

And then you're all set!
