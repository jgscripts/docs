# ps-housing

Open up `ps-housing/client/cl_property.lua`, find and replace these functions with this code:

```lua
function Property:RegisterGarageZone()

    if not next(self.propertyData.garage_data) then return end

    if not (self.has_access or self.owner) then
        return
    end

    local garageData = self.propertyData.garage_data
    local garageName = string.format("property-%s-garage", self.property_id)

    local data = {
        takeVehicle = {
            x = garageData.x,
            y = garageData.y,
            z = garageData.z,
            w = garageData.h
        },
        type = "house",
        label = self.propertyData.street .. self.property_id .. " Garage",
    }

    self.garageZone = lib.zones.box({
        coords = vec3(garageData.x, garageData.y, garageData.z),
        size = vector3(garageData.length + 5.0, garageData.width + 5.0, 3.5),
        rotation = garageData.h,
        debug = Config.DebugMode,
        onEnter = function()
            if IsPedInAnyVehicle(PlayerPedId(), true) then
                lib.showTextUI('Press [E] to put the vehicle in the garage')
            else 
                lib.showTextUI('Press [E] to open the garage')
            end
        end,
        inside = function()
            if IsControlJustReleased(0, 38) then
                Wait(100)
                inGarage = false
                if IsPedInAnyVehicle(PlayerPedId(), true) then
                    TriggerEvent('jg-advancedgarages:client:InsertVehicle', garageName, true)
                else
                    TriggerEvent('jg-advancedgarages:client:ShowHouseGarage:qs-housing', garageName, true)
                end
            end
        end,
        onExit = function()
            lib.hideTextUI()
        end,
    })
end

function Property:UnregisterGarageZone()
    if not self.garageZone then return end

    self.garageZone:remove()
    self.garageZone = nil
end
```
