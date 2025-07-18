# Third-Party Seatbelts

{% hint style="success" %}
The seatbelt key bind, button in the F6 vehicle control menu, sound effects, and indicator light within JG HUD will still work when using a third party seatbelt integration!
{% endhint %}

1. Within `config.lua`, set `Config.UseCustomSeatbeltIntegration = true`. Ensure that `Config.EnableSeatbelt = true`, or it won't work.&#x20;
2. Head to `framework/cl-functions.lua`, and find `Framework.Client.ToggleSeatbelt`. There are two parameters, `vehicle` & `seatbeltOn`. `seatbeltOn` will be equal to true if the seatbelt has been enabled, otherwise it will be false. If you have multiple exports, one for enabling the seatbelt and one for disabling, you will have to use `seatbeltOn` in an if statement; for example:

```lua
if seatbeltOn then
  exports["my_seatbelt_script"]:enableSeatbelt()
else
  exports["my_seatbelt_script"]:disableSeatbelt()
end
```
