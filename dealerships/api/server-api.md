# Server API

{% hint style="info" %}
**Player Identifiers:** Some exports accept an `identifier` parameter. This is the player's `citizenid` (QBCore/Qbox) or `identifier` (ESX), depending on your framework.
{% endhint %}

{% hint style="info" %}
**Return Pattern:** Mutation exports return `boolean, string?` — the boolean indicates success, and the optional string provides an error message on failure.
{% endhint %}

### Stock Management

#### incrementStock

Increment a vehicle's stock at a specific dealership.

```lua
local success, err = exports['jg-dealerships']:incrementStock(dealership, spawnCode, amount)
```

| Parameter    | Type    | Required | Description                           |
| ------------ | ------- | -------- | ------------------------------------- |
| `dealership` | string  | Yes      | Dealership ID                         |
| `spawnCode`  | string  | Yes      | Vehicle spawn code (e.g. `"adder"`)   |
| `amount`     | integer | No       | Amount to increment by (default: `1`) |

**Returns:** `boolean` success, `string?` error

***

#### decrementStock

Decrement a vehicle's stock at a specific dealership. Blocked if stock would go below 0.

```lua
local success, err = exports['jg-dealerships']:decrementStock(dealership, spawnCode, amount)
```

| Parameter    | Type    | Required | Description                           |
| ------------ | ------- | -------- | ------------------------------------- |
| `dealership` | string  | Yes      | Dealership ID                         |
| `spawnCode`  | string  | Yes      | Vehicle spawn code                    |
| `amount`     | integer | No       | Amount to decrement by (default: `1`) |

**Returns:** `boolean` success, `string?` error

***

#### setStock

Set a vehicle's stock to an exact value at a specific dealership.

```lua
local success, err = exports['jg-dealerships']:setStock(dealership, spawnCode, stock)
```

| Parameter    | Type    | Required | Description                    |
| ------------ | ------- | -------- | ------------------------------ |
| `dealership` | string  | Yes      | Dealership ID                  |
| `spawnCode`  | string  | Yes      | Vehicle spawn code             |
| `stock`      | integer | Yes      | New stock value (must be >= 0) |

**Returns:** `boolean` success, `string?` error

***

#### getVehiclePrice

Get the price of a vehicle. If `dealershipId` is provided, returns the per-dealership price. Otherwise returns the base catalog price.

```lua
local price, err = exports['jg-dealerships']:getVehiclePrice(spawnCode, dealershipId)
```

| Parameter      | Type   | Required | Description                              |
| -------------- | ------ | -------- | ---------------------------------------- |
| `spawnCode`    | string | Yes      | Vehicle spawn code                       |
| `dealershipId` | string | No       | Dealership ID for per-dealership pricing |

**Returns:** `number` price or `false`, `string?` error

**Example:**

```lua
-- Get base catalog price
local price = exports['jg-dealerships']:getVehiclePrice("adder")

-- Get price at a specific dealership
local price = exports['jg-dealerships']:getVehiclePrice("adder", "pdm")
```

***

#### getVehicleStock

Get the current stock level of a vehicle at a specific dealership.

```lua
local stock, err = exports['jg-dealerships']:getVehicleStock(spawnCode, dealershipId)
```

| Parameter      | Type   | Required | Description        |
| -------------- | ------ | -------- | ------------------ |
| `spawnCode`    | string | Yes      | Vehicle spawn code |
| `dealershipId` | string | Yes      | Dealership ID      |

**Returns:** `number` stock or `false`, `string?` error

***

### Finance

#### getPlayerFinancedVehicles

Get all financed vehicles for a player.

```lua
local vehicles = exports['jg-dealerships']:getPlayerFinancedVehicles(identifier)
```

| Parameter    | Type   | Required | Description                                |
| ------------ | ------ | -------- | ------------------------------------------ |
| `identifier` | string | Yes      | Player identifier (citizenid / identifier) |

