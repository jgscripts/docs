# Plate Format

{% hint style="warning" %}
Requires Dealerships v1.1.5 or newer
{% endhint %}

When players purchase a new vehicle, you can customise the vehicle license plate that will be generated for them. Your car plate can have letters, numbers, and spaces. **It should be up to 8 characters long.**

## How to Write Your Plate Format

* Use `A` for any random letter.
* Use `1` for any random number.
* Want a specific letter or number always to show up? Put a `^` before it.

## Examples

* **All Random Letters**: **`AAAAAAAA`** gives something like `GHTPAXZQ`.
* **Mix of Letters, Spaces and Numbers**: **`AA11 1AA`** makes something like `GH49 8KJ`.
* **Fixed Letter or Number**: **`^Z123 A^B1`** always has `Z` at the start and `B` in the second part, like `Z456 XB4`.
  * For example in the UK, you would have a fixed year section, and therefore you could use `AA^2^4 AAA` -> which would give something like `MA24 UAW`.

## Quick Steps

1. Open the `config.lua` file
2. Find `Config.PlateFormat`
3. Change it using `A` for letters, `1` for numbers, and `^` for fixed characters.

## Customisation

If you're a developer and would like more control, the function is open source (`Framework.Server.VehicleGeneratePlate`) and can be found in framework/sv-functions.lua
