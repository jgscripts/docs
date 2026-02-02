# Custom Payment Options

{% hint style="success" %}
Available in Dealerships v2.0 or newer.
{% endhint %}

This guide explains how to add custom payment methods to your dealership, such as VIP Coins, Tokens, or any other currency from your server.

***

### Quick Start

1. Open `framework/sv-currencies.lua`
2. Scroll to the **bottom** of the file
3. Find the commented example (`-- VIP Coins`)
4. Copy and modify it for your currency

***

### Step-by-Step Guide

#### Step 1: Open the File

Open `framework/sv-currencies.lua` in any text editor.

Scroll all the way to the bottom. You'll see a commented-out example that looks like this:

```lua
-- ============================================================================
-- EXAMPLE: VIP Coins Custom Currency
-- Uncomment and modify this to add your own custom currency!
-- ============================================================================

-- Currencies.Server.Register({
--   id = "vip_coins",
--   ...
-- })
```

#### Step 2: Copy the Example

Copy the entire example block and paste it below. Remove the `--` comment markers to activate it.

#### Step 3: Fill in the Fields

Here's what each field means:

| Field            | Required? | What it does                                        |
| ---------------- | --------- | --------------------------------------------------- |
| `id`             | ✅ Yes     | Unique identifier (e.g., `"vip_coins"`, `"tokens"`) |
| `label`          | ✅ Yes     | Display name shown to players (e.g., `"VIP Coins"`) |
| `format`         | ✅ Yes     | How to display amounts (see examples below)         |
| `conversionRate` | ✅ Yes\*   | How much 1 unit is worth in dollars                 |
| `flatCost`       | ❌ No      | Fixed cost per purchase (overrides conversionRate)  |
| `allowFinance`   | ✅ Yes     | Can players finance with this currency?             |
| `fetchBalance`   | ✅ Yes     | Function to get player's balance                    |
| `addBalance`     | ✅ Yes     | Function to add to player's balance                 |
| `removeBalance`  | ✅ Yes     | Function to remove from player's balance            |

\*Required unless using `flatCost`

***

### Understanding the Fields

#### `format` - How Prices Are Displayed

The `%s` is replaced with the number:

| Format        | Example Output |
| ------------- | -------------- |
| `"$%s"`       | $1,500         |
| `"%s coins"`  | 1,500 coins    |
| `"%s Tokens"` | 1,500 Tokens   |
| `"💎 %s"`     | 💎 1,500       |

#### `conversionRate` - Currency Value

This determines how much 1 unit of your currency is worth in base dollars:

| Rate    | Meaning          | $50,000 car costs... |
| ------- | ---------------- | -------------------- |
| `1`     | 1 coin = $1      | 50,000 coins         |
| `100`   | 1 coin = $100    | 500 coins            |
| `10000` | 1 coin = $10,000 | 5 coins              |
| `50000` | 1 coin = $50,000 | 1 coin               |

#### `flatCost` - Fixed Price Per Purchase (Optional)

If you want **every vehicle to cost the same amount** regardless of its price, use `flatCost`:

```lua
flatCost = 1,  -- Every vehicle costs 1 token
```

With `flatCost = 1`:

* A $10,000 car costs **1 token**
* A $500,000 car costs **1 token**

