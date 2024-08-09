# Garage Events

{% hint style="danger" %}
"GARAGE\_ID"  is the name of the Garage in the Config.GarageLocations

"JOB\_GARAGE\_ID" is the name of the job garage in Config.JobGarageLocations

"IMPOUND\_ID" is the name of the impound in Config.ImpoundLocations
{% endhint %}

<pre class="language-lua"><code class="lang-lua">-- Open garage (personal garages)
TriggerEvent("jg-advancedgarages:client:ShowGarage", "GARAGE_ID", false)

-- Insert vehicle (personal garages)
TriggerEvent("jg-advancedgarages:client:InsertVehicle", "GARAGE_ID", false)

<strong>-- Open garage (job garages)
</strong>TriggerEvent("jg-advancedgarages:client:ShowJobGarage", "JOB_GARAGE_ID")

-- Insert vehicle (job garages)
TriggerEvent("jg-advancedgarages:client:JobGarageInsertVehicle", "JOB_GARAGE_ID")

-- Open garage (gang garages, QBCore only)
TriggerEvent("jg-advancedgarages:client:ShowGangGarage", "GANG_GARAGE_ID")

-- Insert vehicle (gang garages, QBCore only)
TriggerEvent("jg-advancedgarages:client:GangGarageInsertVehicle", "GANG_GARAGE_ID")

-- Open impound
TriggerEvent("jg-advancedgarages:client:ShowImpound", "IMPOUND_ID")

-- Impound vehicle
TriggerEvent("jg-advancedgarages:client:ImpoundVehicle")
</code></pre>