**Returns:** `table[]` — Array of financed vehicle records. Each record contains `plate`, `financed`, `finance_data` (JSON string), and all other columns from the vehicles table.

**Example:**

```lua
local vehicles = exports['jg-dealerships']:getPlayerFinancedVehicles("ABC12345")
for _, vehicle in ipairs(vehicles) do
  local financeData = json.decode(vehicle.finance_data)
  print(vehicle.plate, financeData.payments_complete .. "/" .. financeData.total_payments)
end
```

***

#### getFinanceByPlate

Get finance details for a specific vehicle by its plate.

```lua
local vehicle = exports['jg-dealerships']:getFinanceByPlate(plate)
```

| Parameter | Type   | Required | Description   |
| --------- | ------ | -------- | ------------- |
| `plate`   | string | Yes      | Vehicle plate |

**Returns:** `table?` — The vehicle record if financed, or `nil` if not found/not financed.

***

#### makeFinancePayment

Programmatically make a finance payment for an online player. Deducts money from their account.

```lua
local success, err = exports['jg-dealerships']:makeFinancePayment(src, plate)
```

| Parameter | Type   | Required | Description                       |
| --------- | ------ | -------- | --------------------------------- |
| `src`     | number | Yes      | Player server ID (must be online) |
| `plate`   | string | Yes      | Vehicle plate                     |

**Returns:** `boolean` success, `string?` error

> **Note:** This deducts the recurring payment amount from the player's account using the currency configured for the finance. The player must be online.

***

#### getPlayerFinanceCount

Get the number of active financed vehicles a player has.

```lua
local count = exports['jg-dealerships']:getPlayerFinanceCount(identifier)
```

| Parameter    | Type   | Required | Description                                |
| ------------ | ------ | -------- | ------------------------------------------ |
| `identifier` | string | Yes      | Player identifier (citizenid / identifier) |

**Returns:** `integer` — Number of financed vehicles.

***

### Dealership Balance

#### getDealershipBalance

Get the current balance of a dealership's account.

```lua
local balance, err = exports['jg-dealerships']:getDealershipBalance(dealershipId)
```

| Parameter      | Type   | Required | Description   |
| -------------- | ------ | -------- | ------------- |
| `dealershipId` | string | Yes      | Dealership ID |

**Returns:** `number` balance or `false`, `string?` error

> **Note:** When using framework jobs, this returns the society/job account balance. Otherwise it returns the `balance` column from `dealership_locations`.

***

#### addDealershipBalance

Add funds to a dealership's account.

```lua
local success, err = exports['jg-dealerships']:addDealershipBalance(dealershipId, amount)
```

| Parameter      | Type   | Required | Description                 |
| -------------- | ------ | -------- | --------------------------- |
| `dealershipId` | string | Yes      | Dealership ID               |
| `amount`       | number | Yes      | Amount to add (must be > 0) |

**Returns:** `boolean` success, `string?` error

***

#### removeDealershipBalance

Remove funds from a dealership's account.

```lua
local success, err = exports['jg-dealerships']:removeDealershipBalance(dealershipId, amount)
```

| Parameter      | Type   | Required | Description                    |
| -------------- | ------ | -------- | ------------------------------ |
| `dealershipId` | string | Yes      | Dealership ID                  |
| `amount`       | number | Yes      | Amount to remove (must be > 0) |

**Returns:** `boolean` success, `string?` error

***

### Employees

#### isEmployee

Check if a player is an employee at a dealership.

```lua
local role = exports['jg-dealerships']:isEmployee(src, dealershipId)
```

| Parameter      | Type   | Required | Description      |
| -------------- | ------ | -------- | ---------------- |
| `src`          | number | Yes      | Player server ID |
| `dealershipId` | string | Yes      | Dealership ID    |

**Returns:** `string` role name (e.g. `"manager"`, `"salesman"`) or `false` if not an employee.

