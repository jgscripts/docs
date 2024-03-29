---
description: >-
  This release improves reliability of vehicle spawning on the server by using
  CreateVehicleServerSetter instead of CreateVehicle.
---

# v.2.2.2

## Upgrade Requirements

* Server build 7290+ (you should do this anyway for an important security update - [download here](https://runtime.fivem.net/artifacts/fivem/build\_server\_windows/master/))
* Game version 2944+ (go to `server.cfg` -> `sv_enforcegamebuild 2944`)

### Potential Issues

There may be an issue with some vehicles or addons as a vehicle type is required. For all base GTA vehicles, I used a mismatch table from [tabarra](https://gist.github.com/tabarra/32ef90524188093ab4218ee7b5121269). An easy way to fix addons are to make sure the class in the `vehicles.meta` of the vehicle is correct. This will only typically be an issue with emergency vehicles, trailers or utility vehicles.

#### Changed Files

* client/cl-main.lua
* server/sv-main.lua

