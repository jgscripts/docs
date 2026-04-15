# Client API

#### openShowroom

Open the dealership showroom UI for a player.

```lua
exports['jg-dealerships']:openShowroom(dealershipId, defaultVehicle, defaultColor)
```

| Parameter        | Type          | Required | Description                                     |
| ---------------- | ------------- | -------- | ----------------------------------------------- |
| `dealershipId`   | string        | Yes      | Dealership ID to open                           |
| `defaultVehicle` | string        | No       | Pre-select a vehicle by spawn code              |
| `defaultColor`   | number\|table | No       | Pre-select a color (paint index or `{r, g, b}`) |

***

#### exitShowroom

Close the showroom UI and clean up resources.

```lua
exports['jg-dealerships']:exitShowroom()
```