> **Note:** When using framework jobs, this checks the player's current job/grade. When using the built-in employee system, this checks the `dealership_employees` table.

**Example:**

```lua
local role = exports['jg-dealerships']:isEmployee(source, "pdm")
if role then
  print("Player is a " .. role .. " at PDM")
end
```

***

#### hasPermission

Check if a player has a specific permission at a dealership.

```lua
local allowed = exports['jg-dealerships']:hasPermission(src, dealershipId, permission)
```

| Parameter      | Type   | Required | Description         |
| -------------- | ------ | -------- | ------------------- |
| `src`          | number | Yes      | Player server ID    |
| `dealershipId` | string | Yes      | Dealership ID       |
| `permission`   | string | Yes      | Permission to check |

**Returns:** `boolean`

**Available Permissions:**

| Permission         | Description                                             |
| ------------------ | ------------------------------------------------------- |
| `ADMIN`            | Full access to everything                               |
| `MANAGE_EMPLOYEES` | Hire, fire, and change employee roles                   |
| `MANAGE_INVENTORY` | Order vehicles, manage stock, display vehicles, pricing |
| `MANAGE_FINANCES`  | Access dealership bank and settings                     |
| `SELL`             | Perform direct sales and test drives                    |
| `DELIVER`          | Complete trucking delivery missions                     |
| `VIEW_RECORDS`     | View sales and order history                            |

***

#### getEmployees

Get all employees at a dealership.

```lua
local employees = exports['jg-dealerships']:getEmployees(dealershipId)
```

| Parameter      | Type   | Required | Description   |
| -------------- | ------ | -------- | ------------- |
| `dealershipId` | string | Yes      | Dealership ID |

**Returns:** `table[]` — Array of employee records with `id`, `identifier`, `dealership`, `role`, `joined`.

***

### Locations

#### getDealerships

Get all dealership locations.

```lua
local dealerships = exports['jg-dealerships']:getDealerships()
```

**Returns:** `Location[]` — Array of all dealership location objects with their full configuration.

**Example:**

```lua
local dealerships = exports['jg-dealerships']:getDealerships()
for _, dealership in ipairs(dealerships) do
  print(dealership.id, dealership.name, dealership.type)
end
```

***

#### getDealership

Get a specific dealership by its ID.

```lua
local dealership = exports['jg-dealerships']:getDealership(dealershipId)
```

| Parameter      | Type   | Required | Description          |
| -------------- | ------ | -------- | -------------------- |
| `dealershipId` | string | Yes      | Dealership ID (UUID) |

**Returns:** `Location` or `false` if not found.

***

### Showroom

#### getShowroomVehicles

Get all vehicles available in a dealership's showroom (with current stock and per-dealership pricing).

```lua
local vehicles = exports['jg-dealerships']:getShowroomVehicles(dealershipId)
```

| Parameter      | Type   | Required | Description   |
| -------------- | ------ | -------- | ------------- |
| `dealershipId` | string | Yes      | Dealership ID |

**Returns:** `Vehicle[]` or `false` if dealership not found.

Each vehicle contains: `spawn_code`, `brand`, `model`, `category`, `price` (per-dealership), `stock`, `unlimited_stock`, `global_stock_limit`.

**Example:**

```lua
local vehicles = exports['jg-dealerships']:getShowroomVehicles("pdm")
if vehicles then
  for _, v in ipairs(vehicles) do
    print(v.spawn_code, v.price, v.stock)
  end
end
```

***

### Coupons

#### createCoupon

Programmatically create a coupon for a dealership.

```lua
local coupon, err = exports['jg-dealerships']:createCoupon(dealershipId, data)
```

| Parameter      | Type   | Required | Description                      |
| -------------- | ------ | -------- | -------------------------------- |
| `dealershipId` | string | Yes      | Dealership ID                    |
| `data`         | table  | Yes      | Coupon configuration (see below) |