{% hint style="warning" %}
**Note:** When using `flatCost`, set `allowFinance = false` (you can't split 1 token into payments!)
{% endhint %}

#### `allowFinance` - Financing Support

* `true` = Players can use this currency for financed purchases
* `false` = Full payment only

{% hint style="warning" %}
Must be `false` if using `flatCost`
{% endhint %}

***

### Balance Functions

You need to tell the system how to check and modify player balances. This depends on how your currency is stored.

#### Example: Using a Database Table

If you store coins in a `player_coins` table:

```lua
fetchBalance = function(src)
  local identifier = Framework.Server.GetPlayerIdentifier(src)
  local result = MySQL.scalar.await(
    "SELECT coins FROM player_coins WHERE identifier = ?",
    { identifier }
  )
  return tonumber(result) or 0
end,

addBalance = function(src, amount)
  local identifier = Framework.Server.GetPlayerIdentifier(src)
  MySQL.update.await(
    "UPDATE player_coins SET coins = coins + ? WHERE identifier = ?",
    { amount, identifier }
  )
  return true
end,

removeBalance = function(src, amount)
  local identifier = Framework.Server.GetPlayerIdentifier(src)
  MySQL.update.await(
    "UPDATE player_coins SET coins = coins - ? WHERE identifier = ?",
    { amount, identifier }
  )
  return true
end,
```

#### Example: Using an Export from Another Resource

If another script manages your currency:

```lua
fetchBalance = function(src)
  return exports["my-coin-system"]:GetPlayerCoins(src) or 0
end,

addBalance = function(src, amount)
  return exports["my-coin-system"]:AddCoins(src, amount)
end,

removeBalance = function(src, amount)
  return exports["my-coin-system"]:RemoveCoins(src, amount)
end,
```

***

### Complete Examples

#### Example 1: VIP Coins (Conversion Rate)

1 VIP Coin = $10,000

```lua
Currencies.Server.Register({
  id = "vip_coins",
  label = "VIP Coins",
  format = "%s coins",
  conversionRate = 10000,
  allowFinance = false,

  fetchBalance = function(src)
    return exports["vip-system"]:GetCoins(src) or 0
  end,

  addBalance = function(src, amount)
    exports["vip-system"]:AddCoins(src, amount)
    return true
  end,

  removeBalance = function(src, amount)
    exports["vip-system"]:RemoveCoins(src, amount)
    return true
  end,
})
```

#### Example 2: Car Tokens (Flat Cost)

1 Token = 1 Vehicle (any price)

```lua
Currencies.Server.Register({
  id = "car_token",
  label = "Car Token",
  format = "%s Token(s)",
  conversionRate = 1,
  flatCost = 1,
  allowFinance = false,

  fetchBalance = function(src)
    local identifier = Framework.Server.GetPlayerIdentifier(src)
    local result = MySQL.scalar.await(
      "SELECT tokens FROM player_tokens WHERE identifier = ?",
      { identifier }
    )
    return tonumber(result) or 0
  end,

  addBalance = function(src, amount)
    local identifier = Framework.Server.GetPlayerIdentifier(src)
    MySQL.update.await(
      "UPDATE player_tokens SET tokens = tokens + ? WHERE identifier = ?",
      { amount, identifier }
    )
    return true
  end,

  removeBalance = function(src, amount)
    local identifier = Framework.Server.GetPlayerIdentifier(src)
    MySQL.update.await(
      "UPDATE player_tokens SET tokens = tokens - ? WHERE identifier = ?",
      { amount, identifier }
    )
    return true
  end,
})
```

#### Example 3: Premium Points (with Financing)

1 Point = $1,000, supports financing

```lua
Currencies.Server.Register({
  id = "premium_points",
  label = "Premium Points",
  format = "%s pts",
  conversionRate = 1000,
  allowFinance = true,

  fetchBalance = function(src)
    return exports["premium-shop"]:GetPoints(src) or 0
  end,

  addBalance = function(src, amount)
    exports["premium-shop"]:AddPoints(src, amount)
    return true
  end,

  removeBalance = function(src, amount)
    exports["premium-shop"]:RemovePoints(src, amount)
    return true
  end,

  -- Required for financing: offline balance functions
  fetchBalanceOffline = function(identifier)
    local result = MySQL.scalar.await(
      "SELECT points FROM premium_points WHERE identifier = ?",
      { identifier }
    )
    return tonumber(result) or 0
  end,

  removeBalanceOffline = function(identifier, amount)
    MySQL.update.await(
      "UPDATE premium_points SET points = points - ? WHERE identifier = ?",
      { amount, identifier }
    )
    return true
  end,
})
```

***

### Enabling Your Currency at a Dealership

After adding your currency, you need to enable it for each dealership:

1. Open the **Admin Panel** (`/dealership-admin`)
2. Go to **Locations**
3. Click on a dealership
4. Go to the **Advanced** tab
5. Enable your new payment method in the **Payment Methods** section
6. Save

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

***

### Troubleshooting

<details>

<summary>Currency not showing in the dealership</summary>

* Make sure you've enabled the currency in the dealership's **Advanced** settings
* Restart the resource after adding a new currency

</details>

<details>

<summary>Wrong price showing</summary>

* Check your `conversionRate` value
* If using `flatCost`, make sure it's set correctly

</details>

<details>

<summary>Payment fails even with enough balance</summary>

* Check your `fetchBalance` function returns the correct number
* Check your `removeBalance` function returns `true` on success
* Check the server console for errors

</details>

<details>

<summary>Financing doesn't work with my currency</summary>

* Make sure `allowFinance = true`
* Make sure `flatCost` is NOT set (flat cost currencies can't be financed)
* Add `fetchBalanceOffline` and `removeBalanceOffline` functions for automatic payments

</details>
