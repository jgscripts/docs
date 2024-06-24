# Why are you not using CreateVehicleServerSetter by default?

{% hint style="info" %}
This article is for nerds & people who will (rightfully) bully me for not using the recommended native
{% endhint %}

Spawning vehicles on FiveM sucks.

Since the introduction of OneSync, the recommended way to spawn vehicles is via the server, originally with the `CreateVehicle` RPC native. This native had reliability issues, so was replaced with `CreateVehicleServerSetter`. This native is _awesome_ - it doesn't need a nearby client to work, and vehicles will persist after the client has disconnected, so their vehicle will be there when they come back.

But perhaps the best reason for spawning anything via the server, is so that you can use entity lockdown mode ([https://docs.fivem.net/natives/?\_0xA0F2201F](https://docs.fivem.net/natives/?\_0xA0F2201F)). This prevents clients _entirely_ from spawning vehicles, objects, peds and more, rendering many cheat menus useless. No anti-cheat required. Sounds great right?

The problem is setting vehicle properties. Vehicle properties a common name for all the vehicle natives that set colours, performance modifications, extras and more. These can _only_ be used on the client side, and will only work if set by the **networked entity's owner**. This means that in order to set a vehicle's customisation back to how a player had saved a vehicle, you need to send the now server-spawned vehicle's network ID back to the current client owner. Then the entity owner can set all of the necessary properties. Easy... right?

No. Once a vehicle has been created on the server, the entity owner can change. Randomly. As quickly as a tick. This means that in scenarios where there are a lot of different clients nearby to where a server created entity has been spawned, you can't simply ask the first entity owner to run the vehicle properties natives. Instead, developers are using [state bags](https://docs.fivem.net/docs/scripting-manual/networking/state-bags/).

By setting a state bag on the server created entity with a table of all the vehicle properties, the data can be sent to _every_ nearby client. This means that all nearby clients can check if they are the owner as they receive it, and attempt to set the vehicle's properties. If they can successfully set the vehicle's properties, they clear the state bag so other clients & future clients won't attempt to set vehicle properties any further.

The problem from earlier still stands though. "_The entity owner can change. Randomly. As quickly as a tick."_ This means that on populated servers, where a lot of vehicles are being spawned, and there are a lot of clients nearby, it can be almost impossible to get a client to reliably set vehicle properties. Vehicles will therefore commonly take a long time to successfully create as the entity owner bounces around, or vehicles simply spawn with the wrong vehicle properties and server owners, rightfully, get ignored.

This isn't a problem on quiet servers, with up to maybe, 25 people. For some reason, Advanced Garages became popular, and was being used on servers with hundreds of active players, and a lot of people started to complain about the reliability of our script. "Why it doesn't work like XYZ garage script", or "I'm switching to XYZ garage because yours is terrible" (which just spawns vehicles on the client).

Therefore, by default, we don't use `CreateVehicleServerSetter`. You have to enable it by setting:

```lua
Config.SpawnVehiclesWithServerSetter = true
```

I _recommend_ setting it to true, and I have it set to true on my servers. I simply had no choice but to turn it off by default, as many server owners just want something that works 100% of the time and aren't interested in the benefits of an alternative method.

If I'm wrong about any of this, or you have any solutions, PLEASE tell me. I will love you forever.

Don't hate me plz.