**Coupon Data:**

| Field                   | Type      | Required | Description                                                           |
| ----------------------- | --------- | -------- | --------------------------------------------------------------------- |
| `code`                  | string    | No       | Custom coupon code. Auto-generated if omitted (`XXXX-XXXX` format)    |
| `discount_type`         | string    | Yes      | `"percent"` or `"fixed"`                                              |
| `discount_value`        | number    | Yes      | Discount amount (percentage or fixed value)                           |
| `max_uses`              | integer   | No       | Maximum total uses                                                    |
| `per_player_limit`      | integer   | No       | Maximum uses per player                                               |
| `expiry_date`           | integer   | No       | Expiry timestamp in milliseconds                                      |
| `vehicle_restrictions`  | string\[] | No       | Array of spawn codes this coupon is valid for                         |
| `category_restrictions` | string\[] | No       | Array of categories this coupon is valid for                          |
| `allow_finance`         | boolean   | No       | Whether coupon can be used with financed purchases (default: `false`) |

**Returns:** `table` coupon object (with `id`, `code`, etc.) or `false`, `string?` error

**Example:**

```lua
local coupon, err = exports['jg-dealerships']:createCoupon("pdm", {
  discount_type = "percent",
  discount_value = 15,
  max_uses = 100,
  per_player_limit = 1,
  category_restrictions = { "super", "sports" },
  allow_finance = false
})

if coupon then
  print("Created coupon: " .. coupon.code)
end
```

***

#### validateCoupon

Validate a coupon code without consuming it. Useful for checking eligibility before a purchase.

```lua
local result = exports['jg-dealerships']:validateCoupon(code, dealershipId, spawnCode, category, isFinanced)
```

| Parameter      | Type    | Required | Description                                        |
| -------------- | ------- | -------- | -------------------------------------------------- |
| `code`         | string  | Yes      | Coupon code                                        |
| `dealershipId` | string  | Yes      | Dealership ID                                      |
| `spawnCode`    | string  | No       | Vehicle spawn code (for vehicle restriction check) |
| `category`     | string  | No       | Vehicle category (for category restriction check)  |
| `isFinanced`   | boolean | No       | Whether the purchase is financed                   |

**Returns:** `table` with:

| Field            | Type    | Description                                                                                     |
| ---------------- | ------- | ----------------------------------------------------------------------------------------------- |
| `valid`          | boolean | Whether the coupon is valid                                                                     |
| `message`        | string? | Error message if invalid                                                                        |
| `discount`       | number? | Calculated discount amount (if `spawnCode` provided and vehicle has a price at this dealership) |
| `discount_type`  | string? | `"percent"` or `"fixed"` (if valid)                                                             |
| `discount_value` | number? | Raw discount value (if valid)                                                                   |

> **Note:** This does NOT consume the coupon or increment its usage count. It is a read-only check.

***

### Sales History

#### getSalesHistory

Get recent sales records for a dealership.

```lua
local sales = exports['jg-dealerships']:getSalesHistory(dealershipId, limit)
```

| Parameter      | Type    | Required | Description                                       |
| -------------- | ------- | -------- | ------------------------------------------------- |
| `dealershipId` | string  | Yes      | Dealership ID                                     |
| `limit`        | integer | No       | Max records to return (default: `50`, max: `500`) |

**Returns:** `table[]` — Array of sale records ordered by most recent first. Each record contains `id`, `dealership`, `vehicle`, `plate`, `player`, `seller`, `purchase_type`, `paid`, `owed`.

***

#### getTotalSales

Get the total revenue (sum of all payments received) for a dealership.

```lua
local total = exports['jg-dealerships']:getTotalSales(dealershipId)
```

| Parameter      | Type   | Required | Description   |
| -------------- | ------ | -------- | ------------- |
| `dealershipId` | string | Yes      | Dealership ID |

**Returns:** `number` — Total revenue amount.
