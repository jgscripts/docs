# Liveries not saving

{% hint style="warning" %}
This is a fix for Benny's (`qb-customs`), not JG Advanced Garages. You will need to take your vehicle to Benny's and re-apply the livery for your existing vehicles in order for this to take effect!
{% endhint %}

This is an issue that can be fixed by making a small modification to `qb-core`

1. Go to `[qb]/qb-core/client/functions.lua`
2. Search (by pressing CTRL+F) for the code below and remove it:

```lua
and GetVehicleLivery(vehicle) ~= 0
```

_The function you modified should now look like this:_

<pre class="language-lua"><code class="lang-lua"><strong>local modLivery = GetVehicleMod(vehicle, 48)
</strong>if GetVehicleMod(vehicle, 48) == -1 then
   modLivery = GetVehicleLivery(vehicle)
end
</code></pre>

